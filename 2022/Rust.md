<a name="O4fW6"></a>

## 数据类型

![](https://cdn.nlark.com/yuque/0/2022/jpeg/22804074/1651135817956-49c61c6a-326f-4c91-a0a3-d0949337d98d.jpeg)

<a name="qsRa2"></a>

### 整数

所有数值类型 未明确指定时 类型为: `i32`

| 长度      | 有符号   | 无符号   |
|---------|-------|-------|
| 8-bit   | i8    | u8    |
| 16-bit  | i16   | u16   |
| 32-bit  | i32   | u32   |
| 64-bit  | i64   | u64   |
| 128-bit | i128  | u128  |
| arch    | isize | usize |

isize 和 usize 类型依赖运行程序的计算机架构：64 位架构上它们是 64 位的， 32 位架构上它们是 32 位的。

数字字面量

| 数字字面值               | 例子          |
|---------------------|-------------|
| Decimal (十进制)       | 98_222      |
| Hex (十六进制)          | 0xff        |
| Octal (八进制)         | 0o77        |
| Binary (二进制)        | 0b1111_0000 |
| Byte (单字节字符)(仅限于u8) | b'A'        |

<a name="PfRXq"></a>

### 浮点数

| f32 | 32位 |
|-----|-----|
| f64 | 64位 |

<a name="qLrTh"></a>

### 布尔值

略
<a name="kv3GJ"></a>

### 字符

`char` 使用四个字节, 单引号, 代表一个 unicode 标量值

<a name="pSlhZ"></a>

### 复合类型

<a name="Pebze"></a>

#### 元组

没有任何值的元组  `()` 是一个特殊类型, 被称为 `单元类型 (unit type)`, **如果表达式不返回任何其他的值, 则会隐式返回单元值;**

```rust
fn main() {
    // 元组声明
    let tup: (i32, char, f64) = (1, '1', 1.1);
    // 元组解构
    let (x, y, z) = tup;
    
    // 元组索引取值
    let i = tup.0
    let c = tup.1
    let f = tup.2
    
}
```

> 注: 大概可以视为定长,不同类型的数组?


<a name="Jm0aa"></a>

#### 数组

```rust
fn main() {
    // 数组声明
    let arr = [1, 2, 3, 4];
    
    // 数组类型注释 [数据类型; 数组长度]
    let charArr: [char; 3] = ['a', 'b', 'c'];
    
    // 快速初始化数组, 相当于: let arr = [233, 233, 233];
    let arr = [233; 3];
    
    
    // 数组取值, 索引从0 开始, arr[index]
}
```

> 如果索引超出了数组长度，Rust 会 _panic, _这是 Rust 术语，它用于程序因为错误而退出的情况
> thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
> note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace



> 注: 定长, 同类型


<a name="rzT2m"></a>

## 函数

命名风格: 使用小写下划线<br />语法:

```rust
fn 函数名([参数列表]) [-> 返回值类型] {
    
    // 最后一个表达式的结果就是返回值; 如果最后一行不是表达式, 默认返回 () 空元组 (单元类型)
    表达式
}
```

其他略

<a name="D3Q2Q"></a>

## 流程控制

<a name="Lmozm"></a>

### 分支

```rust
// 表达式没有括号, else 可选; 支持 if else if 简写
if 表达式 {} [else {}]
```

**if 是一个表达式, 可以有返回值, 但是不同分支的返回值类型必须相同;**  这意味支持如下写法:

```rust
let ok = true
let num: i32 = if ok { 5 } else { 0 }

// 5
println!(num)
```

<a name="r7FwP"></a>

### 循环

<a name="BuLaF"></a>

#### loop

```rust
loop {
    // 一些语句 ...
    
    // 停止循环
    break;
    
    // 中断一次
    continue;
}

// 支持标签
label: loop {

    loop {
    
        // 中断到指定的标签
        break label;
    
    }
}

// 有返回值的循环
let res = loop {
    break 233;
}
```

<a name="eiQc2"></a>

#### while

```rust
while 表达式 {

}
```

<a name="DopVn"></a>

#### for-in

```rust
for item in [1, 2, 3] {

}
```

<a name="dsqQl"></a>

# 所有权 引用借用 Slice

暂略

<a name="QpI2t"></a>

# 结构体

> 结构体如果包含引用类型, 需要指定生命周期

struct 结构体

```rust
// 声明结构体
struct 结构体名称 {
    fieldName: String
}

// 实例化结构体
let str = 结构体名称 {
    结构体属性
}
// 使用 .filedName 取值


// 结构体更新语法, 本质上是浅拷贝, 含所有权转移 (移动)
let str2 = {
    name: String::from(""),
    ..str1
}
    
```

元组结构体

```rust
// 声明
struct RGB(i32, i32, i32);

// 实例化
let color = RGB(0, 100, 255);

// 取值: 结构体结构 或者 .[index]

```

类单元结构体

```rust
// 声明: 不需要 {}
struct UnitStruct;

// 使用: 不需要 {}
let s = UnitStruct;

```
