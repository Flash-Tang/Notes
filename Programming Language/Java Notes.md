# Java Notes

[TOC]

## Process

+ [x] Java程序基础
+ [x] 流程控制
+ [x] 数组操作
+ [x] 面向对象基础
+ [x] Java核心类
+ [x] 集合
+ [ ] 泛型
  + [ ] 泛型和反射

+ [ ] 单元测试
  + [ ] 异常测试


## Basic

+ 一个Java源码只能定义**一个**`public`类型的class，并且class名称和文件名要完全一致；

+ ```java
  // 单行注释
  
  /*
  多行
  注释
  */
  
  /**
  *用于自动创建文档的
  *特殊多行注释
  */
  ```

+ 使用`javac`可以将`.java`源码编译成`.class`字节码;

  使用`java`可以运行一个已编译的Java程序，参数是类名。

+ switch 语法

  ```java
  switch (整型/字符串/枚举) {
    case "apple":
      System.out.println("selected apple");
      break;
    case "mango":
      System.out.println("selected mango");
      break;
    default:
      System.out.println("no fruit selected");
      break;
  }
  ```

+ 三元运算符`b ? x : y`，它根据第一个布尔表达式的结果，分别返回后续两个表达式之一的计算结果。

+ `import`

  + `import org.example.*`会导入改包下的所有class，但不会引入子包的class。

  + `import static`可以引入一个类的**静态字段**和**静态方法**。
  
+ 在开发阶段，多个版本的JDK可以同时安装，当前使用的JDK版本可由`JAVA_HOME`环境变量切换。

+ classpath: JVM搜索class的路径集合，设置classpath: `java -classpath(-cp) . abc.xyz.Hello `

+ jar包

  + 压缩文件，可以包含很多`.class`文件，方便下载和使用。
  + `MANIFEST.MF`文件可以提供jar包的信息，如`Main-Class`，这样可以直接运行jar包。
  + 运行包含主类的jar包，`java -jar hello.jar`
  

### 数据类型

+ 二进制`0b1111`，十六进制`0xf`。

+ `\u####` 转义字符表示一个Unicode编码的字符

+ 对于`float`类型，需要加上`f`后缀; long类型需要加上`l`后缀。

+ 强制转型使用`(类型) 变量`。

  ```java
  double d = 3.14;
  int i = (int) d; // 将 double 类型的 d 转换为 int 类型
  System.out.println(i); // 输出: 3
  ```

  

+ 定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量。常量在定义时进行初始化后就不可再次赋值，再次赋值会导致编译错误。

+ Java的`String`和`char`在内存中总是以Unicode编码表示，要显示一个字符的Unicode编码，只需将`char`类型直接赋值给`int`类型即可：

  ```java
  int n1 = 'A'; // 字母“A”的Unicodde编码是65
  ```


### Integer包装类

+ 创建`Integer`实例

  ```java
  int i = 100;
    // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
    Integer n1 = new Integer(i);
    // 通过静态方法valueOf(int)创建Integer实例:
    Integer n2 = Integer.valueOf(i);
    // 通过静态方法valueOf(String)创建Integer实例:
    Integer n3 = Integer.valueOf("100");
  ```

+ Auto Boxing，Java编译器可以帮助我们自动在`int`和`Integer`之间转型

  ```java
  Integer n = 100; // 编译器自动使用Integer.valueOf(int)
  int x = n; // 编译器自动使用Integer.intValue()
  ```

  

### Math类常用静态方法

```java
Math.abs(Math.PI);
Math.min(x, y);
Math.max(x, y);
double x = Math.random(); 0 <= x < 1
```



### 数组

+ 定义数组时初始化：

  ```java
  int[] nums = new int[] {1, 2, 3, 4};
  // 可进一步简化为：
  int[] nums = {1, 2, 3, 4};
  ```

+ 获取数组大小：`数组变量.length`

+ 快速打印数组内容：

  ```java
  System.out.println(Arrays.toString(数组变量));
  // 多维数组
  System.out.println(Arrays.deepToString(多维数组))；
  ```

  

