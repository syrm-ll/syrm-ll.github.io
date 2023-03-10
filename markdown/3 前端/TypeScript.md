## 数据类型



| 任意类型  | any                       |
| --------- | ------------------------- |
| 数字类型  | number                    |
| 字符串    | string                    |
| 布尔类型  | boolean                   |
| 数组类型  | Type[] <br /> Array<Type> |
| 元组      | [ Type... ]               |
| 枚举      | "EnumClass"               |
| void      | void                      |
| null      | null                      |
| undefined | undefined                 |
| 从不出现  | never                     |



没有初始化的变量值为 undefined



### 类型断言

```ts
var str = '1' 
var str2:number = <number> <any> str   //str、str2 是 string 类型
console.log(str2)

// 编译后

var str = '1';
var str2 = str;  //str、str2 是 string 类型
console.log(str2);
```

仅仅编译时检查, 运行时没用, 算不得类型转换



### 类型推断

╮(╯▽╰)╭ 略





## 运算符

### 算术运算符

假定 **y=5**，下面的表格解释了这些算术运算符的操作：

| 运算符 | 描述         | 例子  | x 运算结果 | y 运算结果 |
| :----- | :----------- | :---- | :--------- | :--------- |
| +      | 加法         | x=y+2 | 7          | 5          |
| -      | 减法         | x=y-2 | 3          | 5          |
| *      | 乘法         | x=y*2 | 10         | 5          |
| /      | 除法         | x=y/2 | 2.5        | 5          |
| %      | 取模（余数） | x=y%2 | 1          | 5          |
| ++     | 自增         | x=++y | 6          | 6          |
| x=y++  | 5            | 6     |            |            |
| --     | 自减         | x=--y | 4          | 4          |
| x=y--  | 5            | 4     |            |            |

### 关系运算符

关系运算符用于计算结果是否为 true 或者 false。

x=5，下面的表格解释了关系运算符的操作：

| 运算符 | 描述       | 比较 | 返回值  |
| :----- | :--------- | :--- | :------ |
| ==     | 等于       | x==8 | *false* |
| x==5   | *true*     |      |         |
| !=     | 不等于     | x!=8 | *true*  |
| >      | 大于       | x>8  | *false* |
| <      | 小于       | x<8  | *true*  |
| >=     | 大于或等于 | x>=8 | *false* |
| <=     | 小于或等于 | x<=8 | *true*  |

### 逻辑运算符

逻辑运算符用于测定变量或值之间的逻辑。

给定 x=6 以及 y=3，下表解释了逻辑运算符：

| 运算符 | 描述 | 例子                      |
| :----- | :--- | :------------------------ |
| &&     | and  | (x < 10 && y > 1) 为 true |
| \|\|   | or   | (x==5 \|\| y==5) 为 false |
| !      | not  | !(x==y) 为 true           |

### 位运算符

位操作是程序设计中对位模式按位或二进制数的一元和二元操作。



| 运算符 | 描述                                                         | 例子        | 类似于       | 结果 | 十进制 |
| :----- | :----------------------------------------------------------- | :---------- | :----------- | :--- | :----- |
| &      | AND，按位与处理两个长度相同的二进制数，两个相应的二进位都为 1，该位的结果值才为 1，否则为 0。 | x = 5 & 1   | 0101 & 0001  | 0001 | 1      |
| \|     | OR，按位或处理两个长度相同的二进制数，两个相应的二进位中只要有一个为 1，该位的结果值为 1。 | x = 5 \| 1  | 0101 \| 0001 | 0101 | 5      |
| ~      | 取反，取反是一元运算符，对一个二进制数的每一位执行逻辑反操作。使数字 1 成为 0，0 成为 1。 | x = ~ 5     | ~0101        | 1010 | -6     |
| ^      | 异或，按位异或运算，对等长二进制模式按位或二进制数的每一位执行逻辑异按位或操作。操作的结果是如果某位不同则该位为 1，否则该位为 0。 | x = 5 ^ 1   | 0101 ^ 0001  | 0100 | 4      |
| <<     | 左移，把 << 左边的运算数的各二进位全部左移若干位，由 << 右边的数指定移动的位数，高位丢弃，低位补 0。 | x = 5 << 1  | 0101 << 1    | 1010 | 10     |
| >>     | 右移，把 >> 左边的运算数的各二进位全部右移若干位，>> 右边的数指定移动的位数。 | x = 5 >> 1  | 0101 >> 1    | 0010 | 2      |
| >>>    | 无符号右移，与有符号右移位类似，除了左边一律使用0 补位。     | x = 2 >>> 1 | 0010 >>> 1   | 0001 | 1      |

