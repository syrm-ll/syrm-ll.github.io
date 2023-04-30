# JDK常用类（含JDK8）

## 数据类型包装类







## 日期时间API

### Date 和 DateFormat

`java.util.Date ` 类是基础的时间类，保存一个`Long`类型数字纪录的时间毫秒数，从1970-01-01 开始计算，以国际标准时为准。

`java.text.DateFormat` 是一个抽象类，其本身没什么意义， 唯一子类 `java.text.SimpleDateFormat` 可使用**格式化字符串**来把`Date`类型数据和日期字符串之间相互转化。

> **注意：**`SimpleDateFormat`是线程不安全的， 不推荐放在公共静态字段上。对于想封装工具类复用对象的人来说是个坏消息。

**用法：** Date

```java
Date date = new Date(1369970751598L);
System.out.println(date);

System.out.println(new Date().getTime());
System.out.println(System.currentTimeMillis());

// 输出：
Fri May 31 11:25:51 CST 2013
1582087530218
1582087530218
```

**用法：** SimpleDateFormat

```java
DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-DD HH:mm:ss::SS");
String format = dateFormat.format(new Date());
System.out.println(format);

System.out.println(dateFormat.parse("2020-02-50 12:47:58::868"));

//输出
2020-02-50 12:49:01::468
Wed Feb 19 12:47:58 CST 2020
```

### JDK8日期时间API

| 类名                        | 作用                                 |
|---------------------------|------------------------------------|
| `java.time.LocalDate`     | 不含时区，不含时分秒的日期。                     |
| `java.time.LocalTime`     | 不含时区，不含日期的时分秒。                     |
| `java.time.LocalDateTime` | 不含时区，完整的日期时间信息。                    |
| `java.time.Instant`       | 时间戳, 类似于`java.util.Date` 二者可以互相转换。 |
| `java.time.MonthDay`      | 不包含年份的月 日 信息。 用来判断特殊节日。            |

| 格式化工具                                | 作用                |
|--------------------------------------|-------------------|
| `java.time.DateTimeFormatter`        | 新的线程安全的格式化器       |
| `java.time.DateTimeFormatterBuilder` | 上面那位的进化版，更**高级**。 |





首先，上面这些新增的类都是不可变`final` 且线程安全的。 原文：

> @implSpec This class is immutable and thread-safe.

虽然都是`final class`  并且构造器是私有的，但是应没有做成单例，你依然可以使用反射获取构造来创建对象，虽然这毫无意义，因为有更简便的静态工厂：

```java
// 反射创建对象。
Class<LocalDate> ldClass = LocalDate.class;
Constructor<?>[] constructors = ldClass.getDeclaredConstructors();
for (Constructor<?> c : constructors) {
    c.setAccessible(true);
    Object o = c.newInstance(1, 1, 1);
}

// 静态工厂， 以LocalDate为例。
public static LocalDate of(int year, int month, int dayOfMonth) {
	...
}
public static LocalDate of(int year, Month month, int dayOfMonth) {
   ...
}

// 第一个参数为年，第二个为当年多少天。
LocalDate.ofYearDay(1991, birDay.getDayOfYear());
// 距离1970-01-01 的天数。
LocalDate.ofEpochDay(birDay.toEpochDay());
// 以默认的yyyy-MM-dd 格式，把字符串转为LocalDate
LocalDate.parse("1991-11-11");
// 指定格式化字符串
LocalDate.parse("19911111",DateTimeFormatter.ofPattern("yyyyMMdd"));

```

#### **常用方法：** 

> 以LocalDate为例。

#### 创建，获取。

```java
// 创建对象,重载的方法不演示。
LocalDate.now();
LocalDate.of(2020, 01, 01);
LocalDate.parse("2020-01-01");

// 获取信息。
LocalDate d = LocalDate.now();
System.out.println(d);
System.out.println();

System.out.println(d.getYear());
System.out.println(d.getDayOfYear());
System.out.println(d.getMonth());
System.out.println(d.getMonthValue());
System.out.println();

System.out.println(d.getDayOfMonth());
System.out.println(d.getDayOfWeek());
System.out.println(d.getDayOfYear());

// 输出：
2020-02-19
    
2020
50
FEBRUARY
2

19
WEDNESDAY
50
```

#### 比较

```java
LocalDate now = LocalDate.now();
LocalDate 全面小康 = LocalDate.of(2020, 12, 31);

// 日期比较
System.out.println(now.isAfter(全面小康));
System.out.println(now.isBefore(全面小康));
System.out.println(now.equals(全面小康));

// 特殊节日判断
MonthDay monthDay = MonthDay.of(now.getMonthValue(), now.getDayOfMonth());
System.out.println(monthDay);
MonthDay end = MonthDay.from(全面小康);
System.out.println(end);

System.out.println(end.equals(monthDay));

// 输出
false
true
false
--02-19
--12-31
false
```

#### 修改

新日期时间都是不可变的， 不只是指`final class` ，和`String` 一样所有的修改方法都会返回一个新对象， 原对象的值**永不可变**。

`LocalDate.minus*` 是当前对象表示时间之前。

`LocalDate.plus*` 是修改到以后的时间。

```java
LocalDate end = LocalDate.parse("2020-12-31");
System.out.println(end);
LocalDate minus = end.minusYears(1);
System.out.println(minus);

System.out.println(minus.equals(end));

// 输出
2020-12-31
2019-12-31
false
```

