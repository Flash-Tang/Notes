# FLINK NOTES
## Anatomy of a Flink Program
Flink programs look like regular programs that transform DataStreams. Each program consists of the same basic parts:

1. Obtain an execution environment,
2. Load/create the initial data,
3. Specify transformations on this data,
4. specify where to put the results of your computations,
5. Trigger the program execution

All Flink programs are **executed lazily**: When the program’s main method is executed, the data loading and transformations do not happen directly. Rather, each operation is created and added to a dataflow graph. The operations are actually executed when the execution is explicitly triggered by an `execute()` call on the execution environment. 

## Back Pressure

Internally, **back pressure** is judged based on the availability of output buffers. If a task has no available output buffers, then that task is considered back pressured. Idleness, on the other hand, is determined by whether or not there is input available.

## Kafka Offset

Kafka source commits the current consuming offset when checkpoints are **completed**, for ensuring the consistency between Flink’s checkpoint state and committed offsets on Kafka brokers.

## Parallel Execution

A Flink program consists of multiple **tasks (transformations/operators, data sources, and sinks)**. A task is split into several parallel instances for execution and each parallel instance processes a subset of the task’s input data. The number of parallel instances of a task is called its *parallelism*.

## Task and Operator Chains

For distributed execution, Flink *chains* operator subtasks together into *tasks*. Each task is executed by one thread. The sample dataflow in the figure below is executed with five subtasks, and hence with five parallel threads.

![](https://nightlies.apache.org/flink/flink-docs-release-1.17/fig/tasks_chains.svg)

## Task Slots and Resources

Each worker (TaskManager) is a *JVM process*, and may execute one or more subtasks in separate threads. To control how many tasks a TaskManager accepts, it has so called **task slots** (at least one).

Each *task slot* represents a fixed subset of resources of the TaskManager. A TaskManager with three slots, for example, will dedicate 1/3 of its managed memory to each slot. Slotting the resources means that a subtask will not compete with subtasks from other jobs for managed memory, but instead has a certain amount of reserved managed memory. Note that no CPU isolation happens here; currently slots only separate the managed memory of tasks.

**By default, Flink allows subtasks to share slots even if they are subtasks of different tasks, so long as they are from the same job. The result is that one slot may hold an entire pipeline of the job.**

Without slot sharing, the non-intensive *source/map()* subtasks would block as many resources as the resource intensive *window* subtasks. 

![](https://nightlies.apache.org/flink/flink-docs-release-1.17/fig/tasks_slots.svg)

With slot sharing, increasing the base parallelism in our example from two to six yields full utilization of the slotted resources, while making sure that the heavy subtasks are fairly distributed among the TaskManagers.

![](https://nightlies.apache.org/flink/flink-docs-release-1.17/fig/slot_sharing.svg)

![image-20240511112533915](/Users/tanghao/Library/Application Support/typora-user-images/image-20240511112533915.png)

## ProcessFunction

The `ProcessFunction` can be thought of as a `FlatMapFunction` with access to keyed state and timers. It handles events by being invoked for each event received in the input stream(s).

## Metrics
### Registering metrics
You can access the metric system from any user function that extends `RichFunction` by calling `getRuntimeContext().getMetricGroup()`. This method returns a MetricGroup object on which you can create and register new metrics.

### Metric types
Flink supports **Counters, Gauges, Histograms and Meters**.

## Windows
windows将流拆分为有限大小的”桶“

**Keyed Windows**

Having a keyed stream will allow your windowed computation to be performed in parallel by multiple tasks, as each logical keyed stream can be processed independently from the rest. All elements referring to the same key will be sent to the same parallel task.

### Session Windows

与滚动窗口和滑动窗口相比，会话窗口不重叠并且没有固定的开始和结束时间，会话窗口在一段时间内没有接收到元素时关闭。

# KAFKA NOTES

+ Kafka is run as a cluster of one or more servers that can span multiple datacenters or cloud regions. Some of these servers **form the storage layer, called the brokers.** Other servers run [Kafka Connect](https://kafka.apache.org/documentation/#connect) to continuously import and export data as event streams to integrate Kafka with your existing systems such as relational databases as well as other Kafka clusters. 
+ Topics are **partitioned**, meaning a topic is spread over a number of "buckets" located on different Kafka brokers. This distributed placement of your data is very important for scalability because it allows client applications to both read and write the data from/to many brokers at the same time. When a new event is published to a topic, it is actually appended to one of the topic's partitions. Events with the same event key (e.g., a customer or vehicle ID) are written to the same partition, and **Kafka [guarantees](https://kafka.apache.org/documentation/#semantics) that any consumer of a given topic-partition will always read that partition's events in exactly the same order as they were written.**
+ To make your data fault-tolerant and highly-available, every topic can be **replicated**, even across geo-regions or datacenters, so that there are always multiple brokers that have a copy of the data just in case things go wrong, you want to do maintenance on the brokers, and so on. 
+ Kafka 0.11.0 includes support for **idempotent** and **transactional** capabilities in the producer. Idempotent delivery ensures that messages are delivered exactly once to a particular topic partition during the lifetime of a single producer. Transactional delivery allows producers to send data to multiple partitions such that either all messages are successfully delivered, or none of them are. Together, these capabilities enable "exactly once semantics" in Kafka.