### 赋值运算符

赋值运算符用于给变量赋值。

给定 **x=10** 和 **y=5**，下面的表格解释了赋值运算符：

| 运算符                  | 例子   | 实例      | x 值   |
| :---------------------- | :----- | :-------- | :----- |
| = (赋值)                | x = y  | x = y     | x = 5  |
| += (先进行加运算后赋值) | x += y | x = x + y | x = 15 |
| -= (先进行减运算后赋值) | x -= y | x = x - y | x = 5  |
| *= (先进行乘运算后赋值) | x *= y | x = x * y | x = 50 |
| /= (先进行除运算后赋值) |        |           |        |

### 三元运算符 (? :)

三元运算有 3 个操作数，并且需要判断布尔表达式的值。该运算符的主要是决定哪个值应该赋值给变量。

```
Test ? expr1 : expr2
```

- Test − 指定的条件语句
- expr1 − 如果条件语句 Test 返回 true 则返回该值
- expr2 − 如果条件语句 Test 返回 false 则返回该值



### typeof 运算符

typeof 是一元运算符，返回操作数的数据类型。





## 流程控制

if

if else

switch



for

while

for in (类似 foreach)

for of : foreach 进阶版, 可以迭代 list array str maps sets



while

do while



break

continue





## 函数

### 常见

```ts
// 普通函数
function name() {}

// 定义返回值
function name() : type {}

// 定义参数类型
function name(arg : type, ...) : type {}

// 定义可选参数
function name(arg : type, arg2 ?: type) :type {}


// 定义参数默认值(隐式可选, 不能和可选同时定义)
function name(arg : type, arg2 : type = default_value) : type {}


// 剩余参数, 可变参数
function name(...arg : type) : type {}
```

### 构造函数

```ts
// 构造函数
var res = new Function ([arg1[, arg2[, ...argN]],] functionBody)

var myFunction = new Function("a", "b", "return a * b"); 
var x = myFunction(4, 3); 
console.log(x);

```

### 匿名函数, 箭头函数

```ts
// 匿名函数
let v = function(){}

// 箭头函数
( [param1, parma2,…param n] )=>statement;
```



### 重载方法

```ts
// 重载规则同 java, 方法名相同, 参数列表不同
// 没啥用

```



## 特殊类型

### Number

常用API, 特殊值

### String

常用API



### Array

#### 定义, 初始化, 略

#### 数组解构

```ts
let arr: number[] = [1, 2, 3, 4]
// 将数组的两个元素赋值给变量 x 和 y
let [x, y] = arr
console.log(x)
console.log(y)

```



#### 数组迭代, 多维数组 略



#### 常用方法

| 序号 | 方法 & 描述                                                  |
| :--: | :----------------------------------------------------------- |
|  1.  | concat()连接两个或更多的数组，并返回结果。                   |
|  2.  | every()检测数值元素的每个元素是否都符合条件。                |
|  3.  | filter()检测数值元素，并返回符合条件所有元素的数组。         |
|  4.  | forEach()数组每个元素都执行一次回调函数。                    |
|  5.  | indexOf()搜索数组中的元素，并返回它所在的位置。如果搜索不到，返回值 -1，代表没有此项。 |
|  6.  | join()把数组的所有元素放入一个字符串。                       |
|  7.  | lastIndexOf()返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。 |
|  8.  | map()通过指定函数处理数组的每个元素，并返回处理后的数组。    |
|  9.  | pop()删除数组的最后一个元素并返回删除的元素。                |
| 10.  | push()向数组的末尾添加一个或更多元素，并返回新的长度。       |
| 11.  | reduce()将数组元素计算为一个值（从左到右）。                 |
| 12.  | reduceRight()将数组元素计算为一个值（从右到左）。            |
| 13.  | reverse()反转数组的元素顺序。                                |
| 14.  | shift()删除并返回数组的第一个元素。                          |
| 15.  | slice()选取数组的的一部分，并返回一个新数组。                |
| 16.  | some()检测数组元素中是否有元素符合指定条件。                 |
| 17.  | sort()对数组的元素进行排序。                                 |
| 18.  | splice()从数组中添加或删除元素。                             |
| 19.  | toString()把数组转换为字符串，并返回结果。                   |
| 20.  | unshift()向数组的开头添加一个或更多元素，并返回新的长度。    |



### Map

### Set



## OOP

### 接口 Interface

### 类 Class

支持实现

支持继承, 单继承

支持部分 status

支持 `instanceof` 运算符

支持 三种权限控制 : public (默认),  protected,  private





## 命名空间

