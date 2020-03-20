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

## Generator(生成器)

* Generator 是ES6标准引入的新的数据类型。一个Generator看上去像一个函数 但是函数执行过程中可以停止。
* 最大的特点就是可以交出函数的执行权(暂停执行)
* 以一种看似顺序执行 同步的方式实现了异步控制的流程, 增强了代码的可读性。

```
  声明方式
    function *main(){
      // code
    }
    function* main(){
      // code
    }
```


### generator函数的数据交换和错误处理

```
  
    function* gen(x){
      var y = yield x + 2;
      return y;
    }

    var g = gen(1);
    g.next() // { value: 3, done: false }
    g.next(2) // { value: 2, done: true }

```
调用generator函数时, 会返回一个内部指针g。调用指针g的next方法，会移动指针指向第一个yield。
* next 方法返回值的 value属性是 generator函数向外输出的数据, next() 方法还可以接受参数 这是向generator函数体内输入数据。


### next()

```
next方法的作用是分阶段执行Generator函数。每次调用next(),会返回一个对象,表示当前阶段的信息({value属性,done属性})。value属性时yield语句后面表达式的值, 表示当前阶段的值; done属性是一个布尔值, 表示generator函数是否执行完毕, 即是否还有下一个阶段。
```
* next() 会永远比yield多出一个, 因为var g = gen(1)实例化后 代码是不会主动执行的, 第一个next() 永远用于启动器。

### 消息传递

* generator生成器的作用之一就是消息传递。通过yield和next()的组合构成一个双向通信的系统
* yield => next()
* next(i) => 前一个yield

```
function *main() {
    let x = yield "starting";
    let y = yield (x * 2);

    console.log(x, y);
    return x + y;
}

let it = main();

let res = it.next(); // 第一个next()用于启动生成器
console.log(res.value);  // 输出"starting" （yield语句后跟的值传给了next()的对象）

res = it.next(5); // 向等待的第一个yield传入值5，*main()中的 x 被赋值为5
console.log(res.value); // 输出10 （x * 2得到了10传给next(5)运行后的对象）

res = it.next(20); // 向等待的第二个yield传入值20， *main()中的x被赋值为20
                   // 输出5 20    （执行后面的console.log(x, y)语句分别输出x,y的值）
console.log(res.value); // 输出25  (return ...的值传给了next(20)运行后的对象)

```

### generator在业务中的应用(结合promise)

```
function getUserInfo() {
   return new Promise((resolve, reject) => {
        resolve();
    })
}
function *dealData() {
    try {
        let setUser = yield getUserInfo();
        // other code
    } catch (error) {
        throw new Error(error);
    }    
}
let data = dealData();
let getInfo = data.next().value;

getInfo.then(res=>{
    data.next(res) // 根据generator的yield 和 next 的双向消息传递机制 传回到generator生成器的内部的上一个yield中,便于下一步的业务处理
}).catch(err=>{
    throw new Error(err);
})
```
### yield 委托

* 处于功能模块化等原因 很有可能在一个生成器中去调用另外一个生成器比如 a() 中 调用b(), 但是通常情况下 在a()的实例中是无法使用next方法对b()内部进行调用的, 这时候 我们就可以通过yeild将b() 委托给a();

```
function *a() {
    console.log('a start');
    yield 2;
    console.log('a end');
}
function *b() {
    console.log('b start');
    yield 1;
    yield *a(); // 将a委托给b
    yield 3;
    console.log('b end');
}

let it = b();

it.next().value;    // b start
                    // 1 
it.next().value;    // a start
                    // 2
it.next().value;    // a end
                    // 3
it.next().value;    // b end

`

