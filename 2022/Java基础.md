<a name="YyQEJ"></a>

# 开始

<a name="cgJ9Q"></a>

## 解释

<a name="wHyoG"></a>

### 基础概念

关于 `Java`的历史不多介绍, 一些基础概念解释一下:<br />`JVM`:  Java Virtual Machine -- Java 虚拟机, Java 跨平台的基础, 屏蔽不同操作系统的差异,
字节码的执行器<br />`JRE`:  Java Runtime Environment -- Java 运行时环境. 本质上就是 JVM + 标准库;<br />`JDK`:  Java Development Kit -- Java 开发工具包,
提供 Java 开发相关的工具, 比如: javac, java, jar, javadoc, javap 等(见 <JDK安装目录>/bin); JDK 包含 JRE
> 在 JDK 1.9 之后, 由于 `JPMS`的推出, JDK 使用模块化重新组织, JDK 安装目录并不直接包含 jre 目录 (但是不影响Java程序运行)
> 如果需要 JRE 目录, 可以执行:  `bin\jlink.exe --module-path jmods --add-modules java.desktop --output jre`


<a name="BAHzi"></a>

### Java 平台

<a name="Pem8c"></a>

#### Java 1 和 Java 2

1998-12 SUN 公司发布了 Java 1.2 , 在此之前的 Java 版本 称为 Java 1, 在此之后称为 Java 2

<a name="zjdz5"></a>

#### J2SE, J2ME, J2EE 和 Java SE, Java EE, Java ME

J2SE: Java 2 Platform Standard Edition -- Java 2 平台标准版<br />J2EE: Java 2 Platform Enterprise Edition -- Java 2 平台企业版<br />J2ME: Java 2
Platform Micro Edition -- Java 2 平台微型版

Java 5.0 版本后, J2SE J2EE J2ME 更名为 Java SE, Java EE, Java ME

SE 定义 Java 语言<br />EE 定义 Java 企业开发标准规范<br />ME 是面向嵌入式设备的魔改版 (已经基本被 安卓取代)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22804074/1658939934885-0f84c09c-018d-4f23-991d-102c87006f31.png#averageHue=%23c3cfcc&clientId=u93be7090-24ec-4&errorMessage=unknown%20error&from=paste&height=665&id=u1b418e72&originHeight=998&originWidth=1364&originalType=binary&ratio=1&rotation=0&showTitle=false&size=151281&status=error&style=none&taskId=u996ac38c-b8c6-431d-baef-60800add665&title=&width=909.3333333333334)

**Java SE**<br />包含基础语法定义, 常用标准库, 所有内容都在 JDK 包含的标准库中, 准确定义可以参考模块化后的 标准库中 `java.se`模块.

```java
/**
 * 定义 Java SE 平台的 API
 * <dl class="notes">
 * <dt>Java SE 平台可选:</dt>
 *   <a href="{@docRoot}/../specs/jni/index.html">Java 本机接口 (JNI)</a><br>
 *   <a href="{@docRoot}/../specs/jvmti.html">Java 虚拟机工具接口 (JVM TI)</a><br>
 *   <a href="{@docRoot}/../specs/jdwp/jdwp-spec.html">Java 调试有线协议 (JDWP)</a><br>
 * @since 9
 */
module java.se {
    requires transitive java.compiler;
    requires transitive java.datatransfer;
    requires transitive java.desktop;
    requires transitive java.instrument;
    requires transitive java.logging;
    requires transitive java.management;
    requires transitive java.management.rmi;
    requires transitive java.naming;
    requires transitive java.net.http;
    requires transitive java.prefs;
    requires transitive java.rmi;
    requires transitive java.scripting;
    requires transitive java.security.jgss;
    requires transitive java.security.sasl;
    requires transitive java.sql;
    requires transitive java.sql.rowset;
    requires transitive java.transaction.xa;
    requires transitive java.xml;
    requires transitive java.xml.crypto;
}
```

