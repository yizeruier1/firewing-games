# 基础
### JavaScript
+ JS调用堆栈

当 js 文件开始执行时，代码会以堆栈的方式来执行，栈底永远是**全局上下文**，栈顶永远是**正在执行的方法**，举个栗子：

```JavaScript
function a(){
    console.trace()
}
function b(){
    console.trace()
    a()
}
a()
b()
```

分析一波：

![堆栈流程图](https://github.com/yizeruier1/firewing-games/blob/master/%E5%A0%86%E6%A0%88.png "堆栈流程图")

+ Promise (async/await)
+ 原型继承和原型链

### 其他
+ TypeScript 高阶类型

### 开发应用
+ Vue 生命周期
+ Vue 的实现原理
+ Vue 父子组件交互
+ CSS 规范

### Vue 实现 TodoList