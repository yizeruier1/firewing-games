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

async/await 出书于ES7，其实也只是 Promise 的语法糖，这样写看起来更像同步代码。await 后面的方法必须要返回一个 Promise ，且只能是带有 async 的方法。例：
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

## 其他
+ **TypeScript 高阶类型**

## 开发应用
+ **Vue 生命周期**
+ **Vue 的实现原理**
+ **Vue 父子组件交互**
+ **CSS 规范**

## Vue 实现 TodoList