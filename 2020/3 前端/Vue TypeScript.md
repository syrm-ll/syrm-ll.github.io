## 组件

```ts
import { Component, Vue } from 'vue-property-decorator'

@Component
export default class HelloWorld extends Vue {

}
```

等价于:

```js
export default {
name: 'HelloWorld'
    
}
```





## 组件注册

```ts
import { Component, Vue } from 'vue-property-decorator'
import Project from '@/components/Project.vue'

@Component({
  components: {
    project
  }
})
export default class HelloWorld extends Vue {
    
}
```

等价于:

```js
import Project from '@/components/Project.vue'
export default {
  name: 'HelloWorld',
  components: {
    project
  }
})
```



### Vue 属性

#### Data

```ts
@Component
export default class HelloWorld extends Vue {
  
  private msg: string = "welcome to my app"
  private list: Array<object> = [
    { name: 'Preetish',　age: '26'},
    { name: 'John', age: '30' }
  ]
}
```

等价于: 

```js
export default {
  data() {
    return {
      msg: "welcome to my app",
      list: [
	    { name: 'Preetish',　age: '26'},
    	{ name: 'John', age: '30' }
      ]
    }
}
```

#### props

```ts
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class HelloWorld extends Vue {
  @Prop() readonly msg!: string
  @Prop({default: 'John doe'}) readonly name: string
  @Prop({required: true}) readonly age: number
  @Prop(String) readonly address: string
  @Prop({required: false, type: String, default: 'Developer'}) readonly job: string
}
```

等价于:

```js
props: {
    msg,
    name: {
      default: 'John doe'
    },
    age: {
      required: true,
    },
    address: {
      type: String
    },
    job: {
      required: false,
      type: string,
      default: 'Developer'
    }
  }
}
```

#### Computed

```ts
export default class HelloWorld extends Vue {
  get fullName(): string {
    return this.first+ ' '+ this.last
  }
}

// 或者
export default class HelloWorld extends Vue {
  get fullName(): string {
    return this.first+ ' '+ this.last
  }
  set fullName(newValue: string) {
    let names = newValue.split(' ')
    this.first = names[0]
    this.last = names[names.length - 1]
  }
}

```

等价于:

```js
export default {
  fullName() {
    return this.first + ' ' + this.last
  }
}
// 或者

fullName: {
  get: function () {
    return this.first + ' ' + this.last
  },
  set: function (newValue) {
    let names = newValue.split(' ')
    this.first = names[0]
    this.last = names[names.length - 1]
  }
}

```

#### Methods

```ts
export default class HelloWorld extends Vue {
  public clickMe(): void {
    console.log('clicked')
    console.log(this.addNum(4, 2))
  }
  public addNum(num1: number, num2: number): number {
    return num1 + num2
  }
}

```

等价于: 

```js
export default {
  methods: {
    clickMe() {
      console.log('clicked')
      console.log(this.addNum(4, 2))
    }
    addNum(num1, num2) {
      return num1 + num2
    }
  }
}
```



#### Watchers

```ts
@Watch('name')
nameChanged(newVal: string) {
  this.name = newVal
}

// 或
@Watch('project', { 
  immediate: true, deep: true 
})
projectChanged(newVal: Person, oldVal: Person) {
  // do something
}
```

等价于:

```js
watch: {
  person: {
      handler: 'projectChanged',
      immediate: true,
      deep: true
    }
}
methods: {
  projectChanged(newVal, oldVal) {
    // do something
  }
}

```



#### Emit

```ts
@Emit()
addToCount(n: number) {
  this.count += n
}
@Emit('resetData')
resetCount() {
  this.count = 0
}

```



#### 生命周期回调

没变化~





