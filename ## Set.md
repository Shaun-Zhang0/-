## Set

* 类似于数组，但是成员的值都是唯一的。没有重复的值。

Set是一个构造函数。用于生成Set数据结构。

Set实例有以下几种属性。

``` 
Set.prototype.constructor() 构造函数
Set.prototype.add() 添加某个值，返回Set结构。
Set.prototype.size() 返回Set实例的成员总数。
Set.prototype.delete() 删除某一个值。
Set.prototype.has() 表示Set成员中是否有该值 返回一个布尔型。
Set.prototype.clear() 清除Set所有的成员。
```

### Array与Set之间的转换

1. Array转换为Set

   

``` 
    let arr = [1,1,2,2,3,4,4]
    arr = new Set([...arr]);
    console.log(arr) // Set { 1, 2, 3, 4 }
   ```

2. Set转换为Array

    

``` 
    let set = new Set([1,2,3,4,5])
    set = Array.from(set);
    console.log([ 1, 2, 3, 4, 5 ]);
```

### Set遍历

* forEach遍历

``` 
let set = new Set([1,2,3,4,5])
set.forEach(function(key,value){
   console.log({key,value})
})
// 输出
{ key: 1, value: 1 }
{ key: 2, value: 2 }
{ key: 3, value: 3 }
{ key: 4, value: 4 }
{ key: 5, value: 5 }

```

* for of 遍历

``` 
let set = new Set([1,2,3,4,5]);
for(const item of set){
   console.log(item);
}
// 输出
1
2
3
4
5
```

### 并集、差集、交集的实现

* 并集的实现
```
let num1 = [1,2,3,4,5];
let num2 = [1,2,6,7,8];

// 获取并集
console.log(new Set([...num1,...num2]));
```
* 差集的实现

```
let num1 = new Set([1, 2, 3, 4, 5]);
let num2 = new Set([1, 2, 4, 7, 9]);

// 获取差集
console.log(Array.from(new Set([...num1].filter(function(item) {
    return !num2.has(item);
}))))
// 输出
[ 3, 5 ]
```

//交集的实现

```
let num1 = new Set([1, 2, 3, 4, 5]);
let num2 = new Set([1, 2, 4, 7, 9]);
console.log(Array.from(new Set([...num1].filter(function(item) {
    return num2.has(item);
}))))
// 输出
[ 1, 2, 4 ]
```
