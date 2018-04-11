# flexbox 布局

## 基本概念
> 弹性布局,容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）

## 容器的属性
___
`@flex-direction` 伸缩方向
- `row`：（默认值），主轴为水平方向，起点在左端。
- `row-reverse`：主轴为水平方向，起点在右端。
- `column`：主轴为垂直方向，起点在上沿。
- `column-reverse`：主轴为垂直方向，起点在下沿。
___
`@flex-wrap` 伸缩换行
- `nowrap`: （默认值），不换行。
- `wrap`：换行，第一行在上方。
- `wrap-reverse`：换行，第一行在下方。
___
`@flex-flow` @flex-direction @flex-wrap; 简写形式
___
`@align-iterms` 交叉轴对齐
- `flex-start`：交叉轴的起点对齐。
- `flex-end`：交叉轴的终点对齐。
- `center`：交叉轴的中点对齐。
- `baseline`: 交叉轴的项目文字基线对齐。
- `stretch`：（默认值），项目未设置高度，伸缩项目拉伸，填满整个侧轴
___
`@justify-content` 主轴对齐
- `flex-start`：（默认值），主轴的起始位置对齐。
- `flex-end`：主轴的结束位置对齐。
- `center`：主轴的中间位置居中对齐。
- `space-between`：两端对齐，项目之间的间隔相等。
- `space-around`: 每个项目两侧的间隔相等。项目之间的间隔比项目与边框的间隔大一倍。
___
`@align-content` 多行主轴对齐
- `flex-start`：与交叉轴的起点对齐。
- `flex-end`：与交叉轴的中点对齐。
- `center`：与交叉轴的中点对齐。
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
- `space-around`：各每根轴线两侧的间隔都相等。轴线之间的间隔比轴线与边框的间隔大一倍。
- `stretch`：（默认值）,轴线占满整个交叉轴。

## 项目的属性
___
`@order`：属性定义项目的排列顺序。默认为0 数值越小，排列越靠前。
___
`@flex-grow`：属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
___
`@flex-shrink`：属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
___
`@flex-basis`：属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
___
`@flex`:@flex-grow @flex-shrink @flex-basis;
