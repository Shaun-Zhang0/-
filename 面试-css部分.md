# CSS

## 盒子模型
```
* 标准盒子模型:

width = content+padding+border+margin;

* 怪异盒子模型 (IE盒子模型)

width = content + margin;
```
css 属性 box-sizing
content -> 标准盒子模型
border -> IE盒子模型

## 0.5px 的线
```
1. 使用meta viewport
<meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
2. 使用border-image
3. transform:scale()
```

## link 和 @import标签

1. <link>属于html标签 @import 是css提供的
2. 当页面加载时 link会被同时加载, 而@import引用的css 则会等到页面资源加载完成后加载
3. @import 只有IE5以上支持
4. link样式权重大于@import

## BFC 块级格式化上下文 
1. 是一个独立渲染区域, 并且有一定布局的规则
2. 子元素不会影响到外面的元素
3. 计算高度时 浮动元素的高度也会计算在内
触发条件:
```
1. float不为none 的元素
2. position为fixed 和 absolute 的元素
3. inline-block、table-cell、table-caption，flex，inline-flex的元素
4. overflow不为visible的元素
```

## 垂直居中
1. margin: auto
   ```
    position: absolute;
    left:0;
    right:0
    top:0;
    bottom:0;
    margin: auto;

   ```
2. margin 负值法
   ```
    position: absolute;
    width:100px;
    height: 100px;
    margin-left: -50px;
    margin-right: -50px;
    top: 50%;
    left: 50%;

   ```

3. table-cell (未脱离文档流)
   ```
    父元素:{
        position: display:table-cell;
        vertical-align: middle;
    }
   ```
4. flex
   ``` 
   父元素:{
       display:flex;
       align-items:center;
       justify-content:center;
   }
   ```

## 块级元素和内联元素

1. 块级元素独占一行, 并且自动填满整个父元素 内联元素不会独占一行
2. 内联元素高度和宽度均失效 并且在垂直方向的padding和margin会失效

## 隐藏元素的方法
1. opacity:0; 
   将元素隐藏起来, 但是不会改变页面布局, 如果有绑定的事件 同样可以触发
2. visibility = hidden
   将元素隐藏起来 但不会改变页面布局, 并没有脱离文档流,  但是点击不会触发绑定的事件
3. display:none
   将元素隐藏起来, 并且会改变布局. 直接脱离文档流


## 重绘 和 回流
1. 回流 - render树中因为大小边距发生改变而导致重建的过程叫做回流
2. 重绘 - 当元素一部分属性发生变化时, 不会引起布局变化而需要重新渲染的过程叫做重绘
3. display: none 会导致回流和重绘 visibility:hidden 只会导致重绘