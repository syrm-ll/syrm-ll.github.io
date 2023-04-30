# vue组件

## 创建 公共组件

### 方式一：

使用`Vue.extend` 创建模板， 使用`Vue.component` 创建组件， 在页面中使用自定义组件名为标签名插入组件。

> `Vue.extend` 中template 必须包含在一个标签内， 不能为纯文本域。
>
> `Vue.extend` 和 `Vue.component`都是全局的。

javascript

```js
let t = Vue.extend({
	template: "HTML" // 这里写HTML
});

Vue.component("com-name", t );

// 或者写作：
Vue.component("com1", Vue.extend({
        template: "<h1>HTML component 测试 0.0</h1>"
}));

Vue.component("com1", {
        template: "<h1>HTML component 测试 0.0</h1>"
});
```

html

```html
<div id="#app">
    <com-name></com-name>
</div>
```

### 方式二：

在vue控制区域之外 ，使用vue提供的组定义标签`template` 创建模板， 使用`Vue.component` 创建组件。

javascript

```html
<main id="app">
</main>
<template id="tem1">
    
</template>

<script>
Vue.component("com2", "#tem1");
    
</script>


```





## 创建私有组件

使用vue的`components` 属性

### 回顾 vue对象的属性

```js
let app = new Vue({
    el: "",
    data: {},
    methods: {},
    filters: {}, // 过滤器
    directives: {}, // 指令
    components: {}, //组件
    
    // 生命周期回调函数
    beforeCreate() {},
    created() {}, 
    beforeMount(){},
    mounted() {},
    beforeUpdate() {},
    update(){},
    beforeDestory() {},
    deftoryed() {}    
})
```

### 私有组件 components

```html
<template id="tem1">
    <div>
        <h1>test</h1>
    </div>
</template>


<script>
let app = new Vue({
    el: "#app",
    data: {},
    components: {
        login: {
            template: "#tem1"
        }
    }
})

```

引用方式同上， 使用组件名为标签名。

## 组件的data

```js
Vue.component("com1", {
  template: "#tem",
  data: function (){
      return {};
  }
    
})
```









<hr />



# Vue



## 事件监听

### V-On 参数传递

`@click="<menthod>"`  其中 method默认不写括号， 如果写括号意味传参，而不是调用。

当 method 无形参时，两种写法无区别。

当 method 有形参时：

无括号调用，第一个实参为`event`， 其余为 `undefiend` 。
	

有括号调用，实参支持字面量和 `vue.date`  如果需要事件对象使用`$event` 传入， 其余未传入参数皆为 `undefiend`。





## 条件判断

### v-if  v-else   v-else-if

`v-if` 条件为 `true` 时显示， 否则隐藏。  紧邻的`v-else ` 和他共享同一个表达式，可以省略不写。

`v-if` 的显示隐藏，是以创建和销毁DOM对象来实现的， 性能较 `v-show` 较差。

```html
<ul v-if="flag.isShow">
    <li v-for="(item, index) in list" index="index">{{item.name}}</li>
</ul>
<!--  这里共享上面的 flag.isShow 所以省略不写 -->
<ul v-else>
    <h1>这里没有数据！(๑•̀ㅂ•́)و✧</h1>
</ul>


<ul v-if="flag.showTest">
    <h1>show - if</h1>
</ul>
<ul v-else>
    <h1> else </h1>
</ul>
```



### v-show

`v-show` 和 `v-if` 类似， 区别在于 `v-show` 显示隐藏仅仅是添加删除 `display=none` 属性， 适合经常切换，有大概率会展示的DOM对象。



## 组件



### 父子组件传值

父组件使用`v-bind` 以HTML属性的形式赋值，子组件使用组件的`props` 定义接口变量， 接受传值。 未接收的变量可以使用`$attrs:Object` 获取



子传父：抛出使用自定义事件 `$emit`

```js
$emit(<"事件名称", args ...>)
```



### 父子组件引用

`$children` 数组类型， 一般用来获取所有组件。

```html
<script>
...
// this.$children[数组下标]
let vuwComponent = this.$children[?]
</script>
```



`$refs`  获取所有`<template>` 标签拥有 `ref` 属性的组件。

```html
<template ref="comp">
    ...
</template>

<script>
...
// this.$refs.<ref.Value>
let vueComponent = this.$refs.comp
...
</script>

```



`$parent:Object`

```js
// 获取当前组件的父组件。
let vueComponent = this.$parent
```

`$root` 在任何位置直接访问Vue实例（根组件

```js
this.$root...
```



### 插槽

#### `<slot>` 标签
定义：
```html
<template id="comp">
  ...
	<slot></slot>
  ...
</template>
```

使用：

```html
<!--  单个插槽 / 非命名插槽， 直接在组件标签内插入HTML元素 或者组件标签
	如果定义了多个组件，则会循环替换所有插槽。
-->
<main id="app">
	<comp>
        <div>......</div>
    </comp>
</main>
```



具名插槽：

```html
<main id="app">
	<comp>
    	<div slot="slot_1"></div>
    </comp>
</main>

.....

<template id="comp">
    <slot name="slot_1"></slot>
    <slot name="slot_2"></slot>
    <slot name="slot_3"></slot>
    <slot name="slot_4"></slot>    
</template>
```







#### `v-slot` 指令





#### 作用域插槽

在父组件内使用子组件的变量

模板内 `slot`  以属性的形式把内部变量暴漏给外部使用。

插槽内容需要使用 `div` / `template` 包裹，并添加属性 ``slot-scope="slot"``

使用`slot.XXX` 的形式使用传递的变量。

```html
<main id="app">
    ......
    <comp>
        <div slot-scope="slot">
            <span >{{slot.languages.join(' -- ')}}</span>
        </div>
    </comp>
    ......
</main>

<template id="comp">
    <div>
        <h2> QAQ </h2>
        <slot :languages="languages" >
            <ul>
                <li :key="index" v-for="(item, index) in languages">{{item}}</li>
            </ul>
        </slot>
    </div>
</template>

<script src="../static/lib/vue.js"></script>
<script>
    let app = new Vue({
        el: "#app",
        components: {
            "comp": {
                template: "#comp",
                data() {
                    return {
                        languages: [......]
                    }
                }
            }
        }
    })
</script>
```

