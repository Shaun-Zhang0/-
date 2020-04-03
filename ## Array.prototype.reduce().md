## Array.prototype.reduce()

* 累加器

### 基本语法

``` 
array.reduce(回调函数,[初始值]);
如果没有第二个初始值的参数,则初始值会以数组的一个成员对象为准
```

### 举例： 

#### 初始值参数

``` 

let arr = [1,2,3,4]

arr.reduce(function(pre,cur,index,arr){
    console.log({pre,cur});
    return cur;
},0);
// 输出
{ pre: 0, cur: 1 }
{ pre: 1, cur: 2 }
{ pre: 2, cur: 3 }
{ pre: 3, cur: 4 }
{ pre: 4, cur: 5 }
```

* 数组长度为4 执行了5次

#### 没有初始值参数

``` 
arr.reduce(function(pre,cur,index,arr){
    console.log({pre,cur});
    return cur;
});

// 输出
{ pre: 1, cur: 2 }
{ pre: 2, cur: 3 }
{ pre: 3, cur: 4 }
{ pre: 4, cur: 5 }
```

* 数组长度为4 执行4次

### 运用场景

1. 统计数组成员出现的次数

``` 
function arrayCount(array, item) {
   return array.reduce(function(total, cur) {
        total += cur === item ? 1 : 0;
        return total;
    }, 0);
}

```

2. 统计数组成员中最大值

``` 
let arr = [1, 2, 3, 4, 5, 1, 2, 22];

function arrayMax(array){
    return array.reduce(function(prev,next){
        return prev>next?prev:next;
    })
}
console.log(arrayMax(arr));
```

3. 计算数组对象中的某个属性最大值

``` 
let cart = [{
        name: '张三',
        age: 15
    },
    {
        name: '李四',
        age: 20
    },
    {
        name: '王五',
        age: 25
    }
]

function maxAge(array){
    return array.reduce(function (pre,cur) {
        return pre.age>cur.age? pre:cur        
    })
}
console.log(maxAge(cart));
```