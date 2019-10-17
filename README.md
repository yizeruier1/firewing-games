# 基础
## JavaScript
+ **JS调用堆栈**

当 js 文件开始执行时，代码会以堆栈的方式来执行，栈底永远是**全局上下文**，栈顶永远是**正在执行的方法**，举个栗子：
```JavaScript
function a(){
    // 第一次输出 a , 第二次输出 b a
    console.trace()
}
function b(){
    // 输出 b
    console.trace()
    a()
}
a()
b()
```

分析一波：

![堆栈流程图](https://github.com/yizeruier1/firewing-games/blob/master/%E5%A0%86%E6%A0%88.png "堆栈流程图")

当代码执行时遇到**异步任务**时，异步任务会被**push**到异步任务队列，等当前同步代码执行完成后，才会去执行异步任务队列，即异步任务开始入栈依次执行，又一个栗子：
```JavaScript
setTimeout(() => {
    console.log(5)
}, 0)
function a(){
    console.log(2)
}
new Promise((resolve, reject) => {
    console.log(1)
    resolve(100)
}).then(res => {
    console.log(4)
})

a()
console.log(3)

// 依次输出 1 2 3 4 5，
```

因为定时器和 Promise 属于异步任务所以会被先 push 到异步任务队列，同步代码依次执行，Promise 的构造函数输出 1 然后方法 a 输出 2 然后输出 3，当执行异步队列时，微任务(Promise)会先于宏任务(setTimeout)执行，所以先输出 4 再输出 5

+ **Promise (async/await)**

Promise 出生于ES6，主要为了解决**回调地狱**问题。Promise 有三种状态，分别是：pending、resloved、rejected，且只能由 pengding 变为 resloved 或者 rejected。then 方法的返回值也是 Promise。应用举例：
```JavaScript
// 假设我们要在登陆成功的方法里 做一些操作
function login(){
    return new Promise((resolve, reject) => {
        axios.post('/login', {
            username: 'stephen',
            password: '123456'
        })
        .then(res => resolve(res))
        .catch(err => reject(err))
    })
}
function doSth(){
    // 登陆后的操作
}
login.then(res => doSth()).catch(err => {
    console.log(err)
})
```

async/await 出生于ES7，其实也只是 Promise 的语法糖，这样写看起来更像同步代码。await 后面的方法必须要返回一个 Promise ，且只能是带有 async 的方法。例：
```JavaScript
// 还是假设我们要在登陆成功的方法里 做一些操作
async function login(){
    return new Promise((resolve, reject) => {
        axios.post('/login', {
            username: 'stephen',
            password: '123456'
        })
        .then(res => resolve(res))
        .catch(err => reject(err))
    })
}
function doSth(){
    // 登陆后的操作
}
let res = await login()
if (res) doSth()
```

+ **原型继承和原型链**

原型继承
```JavaScript
// 方式 1、 Child 会继承 Parent 所有原型方法 和 构造函数中的属性
function Parent(name){
    this.name = name
}
Parent.prototype.sayHi = function(){
    console.log(`Hi ${this.name}`)
}

function Child(){}
Child.prototype = new Parent('Child')
let child = new Child()
console.log(child.name)
child.sayHi()

// 方式 2、原型继承原型方法、构造函数继承构造函数属性
function Child1(name){
    Parent.call(this, name)
}
Child1.prototype = Parent.prototype
Child1.prototype.constructor = Child1
let child1 = new Child1('Child1')
```

原型链：每一个对象都有一个 __proto__ 指针 指向它自己的原型对象，原型对象的 __proto__ 指向原型对象的原型对象，原型的终点是 Object.prototype，整个串起来就是原型链。实例会继承原型上的方法和属性。

## 其他
+ **TypeScript 高阶类型**

常用的高阶类型有 接口(interface) 和 联合类型吧。Demo：
```JavaScript
// 比如定义一个 RESTful 的返回值
interface res = {
    code: number,
    message: string,
    data: any
}

// 联合类型
type ns = number | string
const code: ns = 111

// 泛型
function getRes<T>(param: ns): <T>{
    return param + 1
}
console.log(getRes<string>('abc'))
```

## 开发应用
+ **Vue 生命周期**

从创建一个实例到销毁依次是：beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed，DOM 在 mounted 中渲染完成。

+ **Vue 的实现原理**

简单理解下：使用 Object.defineProperty 来劫持数据，重写 get set 方法，实现数据双向绑定。创建实例时 Compiler 来编译模板，替换 dom 里的数据，并生成一个观察者(watcher)存放到 Dep 中，Observre 负责实现数据劫持，当初发数据的 set 方法，数据发生改变时，会调用 Dep 的 notify 来通知 dom 更新。学习的时候做了个小图，暂时待补充：

![Vue 的实现原理](https://github.com/yizeruier1/firewing-games/blob/master/vue.png "Vue 的实现原理")

+ **Vue 父子组件交互**

父组件向子组件通信：
```JavaScript
// 父组件
<template>
    <div>
        <Child :msg="message" />
    </div>
</template>
<script>
    export default {
        name: "Parent",
        data: () => {
            message: "Hello World"
        }
    }
</script>

// 子组件
<template>
    <div>
        {{ msg }}
    </div>
</template>
<script>
    export default {
        name: "Child",
        props: {
            msg: {
                type: String,
                default: ""
            }
        }
    }
</script>
```

子组件向父组件通信：
```JavaScript
// 父组件
<template>
    <div>
        <Child @getMsg="deal" />
    </div>
</template>
<script>
    export default {
        name: "Parent",
        methods: {
            deal(msg){
                console.log(msg)
            }
        }
    }
</script>

// 子组件
<template>
    <div>
        <button @click="$emit('getMsg', 'Hello')">click to send Parent</button>
    </div>
</template>
<script>
    export default {
        name: "Child"
    }
</script>
```

+ **CSS 规范**

比较常用 stylus ...  规范比较欠缺，因为之前待的小公司。

## Vue 实现 TodoList

todo 我用 cli 的形式写的，您可以 [在线体验](http://www.gostephen.cn/todoList) 或者 [查看代码](https://github.com/yizeruier1/firewing-games/blob/master/todoList.vue)