# groovy

## 快速开始

### 基本写法:

groovy 中可以省略每行结束的 `;` 和 方法实参列表的 `()`

```groovy
// 以下三种写法等价
println("基本输出");
println("基本输出")
println "基本输出"
```

### 变量定义

```groovy
// 变量定义
def v = 1
println v

/* --------------------------------------------------------------------------------------------------------------------*/
// 复杂类型
// 貌似就是ArrayList
def list = ["冰凝", "琉璃"]
list << "a"
println list

// 貌似是HashMap
def map = ["name":"冰凝", "age": "18"]
map.other = "备份字段"
println map
```

### 闭包

```groovy
// 闭包
// 大概就是一段匿名代码块?
def bb = {
    def bb_map = ["name":"冰凝", "age": "18"]
    println bb_map
    return bb_map
}()

println bb

// 带参数的闭包

def method_1 (Closure ) {
    Closure ("冰蝶", "琉璃")
}

method_1({
    a, b ->
        println "传入的参数 : ${a} ${b}"
})
```

极其诡异的闭包语法

```groovy
// 完整写法, 类似lambda
def closure1 = {
    param -> return param + 1
}
// 最后一条语句可作为返回值, return 可以省略
def closure2 = {
    param -> param + 1
}
// it 为默认的形参, 单参数可以省略
def closure3 = { it + 1 }

// 以上三种方式等价
println closure1(1)
println closure2(1)
println closure3(1)

/* ------------------------------------------------------------------------------- */

def c1 = {
    i, c -> c(i)
}
// 如果闭包是实参列表最后一个参数, 可以提取到括号后面写
println c1(4, { it * 3 })
println c1(4) { it + 1 }

```





## 进阶一点

```groovy
class User {
    int id
    String name
    int age
    String other

    String toString() {
        return "ID: ${id} \n name: ${name} \n age: ${age} \n other: ${other}"
    }
}

// 这里全是语法糖: 传入Map, 自适应字段创建构造
// 输出: new User([id: 1, name:  "琉璃", age:  18]).toString()
println new User(id: 1, name:  "琉璃", age:  18).toString()

/* ----------------------------------------------------------------------------------------- */
// 关于原理解释:
// 对于以下代码:
println new User(id:  2).toString()
// java: invokedvnamic 静态调用
// groovy: MOP
// 动态调用, 类似纯反射
def tempo = new User(id:  3)
println invokeMethod(tempo,"toString").toString()
```