#### 格式化

在JDK8之前，时间日期的格式化非常麻烦，经常使用`SimpleDateFormat`来进行格式化，但是`SimpleDateFormat`并不是线程安全的。在JDK8中，引入了一个全新的线程安全的日期与时间格式器`DateTimeFormatter`。

```java
LocalDateTime ld = LocalDateTime.now();
System.out.println(ld);
DateTimeFormatter formatter = 
    DateTimeFormatter.ofPattern("yyyy年MM月dd日 hh:mm:ss → SSSS");

System.out.println(formatter.format(ld));
System.out.println(ld.format(formatter));

MonthDay md = MonthDay.now();
System.out.println(md.format(DateTimeFormatter.ofPattern("MM月dd日")));

// 输出
2020-02-20T12:48:25.148167200
2020年02月20日 12:48:25 → 1481
2020年02月20日 12:48:25 → 1481
02月20日    
```

> **注意：**
>
> 有一个很搞笑的事情，如果格式化字符串包含时间部分，也就是 hh:mm:ss 这些， 日期对象使用`LocalDate` 会报错：
>
> ```java
> // 不支持的时间类型异常:不支持的字段：HourOfDay
> UnsupportedTemporalTypeException: Unsupported field: HourOfDay
> ```
>
> 同理，如果日期对象是 `LocalTime` 或者 `MonthDay` 也有类似限制。
>
> --------
>
> **小声BB：**<small> 有点失了智的设计，没有的直接忽略不行吗。。。 </small>



#### 计算日期时间差

| 类型                              | 作用                                         |
|---------------------------------|--------------------------------------------|
| `java.time.Period`              | 仅包含年月日的日期计算                                |
| `java.time.Duration`            | 基于时间的值测量时间量                                |
| `java.time.temporal.ChronoUnit` | 用于在单个时间单位内测量一段时间，这个工具类是最全的了，可以用于比较所有的时间单位。 |



> 以上 参考：
>
> [博客园]:https://www.cnblogs.com/wbxk/p/9598518.html



# IO

首先，有如下概念：

> **同步和异步**
>
> **同步**是一种可靠的有序运行机制，当我们进行同步操作时，后续的任务是等待当前调用返回，才会进行下一步；而**异步**则相反，其他任务不需要等待当前调用返回，通常依靠事件、回调等机制来实现任务间次序关系
>
> **阻塞和非阻塞**
>
> 在进行**阻塞**操作时，当前线程会处于阻塞状态，无法从事其他任务，只有当条件就绪才能继续，比如ServerSocket新连接建立完毕，或者数据读取、写入操作完成；而**非阻塞**则是不管IO操作是否结束，直接返回，相应操作在后台继续处理

BIO：同步阻塞。

​		传统的 `java.io `包和 `java.net `包都输入此类， 依据流模型实现。

NIO：non-blocking 同步非阻塞。

​		java 1.4 引入的 `java.nio `包。

AIO：异步非阻塞。

​		基于事件和回调改进的NIO。



## BIO：`java.io`

按照数据流入方向可分为：输入流（向内存输入），输出流（向文件输出）。

按照操作数据可分为：字节流和字符流。

### 基本介绍

#### **字符流**

字符流只能用来处理文本数据，本质上就是在读取字节时查了对应的字符集，按照字符集格式读取数据 避免乱码。

注意事项：

- 写入文件必须要用`flush()`刷新。
- 使用流对象需要抛出`IOException`，用完需要关闭`close()`。
- 定义路径时可以用“\” 或者“/”。
- 对指定文件写入，如果不存在将会创建，如果存在将会覆盖（存疑）。
- 读取指定文件时必须保证文件存在，否则抛出异常。

缓冲区：

- 缓冲区的出现是为了提高流的操作效率而出现的
- 需要被提高效率的流作为参数传递给缓冲区的构造函数
- 在缓冲区中封装了一个数组，存入数据后一次取出

#### **字节流**

字节流可以用来处理任何形式的数据， 一次操作一个字节byte（8 个 bit）。

注意事项：

- 字节流和字符流的基本操作是相同的，但是想要操作媒体流就需要用到字节流
- 字节流因为操作的是字节，所以可以用来操作媒体文件（媒体文件也是以字节存储的）
- 输入流（InputStream）、输出流（OutputStream）
- 字节流操作可以不用刷新流操作
- InputStream特有方法：int available()（返回文件中的字节个数）但是慎用！ 对于本地文件一般没问题，socket因为网络是分包发送，返回的值没有参考意义。





字节流的缓冲区
字节流缓冲区跟字符流缓冲区一样，也是为了提高效率。

#### Scanner 和 Random

`java.util.Scanner`  jdk 1.5 新增加的。

`java.io.RandomAccessFile`

略

### 字节输入流



### 字节输出流



### 字符输入流



### 字符输出流





## BIO：`java.net`



## NIO：`java.nio`





# 网络编程

| 类                     | 作用     |
|-----------------------|--------|
| java.net.ServerSocket | 服务器套接字 |
| java.net.InetAddress  | 网络地址   |
| java.net.URL          | URL    |
|                       |        |
|                       |        |
|                       |        |



## UDP

| 类                       | 作用  |
|-------------------------|-----|
| java.net.DatagramSocket |     |
| java.net.DatagramPacket |     |
|                         |     |
|                         |     |