+ 数组排序：`Arrays.sort(数组变量)`

### enum

定义`Color`枚举类：

```java
public enum Color {
    RED, GREEN, BLUE;
}
```

编译器编译出的`class`大概就像这样：

```java
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```

- 定义的`enum`类型总是继承自`java.lang.Enum`，且无法被继承；
- 只能**定义出**`enum`的实例，而无法通过`new`操作符创建`enum`的实例；
- 定义的每个实例都是引用类型的唯一实例；
- `name()`方法：返回枚举常量名。`Color.RED.name() // "RED"`

**定义`private`构造方法，给每个枚举常量绑定一个值**

```java
enum Weekday {
    MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);

    public final int dayValue;

    private Weekday(int dayValue) {
        this.dayValue = dayValue;
    }
}
```

## 字符串

+ 如果用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串，再连接。
+ 在Java中，判断值类型的变量是否相等，可以使用`==`运算符。但是，判断引用类型的变量是否相等，`==`表示“引用是否相等”，或者说，是否指向同一个对象。引用类型判断变量内容是否相等要使用`equals()`，注意避免`NullPointerException`。推荐写法：`if ("hello".equals(s)) { ... }`。

### 字符串不可变

```java
String s = "hello";
String t = s;
t = "world"; // JVM虚拟机先创建字符串"world"，然后，把字符串变量t指向它：
System.out.println(s); // "hello"
```

+ 字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的。因为`String`太常用了，所以Java提供了`"..."`这种字符串字面量表示方法。

  ```java
  String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
  ```

+ 不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

### 字符串常用方法

```java
"hello".contains("ll"); // true
"hello".startsWith("he"); // true
"hello".endsWith("lo"); // true
"hello".substring(2); // "llo"
"hello".charAt(0); // 'h'

// 剔除首尾空白符
"   \tHello\r\n ".trim(); // "Hello"

// 判断字符串是否为空
"".isEmpty(); // true (Returns true if, and only if, length() is 0)

// 替换子串
String replace(char oldChar, char newChar);
String replace(CharSequence target, CharSequence replacement);
String replaceAll(String regex, String replacement);

// 分割字符串
String[] split(String regex);
String[] split(String regex, int limit)
  
// 使用指定分隔符拼接字符串
String.join(CharSequence delimiter, CharSequence... elements);
String.join(CharSequence delimiter, Iterable<? extends CharSequence);
// 或者使用StringJoiner，可以额外指定前缀和后缀
StringJoiner sj = new StringJoiner(CharSequence delimiter, CharSequence prefix, CharSequence suffix)

// 格式化字符串
String.format()
  
// 把其他类型转换为字符串
String.valueOf(123); // "123"

// 把字符串解析为整数
Integer.parseInt("100", 2); // 将“100”按二进制转化为整数
```



## Maven

+ 使用Maven构建项目就是执行lifecycle，执行到指定的phase为止。每个phase会执行自己默认的一个或多个goal，goal是最小执行单元。phase通过调用插件执行关联的goal。

## 函数式编程

+ 可变参数
  + `类型... 变量名`；
  + 调用时可以不传参数（零个参数）；
  + 相比使用数组作为方法参数，调用方不需要构造数组，且可变参数可以保证无法传入null（传入0个参数时，接收到的实际值是一个空数组而不是null）
  
+ **Lambda表达式**
  
  + 接收单方法接口`FunctionalInterface`作为参数的时候，可以把实例化的匿名类改写为Lambda表达式，能大大简化代码。
  
  + Lambda表达式的写法：
  
    ```
    (s1, s2) -> {
        return s1.compareTo(s2);
    }
    ```
  
    如果只有一行`return xxx`的代码，可以用更简单的写法：
  
    ```
    Arrays.sort(array, (s1, s2) -> s1.compareTo(s2));
    ```
  
    其中，参数是`(s1, s2)`，参数类型可以省略，因为编译器可以自动推断出`String`类型，返回值的类型也是由编译器自动推断的。`-> { ... }`表示方法体，所有代码写在内部即可。

## 面向对象

