## 箭头函数 和 普通函数的区别
```
1. 箭头函数是匿名函数 , 不能作为构造函数, 不能使用new关键字实例化
2. 箭头函数不绑定argument获传进来的参数, 可以是用es6的扩展符号...
3. 箭头函数不绑定this, 它会捕获其上下文中的this 作为自己的this值
4. 箭头函数 没有原型属性
5. 箭头函数使用call()或者apply() 调用一个函数时 , 只传入了一个参数 , 对this并没有影响。
6. 普通函数的this指向是调用它的那个对象
```
## Promise
* js的代码是单线程的 所以导致js的所有网络请求 浏览器事件都必须是异步执行的。 
* Promise是异步编程的一种解决方案

特点：
```
1. 对象的状态不受外界影响。
    promise对象代表着一个异步操作，有三种状态 pending fulfilled reject。 只有异步操作的结果可以决定是哪一种状态, 任何其他操作都无法改变这个状态
2. 状态一旦改变 就不会再变了, 任何时候都是可以得到这个结果的。Promise对象的状态改变只有两种情况pending -> rejected 和 pending -> fulfilled
3. 为了解决回调地狱


```

### promise的使用
```
let p = new Promise((resolve, reject) => {
  // 做一些事情
  // 然后在某些条件下resolve，或者reject
  if (/* 条件随便写^_^ */) {
    resolve()
  } else {
    reject()
  }
})

p.then(() => {
    // 如果p的状态被resolve了，就进入这里
}, () => {
    // 如果p的状态被reject
})
```
#### then()
```
1. 用于为promise对象状态注册回调函数,他会返回一个promise对象, 因此可以链式调用, 避免了回调地狱。
2. then()提供了两个回调函数, 第一个是异步操作成功时 状态更改为fulfilled时 的回调函数。第二个是异步操作失败时 状态更改为rejected时的回调函数。状态一旦改变，就调用对应的回调函数。

```

#### then()的链式调用
```
p1
  .then(step1)
  .then(step2)
  .then(step3)
  .then(
    console.log,
    console.error
  );
```
* 只要前一步的状态修改为fulfilled, 就会依次执行后面的回调函数。
* 最后一个then()方法 console.log() 只会显示step3的返回值, 而console.error() 则是可以显示p1 step1 step2 step3 中的任意一个发生的错误。所以说promise对象的报错是具有传递性的。

#### promise.then第二个回调函数和catch的区别

1. 如果then的第一个回调函数抛出异常 第二个回调函数不会捕获到异常 但是catch可以捕获到。
2. then第二个回调函数和catch捕获异常的时候 是采取就近原则，当promise实例的状态更改为rejected的时候。第二个回调函数和catch同时存在的情况下，只有第二个回调函数会捕获到。如果没有第二个回调函数，则catch会捕获到

## Generator

* Generator 是ES6标准引入的新的数据类型。一个Generator看上去像一个函数 但是函数执行过程中可以停止。

