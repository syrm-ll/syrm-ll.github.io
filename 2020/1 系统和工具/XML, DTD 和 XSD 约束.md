# XML

略



# DTD约束

> **DTD（文档类型定义）的作用是定义 XML 文档的合法构建模块。**
>
> **它使用一系列的合法元素来定义文档结构。**
>
> [来自W3CSchool](https://www.w3school.com.cn/dtd/index.asp)

说人话就是,DTD约束XML文件内容,定义XML文档结构合法性.

没有DTD的XML也能用, 但是不标准,没有合适的提示.



> ### 为什么使用 DTD？
>
> 通过 DTD，您的每一个 XML 文件均可携带一个有关其自身格式的描述。
>
> 通过 DTD，独立的团体可一致地使用某个标准的 DTD 来交换数据。
>
> 而您的应用程序也可使用某个标准的 DTD 来验证从外部接收到的数据。
>
> 您还可以使用 DTD 来验证您自身的数据。
>
> [W3CSchool](https://www.w3school.com.cn/dtd/dtd_intro.asp)



## 定义

### 局部定义

在XML 文件 DOCTYPE 声明中

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE 配置 [
    <!ELEMENT 配置 (设置, 别名, 插件, 其他)>		<!--  声明一级元素内容和顺序  -->
    <!ELEMENT 设置 (#PCDATA)>					<!--  声明一级元素类型, PCDATA　可被解析(还有子元素)，　CDATA 不被解析  -->
    <!ELEMENT 别名 (#PCDATA)>
    <!ELEMENT 插件 (#PCDATA)>
    <!ELEMENT 其他 (#PCDATA)>
        ]>
<配置>
    <设置/>
    <别名/>
    <插件/>
    <其他/>
</配置>
```



### 外部定义

独立的 XXX.dtd 文件, 在XML DOCTYPE中引用

```xml
<!DOCTYPE 配置 SYSTEM "冰蝶.dtd">
```

冰蝶.dtd

```xml-dtd
<?xml version="1.0" encoding="utf-8" ?>
<!ELEMENT 配置 (设置, 别名, 插件, 其他)>
<!ELEMENT 设置 (#PCDATA)>
<!ELEMENT 别名 (#PCDATA)>
<!ELEMENT 插件 (#PCDATA)>
<!ELEMENT 其他 (#PCDATA)>
```



## 语法

### 元素声明

XML中的元素由dtd中的元素声明来指定:

```xml-dtd
<!ELEMENT 元素名称 类别>
<!ELEMENT 元素名称 (元素内容)>

例:
声明空元素
<!ELEMENT br EMPTY>

声明 PCDATA 元素
<!ELEMENT config (#PCDATA)>

带有任何元素的元素
<!ELEMENT 元素名称 ANY>

带有子元素（序列）的元素, 子元素必须被完整声明,在 xml中按声明顺序完整出现
<!ELEMENT 元素名称 (子元素名称 1)>

声明子元素
<!ELEMENT 元素名称 (子元素名称)> 		必须且只能出现一次
<!ELEMENT 元素名称 (子元素名称+)>		最少出现一次
<!ELEMENT 元素名称 (子元素名称*)>		出现 0 或无数次
<!ELEMENT 元素名称 (子元素名称?)>		出现零次或一次的元素
```



复杂类型声明

```xml-dtd
<!-- message body 二选一 -->
<!ELEMENT note (to,from,header,(message|body))>

<!--  批量声明.   疑似必须用#PCDATA开头,且不强制顺序  -->
<!ELEMENT note (#PCDATA|to|from|header|message)*>
```



### 属性声明

语法:

```xml-dtd
<!ATTLIST 元素名称 属性名称 属性类型 默认值>
例:
<!ATTLIST payment type CDATA "check">
xml:
<payment type="check" />
```

 以下是*属性类型*的选项：

| 类型               | 描述                          |
| :----------------- | :---------------------------- |
| CDATA              | 值为字符数据 (character data) |
| (*en1*\|*en2*\|..) | 此值是枚举列表中的一个值      |
| ID                 | 值为唯一的 id                 |
| IDREF              | 值为另外一个元素的 id         |
| IDREFS             | 值为其他 id 的列表            |
| NMTOKEN            | 值为合法的 XML 名称           |
| NMTOKENS           | 值为合法的 XML 名称的列表     |
| ENTITY             | 值是一个实体                  |
| ENTITIES           | 值是一个实体列表              |
| NOTATION           | 此值是符号的名称              |
| xml:               | 值是一个预定义的 XML 值       |

 默认值参数可使用下列值：

| 值           | 解释           |
| :----------- | :------------- |
| 值           | 属性的默认值   |
| #REQUIRED    | 属性值是必需的 |
| #IMPLIED     | 属性不是必需的 |
| #FIXED value | 属性值是固定的 |

> 以上摘自 W3CSchool 懒得看了
>
> https://www.w3school.com.cn/dtd/dtd_attributes.asp



### 实体声明

略





# XSD

> **XML Schema 是基于 XML 的 DTD 替代者。**
>
> **XML Schema 描述 XML 文档的结构。**
>
> **XML Schema 语言也称作 XML Schema 定义（XML Schema Definition，XSD）。**
>
> https://www.w3school.com.cn/schema/index.asp



学不动了, 告辞~