+ 类中访问当前实例（`this`）的字段，在没有命名冲突时，可以省略`this`。

  ```java
  // 有命名冲突时，不能省略this
  public void setName(String name) {
          this.name = name; // 局部变量优先级更高。
  }
  ```

+ `super`关键字表示父类（超类）。

  + 子类引用父类的字段时，可以用`super.fieldName`。

  + 在子类的覆写方法中，如果要调用父类的被覆写的方法，可以通过`super.foo()`来调用

### 构造函数

  + 类外调用构造方法，必须用`new`操作符。

  + 如果一个类没有定义构造方法，编译器会自动为我们生成一个没有参数，也没有执行语句的默认构造方法；

  + 如果自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法，**若要使用不带参数的构造方法，需要自行定义**。

  + 一个构造方法调用其他构造方法的语法是`this(…)`。

  + 子类的构造方法
    + 子类的构造方法可以通过`super()`调用父类的构造方法；

    + 在Java中，任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`

    + 如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。

    + 即子类*不会继承*任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

### `protected` 访问权限

  + `protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问。

  + `protected`和`public`区别：protected字段不能在**不同包的非子类**中被访问。

    ```java
    package p1;
    
    public class A {
      protected int num;
    }
    ```

    ```java
    package p2;
    public class B {
      A a = new A();
      System.out.println(a.num); // error，若为同一package则没问题
    }
    ```

    

### 向下转型

  + 由于向下转型可能会失败，Java提供了`instanceof`操作符，向下转型前可以先判断一个实例究竟是不是某种类型。

    ```java
    Person p = new Student();
    if (p instanceof Student) {
        // 只有判断成功才会向下转型:
        Student s = (Student) p; // 一定会成功
    }
    ```

  + `instanceof`实际上判断一个变量所指向的实例是否是指定类型，或者这个类型的子类。

  + 如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

    ```java
    Student n = null;
    System.out.println(n instanceof Student); // false
    ```


### 多态

  + 实例的方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。

    ```java
    public class Main {
        public static void main(String[] args) {
            // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
            Income[] incomes = new Income[] {
                new Income(3000),
                new Salary(7500),
                new StateCouncilSpecialAllowance(15000)
            };
            System.out.println(totalTax(incomes));
        }
    
        public static double totalTax(Income... incomes) {
            double total = 0;
            for (Income income: incomes) {
                total = total + income.getTax();
            }
            return total;
        }
    }
    
    class Income {
        protected double income;
    
        public Income(double income) {
            this.income = income;
        }
    
        public double getTax() {
            return income * 0.1; // 税率10%
        }
    }
    
    class Salary extends Income {
        public Salary(double income) {
            super(income);
        }
    
        @Override
        public double getTax() {
            if (income <= 5000) {
                return 0;
            }
            return (income - 5000) * 0.2;
        }
    }
    
    class StateCouncilSpecialAllowance extends Income {
        public StateCouncilSpecialAllowance(double income) {
            super(income);
        }
    
        @Override
        public double getTax() {
            return 0;
        }
    }
    
    ```

    利用多态，`totalTax()`方法只需要和`Income`打交道，它完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在，就可以正确计算出总的税。如果我们要新增一种稿费收入，只需要从`Income`派生，然后正确覆写`getTax()`方法就可以。把新的类型传入`totalTax()`，不需要修改任何代码。

  + 多态的作用：允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

### final

  + final修饰的变量的值不可变，重新赋值会导致编译错误。

  + 修饰引用类型时，也就是不能更改引用类型变量的指向，但是可以操作变量指向的对象本身，比如可以向final map中增删数据。

  + 修饰class时，表示该类不能被继承。

  + 修饰类的字段时，表示该字段在初始化后不能被修改。

    ```java
    // 可以在构造方法中初始化final字段，这种方法更为常用，因为可以保证实例一旦创建，其final字段就不可修改。
    class Person {
      	// public final String name = "some name";
        // or
        public final String name;
        public Person(String name) {
            this.name = name;
        }
    }
    ```

  + 修饰类的方法时，表示该方法不能被override。
