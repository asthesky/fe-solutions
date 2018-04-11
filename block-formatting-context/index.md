#块级格式化上下文

#基本概念
>具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

#触发BFC
- float的值不为none。
- overflow的值为auto,scroll或hidden。
- display的值为table-cell, table-caption, inline-block中的任何一个。
- position的值不为relative和static。

#BFC特性及应用

1. 同一个 BFC 下外边距会发生折叠
```
块状元素的margin to bottom 会按最大的值来合并
```
1. BFC 可以包含浮动的元素
```
生成bfc可以将脱离文档流的元素包进去（清除浮动）
```
1. BFC 可以阻止元素被浮动元素覆盖
```
生成bfc可以元素与浮动兄弟元素并列
```


