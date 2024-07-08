# Spark Notes

## SparkSession

The entry point into all functionality in Spark is the [`SparkSession`](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/sql/SparkSession.html) class. To create a basic `SparkSession`, just use `SparkSession.builder()`:

```scala
import org.apache.spark.sql.SparkSession

val spark = SparkSession
  .builder()
  .appName("Spark SQL basic example")
  .config("spark.some.config.option", "some-value")
  .getOrCreate()
```

## Spark SQL

Spark SQL is a Spark module for structured data processing. Unlike the basic Spark RDD API, the interfaces provided by Spark SQL provide Spark with more information about the structure of both the **data** and the **computation** being performed.

One use of Spark SQL is to execute SQL queries. Spark SQL can also be used to read data from an existing Hive installation. The `sql` function on a `SparkSession` enables applications to run SQL queries programmatically and returns the result as a `DataFrame`.

## Datasets and DataFrames

A Dataset is a distributed collection of data. Dataset is a new interface added in Spark 1.6 that provides the benefits of RDDs (strong typing, ability to use powerful lambda functions) with the benefits of Spark SQL’s optimized execution engine.

A DataFrame is a *Dataset* organized into named columns. It is conceptually equivalent to a **table** in a relational database or a data frame in R/Python, but with richer optimizations under the hood. DataFrames can be constructed from a wide array of sources as: structured data files, tables in Hive, external databases, or existing RDDs. 

```scala
val df = spark.read.json("examples/src/main/resources/people.json")

// Displays the content of the DataFrame to stdout
df.show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+

df.printSchema()
// root
// |-- age: long (nullable = true)
// |-- name: string (nullable = true)

// Register the DataFrame as a SQL temporary view
df.createOrReplaceTempView("people")

val sqlDF = spark.sql("SELECT * FROM people")
sqlDF.show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+
```

## RDD to DataFrame

1. `toDF()`

   RDD is schema-less without column names and data type, converting from RDD to DataFrame with `toDF()` gives you default column names as `_1`, `_2` and so on and data type as String.

2. Using `createDataFrame()` with the Row type

   `createDataFrame()` has another signature which takes the RDD[Row] type and schema for column names as arguments. To use this first we need to convert our “rdd” object from `RDD[T]` to `RDD[Row]` and define a [schema using StructType & StructField](https://sparkbyexamples.com/spark/spark-sql-structtype-on-dataframe/).

   ```scala
   import org.apache.spark.sql.types.{StringType, StructField, StructType}
   import org.apache.spark.sql.Row
   val schema = StructType( Array(
                    StructField("language", StringType,true),
                    StructField("users", StringType,true)
                ))
   val rowRDD = rdd.map(attributes => Row(attributes._1, attributes._2))
   val dfFromRDD3 = spark.createDataFrame(rowRDD,schema)
   ```

   