### 抽象类

  + 如果父类的方法本身不需要实现任何功能或者没有实际意义，仅仅是为了**定义方法签名**，目的是让子类去**覆写(override)**它，那么，可以把父类的方法声明为抽象方法。
  + 抽象方法用`abstract`修饰，因为无法执行抽象方法，因此**这个类也必须申明为抽象类**（abstract class）。使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类，只能被继承。
  + 因为抽象类本身被设计成**只能用于被继承**，因此，抽象类可以**强迫子类实现其定义的抽象方法**，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。
  + 如果不实现抽象方法，则该子类仍是一个抽象类。
  + 抽象类的作用：抽象类用于建立类的层次结构和继承关系。它可以作为一个**模板类**，定义通用的属性和方法，而将一些特定的实现细节留给子类。

### Interface

  + 就是比抽象类还要抽象的纯抽象接口，因为它**连字段都不能有**。因为接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。
  + 当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字，一个类可以同时实现多个`interface`。
  + 接口的`default`方法：默认方法可以不强制重写，也不会影响到已有的实现类。
  + `interface`可以有静态字段，并且静态字段必须为`final`类型，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，编译器会自动把该字段变为`public static final`类型。
### 内部类

Java的内部类可分为Inner Class、Anonymous Class和Static Nested Class三种，内部类都能访问outer class的`private`字段和方法。

  - Inner Class和Anonymous Class本质上是相同的，**都必须依附于Outer Class的实例**，即隐含地持有`Outer.this`实例，并拥有Outer Class的`private`访问权限。
  
  - Static nested class**不再依附于`Outer`的实例**，而是一个完全独立的类，因此无法引用`Outer.this`。

#### Inner class
+ 被编译为`Outer$Inner.class`。
  
    ```java
    class Outer {
        private String name;
    
        Outer(String name) {
            this.name = name;
        }
    
        class Inner {
            void hello() {
                System.out.println("Hello, " + Outer.this.name);
            }
        }
    }
    ```
  
    要实例化一个`Inner`，我们必须**首先创建一个`Outer`的实例**，然后，调用`Outer`实例的`new`来创建`Inner`实例：
  
    ```java
    Outer outter = new Outer();
    Outer.Inner inner = outer.new Inner();
    ```

#### Anonymous class
+ 被编译为`Outer$1.class, Outer$2.class...`
  
    匿名类不需要在Outer Class中明确地定义class，而是定义在方法内部。匿名类不关心类名，比直接定义Inner Class可以少写很多代码。
  
    ```java
    class Outer {
        private String name;
    
        Outer(String name) {
            this.name = name;
        }
    
        void asyncHello() {
    // Runnable本身是接口，接口是不能实例化的，所以这里实际上是定义了一个实现了Runnable接口的匿名类，并且通过new实例化该匿名类，然后转型为Runnable。在定义匿名类的时候就必须实例化它。
            Runnable r = new Runnable() {
                @Override
                public void run() {
                    System.out.println("Hello, " + Outer.this.name);
                }
            };
            new Thread(r).start();
        }
    }
    ```
  
    定义匿名类语法
  
    ```java
    Runnable r = new Runnable() {
      // 实现必要的抽象方法
    }
    ```
  
    

#### Static nested class
+ 仅能访问Outer class的静态字段和方法

+ ```java
  class Outer {
          private static String NAME = "OUTER";
      
          private String name;
          
          Outer(String name) {
              this.name = name;
          }
          
          static class StaticNested {
              void hello() {
                  System.out.println("Hello, " + Outer.NAME);
              }
          }
      }
  ```
  
+ 实例化

  ```java
  Outer.StaticNested sn = new Outer.StaticNested();
  ```

​    

## 泛型

+ 语法

  ```java
  List<String> list = new ArrayList<String>();
  // 等价于
  // 可以省略后面的String，编译器可以自动推断泛型类型
  List<String> list = new ArrayList<>();
```