**Java EE**<br />Java EE 是一系列技术标准组成的平台, 由一堆JSR (Java Specification Request: Java 规范请求) 组成, <br />很多只是规范提案并不提供代码或
jar 包 不包含在 JDK 中, 需要额外下载;
> 例如:
> java.mail 需要单独下载
> servlet.api 一般包含在 servlet 容器中, 开发时需要需要额外引入


**包括以下内容:**

- Applet - Java Applet 浏览器小程序, 已过时
- EJB - 企业级 Java Bean, 已过时
- JDBC - Java Database Connectivity: Java 数据库连接
- JMS - Java Message Service: Java 消息服务
- JMX - Java Management
- JNDI - Java Naming and Directory Interface: Java 命名目录接口
- JSF - Java Server Faces
- JSP - Java Server Pages: Java 服务器页面
- JSTL - Java Server Pages Standard Tag Library : Java 服务器页面标准标签库
- JTA - Java Transaction API: Java 事务 API
- JavaMain - Java 邮件
- Servlet - Java Servlet API
- WS - Web Service
- 等

> 2017年，Oracle 宣布开源 Java EE 并将项目移交给 Eclipse 基金会，但Oracle移交的很不痛快，提了很多要求，其中就包括不能再使用Java
> EE这个名称，虽然有点无理但Eclipse基金会还是接受了这个要求并且改名为Jakarta EE，经历了从j2ee到java ee的改名再经历一次似乎也无所谓，但要命的是Oracle要求Jakarta
> EE不能修改javax命名空间，这个就意味着java ee移交后代码就此封版
>
> **Java EE 8.0 之后, Java EE 更名为 **`**Jakarta EE**`**, **`**Javax.***`**的包名统一更改为 **`**jakarta.***`

<a name="ZQ7cc"></a>

##  

<a name="P1qXq"></a>

## 安装 JDK

<a name="Litcc"></a>

### 下载

