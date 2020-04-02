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

Array与Set之间的转换

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