+ 静态方法不能引用泛型类型`<T>`，必须定义其他类型（例如`<K>`）来实现静态泛型方法，这样才能清楚地将静态方法的泛型类型和实例类型的泛型类型区分开。

  ```java
  public class Pair<T> {
      private T first;
      private T last;
      public Pair(T first, T last) {
          this.first = first;
          this.last = last;
      }
      public T getFirst() { ... }
      public T getLast() { ... }
  
      // 静态泛型方法应该使用其他类型区分:
      public static <K> Pair<K> create(K first, K last) {
          return new Pair<K>(first, last);
      }
  }
```

+ 泛型限制：

  1. `<T>`不能是基本类型；

  2. 无法取得带泛型的`Class`

     + 所有泛型实例，无论`T`的类型是什么，`getClass()`返回同一个`Class`实例，因为编译后它们全部都是`Pair<Object>`。

  3. 无法判断带泛型的类型

     ```java
     Pair<Integer> p = new Pair<>(123, 456);
     // Compile error:
     if (p instanceof Pair<String>) {
     }
     ```

  4. 不能实例化`T`类型，要实例化`T`类型，我们必须借助额外的`Class<T>`参数：

     ```java
     public class Pair<T> {
         private T first;
         private T last;
         public Pair(Class<T> clazz) {
             first = clazz.newInstance();
             last = clazz.newInstance();
         }
     }
     ```

     上述代码借助`Class<T>`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class<T>`。例如：

     ```java
     Pair<String> pair = new Pair<>(String.class);
     ```

### 上界通配符

+ 使用类似`<? extends Number>`通配符作为方法参数时表示：

  - 方法内部可以调用获取`Number`引用的方法，例如：`Number n = obj.getFirst();`；

  - 方法内部无法调用传入`Number`引用的方法（`null`除外），例如：`obj.setFirst(Number n);`。

​		即一句话总结：使用`extends`通配符表示可以读，不能写。

+ 使用类似`<T extends Number>`定义泛型类时表示：
  - 泛型类型限定为`Number`以及`Number`的子类。

### 下界通配符

+ 使用类似`<? super Integer>`通配符作为方法参数时表示：

  - 方法内部可以调用传入`Integer`引用的方法，例如：`obj.setFirst(Integer n);`；

  - 方法内部无法调用获取`Integer`引用的方法（`Object`除外），例如：`Integer n = obj.getFirst();`。

​		即使用`super`通配符表示只能写不能读。

## 线程同步

  + Java语言内置了多线程支持：一个Java程序实际上是一个JVM进程，JVM进程用一个主线程来执行`main()`方法，在`main()`方法内部，我们又可以启动多个线程。此外，JVM还有负责垃圾回收的其他工作线程等。

  + 线程安全

    + 如果一个类被设计为允许多线程正确访问，我们就说这个类就是“线程安全”的（thread-safe）。
    + 一个类默认是非线程安全的 。

  + Java程序使用`synchronized`关键字对一个对象进行加锁，`synchronized`保证了代码块在任意时刻最多只有一个线程能执行。

    ```java
    synchronized(lockObject) { // 获取锁
        ...
    } // 释放锁
    ```

  + **同步方法**：当我们锁住的是`this`实例时，实际上可以用`synchronized`修饰这个方法。下面两种写法是等价的：

    ```java
    public void add(int n) {
        synchronized(this) { // 锁住this
            count += n;
        } // 解锁
    }
    
    public synchronized void add(int n) { // 锁住this
        count += n;
    } // 解锁
    ```

    用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁。

## ClassLoader

+ ClassLoader

    + Java的ClassLoader（类加载器）是Java虚拟机（JVM）的一个重要组成部分，它负责将类的字节码加载到内存中。ClassLoader提供了一种动态加载类的机制，使得Java应用程序能够在运行时根据需要加载新的类。

    + ClassLoader的主要任务是根据类的全限定名（包括包名和类名）查找类的字节码文件，并将其加载到JVM的内存中。加载完成后，ClassLoader会生成一个对应的Class对象，开发人员可以通过该对象获取类的信息并使用它创建实例、调用方法等操作。

        ClassLoader还具有类加载的双亲委派模型。根据这个模型，当一个类加载器收到加载类的请求时，它会首先将该请求委派给其父加载器。父加载器会继续将请求委派给它的父加载器，直到Bootstrap ClassLoader。只有当父加载器无法加载该类时，子加载器才会尝试加载类。这种机制保证了类的加载是从上到下的，避免了重复加载和类的冲突。

+ Class类

  + `class`是由JVM在执行过程中动态加载的。JVM在第一次读取到一种`class`类型时，将其加载进内存。每加载一种`class`，JVM就为其创建一个`Class`类型的实例，并关联起来。注意：这里的`Class`类型是一个名叫`Class`的`class`。它长这样：

    ```
    public final class Class {
        private Class() {}
    }
    ```

    以`String`类为例，当JVM加载`String`类时，它首先读取`String.class`文件到内存，然后，为`String`类创建一个`Class`实例并关联起来：

    ```
    Class cls = new Class(String);
    ```

    所以，JVM持有的每个`Class`实例都指向一个数据类型（`class`或`interface`），一个`Class`实例包含了该`class`的所有完整信息。

  + 在 Java 中，每个对象都有一个与之关联的**运行时类（Runtime Class）**。运行时类是在对象创建时由 Java 虚拟机动态生成的，并且在对象的整个生命周期中保持不变。运行时类包含了对象的数据和方法定义，以及其他与类相关的信息。

    **运行时类是 `Class` 类的实例**。`Class` 类是 Java 反射 API 的一部分，它提供了许多有关类的信息和操作的方法。通过运行时类，我们可以获取类的名称、父类、实现的接口、字段、方法等信息，并且可以在运行时动态地操作类的成员。
    
  + 获取一个`class`的`Class`实例的三个方法：

    方法一：直接通过一个`class`的静态变量`class`获取：

    ```
    Class cls = String.class;
    ```

    方法二：如果我们有一个实例变量，可以通过该实例变量提供的`getClass()`方法获取：

    ```
    String s = "Hello";
    Class cls = s.getClass();
    ```

    方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

    ```
    Class cls = Class.forName("java.lang.String");
    ```

## 注解

+ `@Override`
  + 加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。希望进行覆写，但是不小心写错了方法签名，编译器会报错。
  + 但是`@Override`不是必需的。

## 集合

### List

  + 如果一个Java对象可以在内部持有若干其他Java对象，并对外提供访问接口，我们把这种Java对象称为集合。

  + 使用迭代器`Iterator`来访问`List`。`Iterator`本身也是一个对象，但它是由`List`的实例调用`iterator()`方法的时候创建的。`Iterator`对象知道如何遍历一个`List`，并且不同的`List`类型，返回的`Iterator`对象实现也是不同的，但总是具有最高的访问效率。

    ```java
    List<String> list = List.of("apple", "pear", "banana");
    for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
      String s = it.next();
      System.out.println(s);
    }
    ```

    Java的`for each`循环本身就可以帮我们使用`Iterator`遍历，Java编译器本身并不知道如何遍历集合对象，但它会自动把`for each`循环变成`Iterator`的调用。

    ```java
    List<String> list = List.of("apple", "pear", "banana"); // List.of()会创建一个不能增删元素的只读了list
    for (String s : list) {
      System.out.println(s);
    }
    ```

  + List <=> Array

    ```java
    // List => Array
    Integer[] array = list.toArray(new Integer[list.size()]);
    // Array => List
    List<Integer> list = Arrays.asList(array); // 注意array和list的改动会相互影响
    
    // 快速创建不可变(fixed-size)List
    // This method also provides a convenient way to create a // fixed-size list initialized to contain several elements:
    List<String> stooges = Arrays.asList("Larry", "Moe", "Curly");
    // 可进一步构建可变List
    stooges = new ArrayList(stooges);
    ```

+ **List元素覆写`equals`方法**

  + 当需要使用List的`contains(Object o)`和`indexOf(Object o)`方法时，List元素需要覆写`equals	`方法，以比较两个元素是否相等。`String`, `Integer`等类型已经覆写了`equals`方法。

  + `equals()`方法的正确编写方法：

    1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
    2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
    3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

    使用`Objects.equals()`比较两个引用类型是否相等的目的是省去了判断`null`的麻烦。两个引用类型都是`null`时它们也是相等的。

  + ```java
    public boolean equals(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o;
            return Objects.equals(this.name, p.name) && this.age == p.age;
        }
        return false;
    }
    ```

### Map

+ Map遍历可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。

  ```java
  Map<String, Integer> map = new HashMap<>();
          map.put("apple", 123);
          map.put("pear", 456);
          map.put("banana", 789);
          for (String key : map.keySet()) {
              Integer value = map.get(key);
              System.out.println(key + " = " + value);
          }
  				// or
  				for (Map.Entry<String, Integer> entry : map.entrySet()) {
              String key = entry.getKey();
              Integer value = entry.getValue();
              System.out.println(key + " = " + value);
          }
  ```

+ Map定义即初始化写法

  ```java
  // mp是一个继承自HashMap的匿名类实例，并且添加了static代码块来初始化数据。
  Map<Integer, String> mp = new HashMap<Integer, String>() {
    {
      put(1, "alan");
      put(2, "jack");
    }
  }
  ```

  
  
+ **Map的key的对象覆写`equals()`和`hashcode()`方法**

  + 覆写`equals()`方法，保证内容相等的key对象返回相同的value。

    ```java
    Map<Student, Integer> mp = new HashMap<>();
    Student s1 = new Student("ming");
    mp.put(s1, 1);
    Student s2 = new Student("ming");
    mp.get(s2); // 若Student类没有覆写equals方法，返回null
    ```

    

  + 通过`key`计算索引的方式就是调用`key`对象的`hashCode()`方法，它返回一个`int`整数。`HashMap`正是通过这个方法直接定位`key`对应的`value`的索引，继而直接返回`value`。

    因此，正确使用`Map`必须保证：

    1. 作为`key`的对象必须正确覆写`equals()`方法，内容相等的两个`key`实例调用`equals()`必须返回`true`；
    2. 作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：

    - 如果两个对象相等，则两个对象的`hashCode()`必须相等；
    - 如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等。

  + ```java
    public class Person {
        String firstName;
        String lastName;
        int age;
    
        @Override
        int hashCode() {
            return Objects.hash(firstName, lastName, age); // 借助Objects.hash()方法计算hash值
        }
    }
    ```

  + 编写`equals()`和`hashCode()`遵循的原则是：

    `equals()`用到的用于比较的每一个字段，都必须在`hashCode()`中用于计算；`equals()`中没有使用到的字段，绝不可放在`hashCode()`中计算。

+ 按key有序map

  + `SortedMap`在遍历时严格按照Key的顺序遍历，最常用的实现类是`TreeMap`；

  + ```mermaid
    graph BT
    A[HashMap]-->B[Map]
    C[SortedMap]-->B
    D[TreeMap]-->C
    ```

  + 作为`SortedMap`的Key必须实现`Comparable`接口，或者传入`Comparator`；

  + `TreeMap`不使用`equals()`和`hashCode()`，`TreeMap`在比较两个Key是否相等时，依赖Key的`compareTo()`方法或者`Comparator.compare()`方法。**在两个Key相等时，必须返回`0`。**

    ```java
    Map<Student, Integer> map = new TreeMap<>(new Comparator<Student>() {
      public int compare(Student p1, Student p2) {
        // 若没有相等判断逻辑，put之后再get只会返回null
        if (p1.score == p2.score && p1.getName().equals(p2.getName())) {
          return 0;
        }
        
        // 按分数从高到低排序，TreeMap根据compare函数的返回值依次比较key，compare返回负数时，将compare函数的第一个参数排在前面
        return p1.score > p2.score ? -1 : 1; 
      }
    });
    ```

    

### Set

**`Set`实际上相当于只存储key、不存储value的`Map`**。因为放入`Set`的元素和`Map`的key类似，都要正确实现`equals()`和`hashCode()`方法，否则该元素无法正确地放入`Set`。

+ TreeSet

  使用`TreeSet`和使用`TreeMap`的要求一样，添加的元素必须正确实现`Comparable`接口，如果没有实现`Comparable`接口，那么创建`TreeSet`时必须传入一个`Comparator`对象。
  
  ```mermaid
  graph BT
  A[HashMap]-->B[Map]
  C[SortedMap]-->B
  D[TreeMap]-->C
  ```
  
  

### Queue

+ Queue添加元素、取队首元素、取队首元素并删除方法：

  |                    | throw Exception | 返回false或null    |
  | :----------------- | :-------------- | :----------------- |
  | 添加元素到队尾     | add(E e)        | boolean offer(E e) |
  | 取队首元素并删除   | E remove()      | E poll()           |
  | 取队首元素但不删除 | E element()     | E peek()           |

+ 注意：**不要把`null`添加到队列中**，否则`poll()`方法返回`null`时，很难确定是取到了`null`元素还是队列为空。

+ 当我们需要从`Queue`中取出队首元素时，如果当前`Queue`是一个空队列，调用`remove()`方法，会抛出异常：

  ```java
  Queue<String> q = ...
  try {
      String s = q.remove();
      System.out.println("获取成功");
  } catch(IllegalStateException e) {
      System.out.println("获取失败");
  }
  ```

  

+ `LinkedList`即实现了`List`接口，又实现了`Queue`接口，但是，在使用的时候，如果我们把它当作List，就获取List的引用，如果我们把它当作Queue，就获取Queue的引用：

  ```java
  // 这是一个List:
  List<String> list = new LinkedList<>();
  // 这是一个Queue:
  Queue<String> queue = new LinkedList<>();
  ```

### PriorityQueue

+ `PriorityQueue`实现了一个优先队列：从队首获取元素时，总是获取优先级最高的元素。

+ `PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法（元素就不必实现`Comparable`接口）。