OpenJDK: [https://jdk.java.net/](https://jdk.java.net/)<br />
OracleJDK: [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/)<br />
GraalVM: [https://www.graalvm.org/downloads/](https://www.graalvm.org/downloads/)<br />Microsoft
OpenJDK: [https://docs.microsoft.com/zh-cn/java/openjdk/download](https://docs.microsoft.com/zh-cn/java/openjdk/download)<br />Alibaba
Dragonwell8: [https://github.com/alibaba/dragonwell8](https://github.com/alibaba/dragonwell8)

<a name="Sx2fe"></a>

### 安装

如果使用 `IDE`仅仅下载并解压 JDK 即可, IDE 由额外的配置引用所需JDK.<br />如果需要在命令行中使用 Java 需要配置如下环境变量:

- 新增名为 `JAVA_HOME`的环境变量, 指向 JDK 解压后的目录 (不包含 `bin`文件夹)
- 把 JDK 中的 `bin`文件夹添加到 `PATH` 环境变量中, 推荐使用引用`JAVA_HOME` 例: `%JAVA_HOME%\bin` -- windows 平台; `$JAVA_HOME/bin` -- Linux 平台

至于如何配置环境变量, 参考不同操作系统设置.

<a name="R5bgS"></a>

### 验证

在命令行输入 `java --version` 或 `javac --version ` 有正确输出即为环境变量配置正确

<a name="DG5VF"></a>

## Hello World

国际惯例, 一个 Hello World 案例

新建一个文本文件, 名为 MainClass.java 写入如下内容:

```java
public class MainClass {
    
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
    
}
```

运行 : javac ./MainClass.java 编译, 生成 MainClass.class 字节码文件<br />运行: java MainClass 执行程序, 查看输出

```shell
# 编译
$ javac ./MainClass.java

# 查看编译结果
$ ll
总用量 8.0K
-rw-r--r-- 1 bingning bingning 424 3月  27 02:14 MainClass.class
-rw-r--r-- 1 bingning bingning 110 3月  27 02:14 MainClass.java

# 运行
$ java MainClass
Hello World!

```

附: 如果你的 JDK 版本大于等于 JDK 11 可以使用单文件程序功能 由 JEP 330 提供, 具体细节 参考 JEP 规范<br />JEP 330: Launch Single-File Source-Code
Programs -- 启动单文件源代码程序 [https://openjdk.java.net/jeps/330](https://openjdk.java.net/jeps/330)

编写代码和上面的栗子没有区别, 只是编译步骤可以跳过:

```shell
# 查看当前目录文件
$ ls
Main.java

# 查看文件内容
$ cat Main.java
public class MainClass {

        public static void main(String[] args){
                System.out.println("Hello World!");
        }
}

# 运行 java 文件 (在这里 java 命令后的参数没有智能提示)
$ java ./Main.java
Hello World!

# 查看当前目录文件
$ ls
Main.java
```

<a name="mFFZm"></a>

# 基础语法

基础语法, 数据结构 类 C, 有略有不同

首先, 代码中所有多余的空格和换行无效; 每行代码以 `;`结尾, 代码块以 `{}`包裹;<br />所有的代码必须写在 `class` 内, 不支持游离的函数和语句;<br />
简单的类定义: `public class <类名> {}` 一个java文件中, public 类最多只能有一个.<br />简单的方法(其他语言中称为函数)
定义: `[访问修饰符] [其他修饰符] [泛型参数] <返回值类型> <方法名>(形参列表){}`

> Java 中的惯例:
> 类名使用驼峰, 首字母大写; public class 的类名和所在文件的文件名保持一致(所以文件名首字母也需要大写)P
> 方法名, 变量名, 属性名使用驼峰, 首字母小写;
> 包名使用所在组织的域名反转, 全小写;
> 常量使用全大写字母, `_`分隔不同单词


关键字, 标识符, 常量, 变量, 作用域, 语句, 操作符, 略

<a name="w28DW"></a>

## 数据类型

![](https://cdn.nlark.com/yuque/0/2022/jpeg/22804074/1648322117124-35e17edf-7159-4008-a101-de3aca23a535.jpeg)

<a name="PEbZc"></a>

### 基础类型

八种基础数据类型:

- 四种整形数字
- 两种浮点型数字
- 一个字符类型(本质上还是数字)
- 一个 boolean 类型 表示真假
  <a name="nHqvl"></a>

### 引用类型

除了基础类型之外 都是引用类型, <br />Java 不存在 函数类型(类似 JavaScript 的那种 Function), 所有函数(方法)必须属于某个类 没有游离的函数定义<br />
如果需要纯函数类型定义, 可以使用 JDK 1.8 提供的 `FunctionInterface` 本质上还是匿名内部类

<a name="uZsNg"></a>

# 流程分支语句

if if-else if-elseif for while do-while 略, 同 C

<a name="XZ8tQ"></a>

## switch: 多分支

和 C 类似, switch 用于执行多分支, 默认按顺序执行所有 `case` , 除非遇到 `break`<br />需要注意的是, switch 只能用于
char/Character，byte/Byte，short/Short，int/Integer，enum(JDK 1.5), 或 String(JDK 1.7)<br />如果switch使用引用类型变量, 当遇到 null 时会引发 空指针异常;

```java
var v;
    switch(v){
    case 1:break;
    case 2:break;
default:
    // ......
    }
```

<a name="qTYoN"></a>

## switch 表达式(JDK 14)

在 JDK 14 中增加一个新功能: switch 表达式, 可以让 switch 不同分支返回不同的值, 让 switch 可以直接参与运算, 避免使用大量 return<br />
由JEP361提供: [https://openjdk.java.net/jeps/361](https://openjdk.java.net/jeps/361)

```java

switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}

int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};

int j = switch (day) {
    case MONDAY  -> 0;
    case TUESDAY -> 1;
    default      -> {
        int k = day.toString().length();
        int result = f(k);
        yield result;
    }
};
int result = switch (s) {
    case "Foo": 
        yield 1;
    case "Bar":
        yield 2;
    default:
        System.out.println("Neither Foo nor Bar, hmmm...");
        yield 0;
};
```

<a name="bs0wN"></a>

## switch 模式匹配

由 JEP305 提供: [https://openjdk.java.net/jeps/305](https://openjdk.java.net/jeps/305)  switch 表达式可以用于处理多个instanceof 分支的情况, 此功能在
JDK 17 中预览

```java
static void testFooBar(String s){
    switch(s){
    case null->System.out.println("Oops");
    case"Foo","Bar"->System.out.println("Great");
default           ->System.out.println("Ok");
    }
    }
```

<a name="XdozP"></a>

# 面向对象

<a name="bGJmr"></a>

## 类, 抽象类, 接口, 枚举, 注解

---

<a name="jBaNd"></a>

## 继承, 实现

---

<a name="AmcQq"></a>

## 重载, 重写

---

<a name="YQCBq"></a>

## 初始化顺序和类加载器

---

<a name="f9HiL"></a>

## 访问权限修饰符

---

<a name="k5fEj"></a>

## 泛型, 异常, 可变参数, 自动装箱/拆箱 及其对方法重载/重写的影响

---

<a name="BUBwv"></a>

# 常用类和功能

---

<a name="LNQZz"></a>

## String

---

<a name="vwPKq"></a>

## 正则表达式

<a name="EdC59"></a>

### 正则通用规则

<a name="LOD1o"></a>

#### 数量限制 (限定符)

- `?`0 或 1
- `+` 1 或 n
- `*` 0 或 n
- `{n}` n
- `n,` 匹配n以上
- `m, n` 最少 m, 最多 n

<a name="zAM3l"></a>

#### 定位符

- `^` 输入字符串开始位置 如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \n 或 \r 之后的位置匹配。
- `$`  输入字符串结尾位置 如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \n 或 \r 之前的位置匹配。
- `\b`  匹配单词边界 即字与空格间的位置。`\B` 非单词边界匹配。

<a name="tpd1r"></a>

#### 特殊字符

- `.` 除了换行符`\n` 之外的任意单字符
- `\d` 数字; `\D` 非数字
- `\w` 数字 字母 下划线, 相当于 `[A-Za-z0-9_]` ; `\W` 非字符
- `()` 子表达式
- `\` 转义
- `|` 或

> <a name="yPU06"></a>

### 略

> <a name="bIQKz"></a>

### ?=、?<=、?!、?<!

> _更多内容可以参考：_[正则表达式的先行断言(lookahead)和后行断言(lookbehind)](https://www.runoob.com/w3cnote/reg-lookahead-lookbehind.html)


<a name="dxv59"></a>

#### 字符区间

`[任意普通字符]` 区间中任意一个<br />`[^任意普通字符]` 取反

<a name="gDTNu"></a>

#### 非打印字符

- `\cx`  匹配由x指明的控制字符。例如， `\cM` 匹配一个 `Control-M `或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。
- `\f` 匹配一个换页符。等价于 `\x0c` 和 `\cL`。
- `\n` 匹配一个换行符。等价于 \x0a 和 \cJ。
- `\r` 匹配一个回车符。等价于 \x0d 和 \cM。
- `\s` 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 `[ \f\n\r\t\v]` (区间中含空格)。注意 Unicode 正则表达式会匹配全角空格符。
- `\S` 匹配任何非空白字符。等价于 `[^ \f\n\r\t\v]`(区间中含空格)。
- `\t` 匹配一个制表符。等价于 \x09 和 \cI。
- `v`  匹配一个垂直制表符。等价于 \x0b 和 \cK。

<a name="juEoC"></a>

### 在Java 中使用正则

> 核心就两个类:
> `Pattern`模式 - 线程安全 代表编译后的正则表达式
> `Matcher` 匹配器 - 线程不安全 正则的解释引擎, 使用 `Pattern` 匹配某个输入字符串后获取

<a name="bVWNP"></a>

### Pattern 模式

> 模块: `java.base`
> 全类名: `java.util.regex.Pattern`
> 声明: ` public final class Pattern implements java.io._Serializable_`


<a name="tQp4P"></a>

### 常用API

<a name="pWwKO"></a>

#### 编译正则

```java
/**
 * 将给定的正则表达式编译成一个模式。
 * @param regex – 要编译的表达式
 * @return 给定的正则表达式编译成一个模式
 * @throws PatternSyntaxException – 如果表达式的语法无效
 */
public static Pattern compile(String regex) {
    return new Pattern(regex, 0);
}

/**
 * 将给定的正则表达式编译成具有给定标志的模式。
 * 
 * @param regex – 要编译的表达式
 * @param flags – 匹配标志，一个位掩码，可能包括CASE_INSENSITIVE 、 MULTILINE 、 DOTALL 、 UNICODE_CASE 、 CANON_EQ 、 UNIX_LINES 、 LITERAL 、 
 * UNICODE_CHARACTER_CLASS和COMMENTS
 * 
 * @return 给定的正则表达式编译成具有给定标志的模式
 * @throws IllegalArgumentException -如果比对应于所述定义的匹配标志其它的比特值中设置flags
 * @throws PatternSyntaxException – 如果表达式的语法无效
 */
public static Pattern compile(String regex, int flags) {
    return new Pattern(regex, flags);
}
```

<a name="d6lFl"></a>

#### 编译并立即匹配

```java
/**
 * 编译给定的正则表达式并尝试将给定的输入与其匹配。
 * 调用这个方便的表单方法
 *        Pattern.matches(regex, input);
 * 行为方式与表达式完全相同
 *        Pattern.compile(regex).matcher(input).matches()
 * 如果一个模式要被多次使用，编译一次并重用它会比每次都调用这个方法更有效率。
 * @param regex – 要编译的表达式
 * @param input – 要匹配的字符序列
 * @return 正则表达式是否与输入匹配
 * @throws  PatternSyntaxException – 如果表达式的语法无效
 */
public static boolean matches(String regex, CharSequence input) {
    Pattern p = Pattern.compile(regex);
    Matcher m = p.matcher(input);
    return m.matches();
}
```

<a name="NSyHK"></a>

### Matcher 匹配器

> 模块: `java.base`
> 全类名: `java.util.regex.Matcher`
> 声明: `public final class Matcher implements _MatchResult_`


<a name="w2QTk"></a>

#### MatchResult 接口 匹配结果

这四个方法用于获取当前匹配子串的偏移量<br />java.util.regex.MatchResult#start()<br />java.util.regex.MatchResult#start(int)<br />
java.util.regex.MatchResult#end()<br />java.util.regex.MatchResult#end(int)<br />这两个用于获取当前匹配的子串内容<br />
java.util.regex.MatchResult#group()<br />java.util.regex.MatchResult#group(int)

详细见接口文档注释

<a name="OXUHJ"></a>

#### 常用API

全部替换

```java
/**
 * @since9
 */
java.util.regex.Matcher#replaceAll(java.util.function.Function<java.util.regex.MatchResult,java.lang.String>)

/**
 * 无限制
 * @since9
 */    
java.util.regex.Matcher#replaceAll(java.lang.String)    

/**
 * 需要配合find() 和 StringBuilder 使用, 如果只是为了全量替换, 不如楼上两位
 * @since9
 */    
java.util.regex.Matcher#appendReplacement(java.lang.StringBuilder, java.lang.String)    
java.util.regex.Matcher#appendTail(java.lang.StringBuilder)
    
public void testEx() {
    Pattern p = Pattern.compile("猫");
    Matcher m = p.matcher("院子里的一只猫两只猫");
    StringBuilder sb = new StringBuilder();
    while (m.find()) {
        m.appendReplacement(sb, "狗");
    }
    m.appendTail(sb);
    System.out.println(sb.toString());
}    
```

---


<a name="AjOuy"></a>

## 基本类型包装

---

<a name="r3Ja3"></a>

## 日期时间API

---

<a name="sVxBp"></a>

## 集合框架

---

<a name="T7lt1"></a>

## 文件和IO

<a name="xHfzc"></a>

###  

<a name="fmqie"></a>

### 概念

<a name="ddKp4"></a>

#### 同步和异步

同步与异步（synchronous/asynchronous）：**同步**是一种可靠的有序运行机制，当我们进行同步操作时，后续的任务是等待当前调用返回，才会进行下一步；而**异步**
则相反，其他任务不需要等待当前调用返回，通常依靠事件、回调等机制来实现任务间次序关系<br />**同步和异步的概念：实际的I/O操作**<br />
同步是用户线程发起I/O请求后需要等待或者轮询内核I/O操作完成后才能继续执行<br />异步是用户线程发起I/O请求后仍需要继续执行，当内核I/O操作完成后会通知用户线程，或者调用用户线程注册的回调函数

<a name="YI9PP"></a>

#### 阻塞和非阻塞

阻塞与非阻塞：在进行**阻塞**操作时，当前线程会处于阻塞状态，无法从事其他任务，只有当条件就绪才能继续，比如ServerSocket新连接建立完毕，或者数据读取、写入操作完成；而
**非阻塞**则是不管IO操作是否结束，直接返回，相应操作在后台继续处理<br />**阻塞和非阻塞的概念：发起I/O请求**<br />
阻塞是指I/O操作需要彻底完成后才能返回用户空间<br />非阻塞是指I/O操作被调用后立即返回一个状态值，无需等I/O操作彻底完成

<a name="WH0t4"></a>

### 标准IO: java.io 同步阻塞(BIO)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/22804074/1638087194109-28f4b604-577c-4db1-940f-3309cb7e9cff.jpeg)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22804074/1638091772832-bb839a4d-1b61-4237-a768-165b5e5ae048.png#clientId=u31f7c591-bad7-4&errorMessage=unknown%20error&from=paste&height=600&id=u2c0f769c&originHeight=501&originWidth=563&originalType=binary&ratio=1&rotation=0&showTitle=false&size=348286&status=error&style=none&taskId=u71d8a8f4-dcbf-4d71-a6f6-82044159c79&title=&width=674)![image.png](https://cdn.nlark.com/yuque/0/2021/png/22804074/1638091799206-b3b9fd98-0021-4063-af4d-bdeac8f40e14.png#clientId=u31f7c591-bad7-4&errorMessage=unknown%20error&from=paste&height=600&id=ubf9bc901&originHeight=407&originWidth=579&originalType=binary&ratio=1&rotation=0&showTitle=false&size=308205&status=error&style=none&taskId=u5a78db51-4eee-446d-975e-ea73a3cae03&title=&width=854)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22804074/1638091591658-fb08b953-b3ac-4af4-bff1-c6a2d2759c39.png#clientId=u31f7c591-bad7-4&errorMessage=unknown%20error&from=paste&height=600&id=u456a4fc4&originHeight=392&originWidth=433&originalType=binary&ratio=1&rotation=0&showTitle=false&size=214598&status=error&style=none&taskId=u061f3be8-35e4-4c8f-83bd-5333265e271&title=&width=663)![image.png](https://cdn.nlark.com/yuque/0/2021/png/22804074/1638091700010-78726959-7182-42ac-b270-985a40f1e316.png#clientId=u31f7c591-bad7-4&errorMessage=unknown%20error&from=paste&height=600&id=u15ebaab8&originHeight=379&originWidth=392&originalType=binary&ratio=1&rotation=0&showTitle=false&size=184038&status=error&style=none&taskId=u02380540-5259-43f2-a4dd-f05a1b189bb&title=&width=621)

<a name="P7VjQ"></a>

### 新IO: java.nio 同步非阻塞(NIO)

<a name="iIdoF"></a>

#### 缓冲区

> <a name="eSI36"></a>

## 特定原始类型数据的容器。

> 缓冲区是特定原始类型元素的线性有限序列。 除了内容之外，缓冲区的基本属性是它的容量、限制和位置：
> 缓冲区的容量是它包含的元素数。 缓冲区的容量永远不会为负且永远不会改变。
> 缓冲区的限制是不应读取或写入的第一个元素的索引。 缓冲区的限制永远不会为负，也永远不会大于其容量。
> 缓冲区的位置是要读取或写入的下一个元素的索引。 缓冲区的位置永远不会为负，也永远不会大于其限制。
> 每个非布尔基本类型都有这个类的一个子类。
> <a name="VZlvW"></a>

### 传输数据

> 该类的每个子类都定义了两类get和put操作：
> <a name="zIT0v"></a>

### 标记和重置

> `缓冲区的标记是调用reset方法时其位置将重置到的索引。`
> <a name="b2j7D"></a>

### 不变量

> 以下不变量适用于标记、位置、限制和容量值：
> 0 <=标记<=位置<=限制<=容量
> 新创建的缓冲区始终具有零位置和未定义的标记。 初始限制可能为零，也可能是某个其他值，具体取决于缓冲区的类型及其构造方式。 新分配的缓冲区的每个元素都初始化为零。

> <a name="zIa3w"></a>

### 附加操作

> 除了用于访问位置、限制和容量值以及用于标记和重置的方法之外，该类还定义了对缓冲区的以下操作：
> - clear使缓冲区为新的通道读取或相对放置操作序列做好准备：它将容量和位置的限制设置为零。
> - flip使缓冲区为新的通道写入或相对获取操作序列做好准备：它将限制设置为当前位置，然后将位置设置为零。
> - rewind使缓冲区准备好重新读取它已经包含的数据：它保持限制不变并将位置设置为零。
> - slice和slice(index,length)方法创建缓冲区的子序列：它们保持限制和位置不变。
> - duplicate创建缓冲区的浅拷贝：它保持限制和位置不变。
    > <a name="mMfi5"></a>

### 只读缓冲区

> 每个缓冲区都是可读的，但并非每个缓冲区都是可写的。 每个缓冲区类的变异方法被指定为可选操作，当在只读缓冲区上调用时将抛出ReadOnlyBufferException 。
> 只读缓冲区不允许更改其内容，但其标记、位置和限制值是可变的。 缓冲区是否为只读可以通过调用它的isReadOnly方法来确定。
> <a name="IMv1r"></a>

### 线程安全

> 多个并发线程使用缓冲区是不安全的。 如果一个缓冲区被多个线程使用，那么对缓冲区的访问应该由适当的同步控制。
>
> -- java.nio.Buffer

```java
// 不变量：标记 <= 位置 <= 限制 <= 容量
private int mark=-1;
private int position=0;
private int limit;
private int capacity;
```

<a name="Oa4GQ"></a>

#### 常用方法

- **clear**() : `**position **= **0**; **limit **= **capacity**; **mark **= -**1**;` 清空(伪), 读取 或 放置之前使用
- **flip(): **`**limit **= **position**; **position **= **0**; **mark **= -**1**;` 反转,

<a name="Ev4MG"></a>

### 通道: Channel

<a name="D4zlA"></a>

#### 获取通道的方式

1. java 支持通道的类都提供了 getChannel() 方法
    1. 本地IO
        1. FileInputStream / FileOutputStream
        2. RandomAccessFile
    2. 网络IO
        1. Socket
        2. ServerSocket
        3. DatagramSocket
2. JDK 1.7 中 NIO.2 针对个个通道提供了 `open()` 方法
3. JDK 1.7 中 NIO.2 `Files#newByteChannel()`

<a name="TkM5b"></a>

### 选择器:

<a name="uFB0q"></a>

### 异步非阻塞(AIO)

<a name="BcKJN"></a>

### 参考资料

[java IO、NIO、AIO详解 - StoneGeek - 博客园.html](https://www.cnblogs.com/sxkgeek/p/9488703.html)

---

<a name="KA767"></a>

## 反射

<a name="pDtjN"></a>

## 并发和线程

<a name="Srmji"></a>

# 国际化

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22804074/1658941104485-69104a8d-a6fb-4fa2-8c9d-bab378627251.png#clientId=u0a692f35-3bd6-4&errorMessage=unknown%20error&from=paste&height=1007&id=ue7cad319&originHeight=1510&originWidth=2166&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1263576&status=error&style=none&taskId=ue6d107e9-f68c-47b6-b30b-fae8bc866f0&title=&width=1444)
