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

```
## react

### 生命周期
* 挂载和卸载
1. constructor()
   完成了React数据初始化，它接受两个参数：props和context，当想在函数内部使用这两个参数时，需使用super()传入这两个参数。
2. componentWillMount()
   它表示组件经过了constructor()初始化后, 但是还没有渲染dom时。
3. componentDidMount() 
   组件第一次渲染完成，此时dom节点已经生成，可以在这里调用ajax请求，返回数据setState后 组件会重新渲染
4. componentWillUnmount()
   在此处完成组件的卸载和数据的销毁。
   清除定时器，移除监听器

* 更新过程
1. componentWillReceiveProps(nextProps)
   在接受父组件改变后的props需要重新渲染组件时用到的比较多
   接受一个参数nextProps
2. shouldComponentUpadte(nextProps,nextState)
   主要用于性能优化(部分更新)
   唯一用于控制组件重新渲染的生命周期, 由于在react中, setState以后, state发生改变, 组件就会进入重新渲染流程,在这里return false 可以阻止组件的更新
   因为react父组件的重新渲染会导致其所有的子组件的重新渲染。这个时候我们是不需要所有的子组件都跟着重新渲染的，因此需要子组件的该生命周期中做出判断
3. componentWillUpdate(nextProps,nextState)
   shouldComponentUpdate返回true以后, 组件进入重新渲染的流程, 进入componentWillUpdate, 这里同样可以拿到nextProps和nextState。
4. componentDidUpdate(prevProps,prevState)
   组件更新完毕后, react只会第一次初始化成功会进入componentDidMount, 之后渲染只会进入到componentDidUpdate这个生命周期, 这里可以拿到更新前的props和state
5. render()
   render函数会插入jsx生成dom结构, react会生成一份虚拟dom树,在每一次组件更新时,再此react 会通过其diff算法比较更新前后的新旧dom树, 比较以后, 找到最小的有差异的dom节点

* 新增的生命周期函数
1. getDerivedStateFromProps(nextProps,preState) 
   这个生命周期主要是用于替代componentWillReceive(nextProps)
   在componentWillReceiveProps中, 我们通常会做两件事, 一是根据props来更新state, 二是触发一些回调, 如动画或者页面跳转
   在新的版本中, 官方将更新模块分给了getDerivedStateFromProps,触发回调分给了componentDidUpdate(), 并且在getDerivedStateFromProps中禁止组件去访this.props, 强制让开发者去比较nextProps和prevState的值。以确保开发者调用这个生命周期时， 就是根据当前的props来更新组件的state，而不是让一些组件自身的状态变得更加不可预测。
2. getSnapshotBeforeUpdate
   主要解决react 开启异步渲染模式后，componentDidUpdate中使用componentWillUpdate中读取的dom元素状态可能是不安全的。
   所以使用getSnapshotBeforeUpdate替代了componentWillUpdate
   因为getSnapshotBeforeUpdate会在最终的render之前被调用, 保证了getSnapshotBeforeUpdate获取到的dom值可以保证和componentDidUpdate中的一致。

   ![avatar](https://upload-images.jianshu.io/upload_images/1888124-b29f9bdbd107d51e.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
## 创建单向链表
```
// 节点类
function Node(element) {
    this.element = element; // 存储数据部分
    this.next = null; // 存储下一个节点的地址
}

function LinkList() {
    this.head = new Node('head');
    this.find = findNode;
    this.insert = insertNode;
    this.remove = removeNode;
    this.showNode = showNode;
    this.findPrevNode = findPrevNode;
}
/**
 * 查找节点
 * @param element 节点element值
 */
function findNode(item) {
    var currentNode = this.head; // 获取当前链表的头节点
    /**
     * 判断节点的element是否与要找的节点相同
     * 是 - 跳出循环
     * 否 - 通过当前节点的next找到下一个节点的地址 继续判断
     */
    while (currentNode.element != item) {
        currentNode = currentNode.next;
    }
    return currentNode; // 返回节点

}

/**
 * 
 * @param element 
 */
function findPrevNode(element) {
    var currentNode = this.head; // 获取当前链表的头结点
    /**
     * 判断当前节点的next指针的指向是否为element
     * 是 - 返回当前节点
     */
    while (currentNode.next !== null && currentNode.next.element !== element) {
        currentNode = currentNode.next;
    }
    return currentNode;
}

function removeNode(element) {
    var prevNode = this.findPrevNode(element);
    console.log(prevNode);

    if (prevNode.next !== null) {
        prevNode.next = prevNode.next.next
    }
}

/**
 * @param newElement 插入节点的element值
 * @param prevElement 插入节点的上一节点的值
 */
function insertNode(newElement, prevElement) {
    var newNode = new Node(newElement);
    var currentNode = this.find(prevElement);
    newNode.next = currentNode.next;
    currentNode.next = newNode;
}

function showNode() {
    var currentNode = this.head;
    /**
     * 判断是否当前节点的next是否为空
     * 不为空, 打印下一个节点的值
     * 为空, 则表示链表结束
     */
    var str = '';
    while (currentNode.next !== null) {
        // str += currentNode.next.element + '->';
        console.log(currentNode.next.element);
        currentNode = currentNode.next;
    }
    console.log(str);
}
var fruits = new LinkList();
fruits.insert('Apple', 'head');
fruits.insert('Banana', 'Apple');
fruits.insert('Pear', 'Banana');

fruits.showNode();
fruits.remove('Banana');
fruits.showNode();
```


```