### Deque

+ `Deque`实现了一个双端队列（Double Ended Queue），它可以：

  - 将元素添加到队尾或队首：`addLast()`/`offerLast()`/`addFirst()`/`offerFirst()`；

  - 从队首／队尾获取元素并删除：`removeFirst()`/`pollFirst()`/`removeLast()`/`pollLast()`；

  - 从队首／队尾获取元素但不删除：`getFirst()`/`peekFirst()`/`getLast()`/`peekLast()`；

  - 总是调用`xxxFirst()`/`xxxLast()`以便与`Queue`的方法区分开；

  - 避免把`null`添加到队列。

+ `Deque`接口的实现类有`ArrayDeque`和`LinkedList`。

### Stack

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop()`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek()`/`peekFirst()`。

当我们把`Deque`作为`Stack`使用时，注意只调用`push()`/`pop()`/`peek()`方法，不要调用`addFirst()`/`removeFirst()`/`peekFirst()`方法，这样代码更加清晰。

### Collections工具类

+ 添加所有元素到指定集合中

  ```java
  public static <T> boolean addAll(java.util.Collection<? super T> c, T... elements )
  ```

  

+ `Collections`提供了一系列静态方法来创建空集合：

  - 创建空List：`List<T> emptyList()`
  - 创建空Map：`Map<K, V> emptyMap()`
  - 创建空Set：`Set<T> emptySet()`

   要注意到返回的空集合是**不可变集合**，无法向其中添加或删除元素。

+ `Collections`提供了一系列静态方法来创建一个单元素集合：

  - 创建一个元素的List：`List<T> singletonList(T o)`

  - 创建一个元素的Map：`Map<K, V> singletonMap(K key, V value)`

  - 创建一个元素的Set：`Set<T> singleton(T o)`


​		要注意到返回的单元素集合也是**不可变集合**，无法向其中添加或删除元素。

+ 对可变`List`排序

  ```java
  public static <T extends Comparable<? super T>> void sort(List<T> list); // List的元素必须实现Comparable接口
  public static <T> void sort(List<T> list, Comparator<? super T> c); // 指定比较规则
  ```

  

## 设计模式

### 静态工厂方法

+ 把能创建“新”对象的静态方法称为静态工厂方法。
+ 创建新对象时，优先选用静态工厂方法而不是new操作符。
