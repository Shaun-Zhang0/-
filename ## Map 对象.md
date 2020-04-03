## Map 对象

* 因为js的对象，本质上是键值对的集合。但只能是用字符串当做键名。

``` 
let key = {p: 'Hello World'}
let obj1 = {key:'aaa'}
let obj2 = {[key]:'aaa'}
console.log(obj1);
console.log(obj2);
// { key: 'aaa' }
// { '[object Object]': 'aaa' }
```

* js的对象Object结构提供了"字符串和值一一对应", Map结构提供值和值一一对应，是一种更加完善的Hash结构实现。

``` 
let key = {p: 'Hello World'}
let obj1 = new Map();
obj1.set(key,'aaa');
let obj2 = {[key]:'aaa'}
console.log(obj1); 
console.log(obj2);
// Map { { p: 'Hello World' } => 'aaa' }
// { '[object Object]': 'aaa' }
```

* 与Set一样，Map的构造函数也可以接收一个数组作为参数。该数组的成员是一个个表示键值对的数组。
```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

/*******实现方式如下********/
let params = [
    ['name', '张三'],
    ['title', 'Author']
];
let obj = new Map();
params.forEach(([key, value]) => {
    obj.set(key,value)
});
console.log(obj);
```

* Map属性
```
Map.prototype.set() 添加键值对或者更新键值对 返回Map对象
Map.prototype.get() 获取键值对
Map.prototype.has() 判断是否有该键值对
Map.prototype.delete() 删除键值对
Map.prototype.size() 返回Map结构的成员数量



```
