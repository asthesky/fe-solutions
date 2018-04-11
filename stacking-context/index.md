# 层叠上下文

## 基本概念
>层叠上下文是HTML元素的三维概念，这些HTML元素在一条假想的相对于面向（电脑屏幕的）视窗或者网页的用户的 z 轴上延伸，HTML 元素依据其自身属性按照优先级顺序占用层叠上下文的空间。 
z轴即用户与屏幕间看不见的垂直线。

## 触发层叠上下文
- 根元素 (HTML)
- position: fixed
- z-index值不为auto的flex项(父元素display:flex|inline-flex).
- 元素的opacity值不是1.
- 元素的transform值不是none.
- 元素mix-blend-mode值不是normal.(PS中的混合模式)
- 元素的filter值不是none.(PS中的滤镜)
- perspective值不是none.(3D元素距视图的距离)
- 元素的isolation值是isolate.(阻隔混合模式)
- will-change指定的属性值为上面任意一个.(强页面渲染性能)
- 元素的-webkit-overflow-scrolling设为touch.


## 层叠顺序
1. 形成层叠上下文环境的元素的背景与边框
1. 拥有负 z-index 的子堆叠上下文元素 （负的越高越堆叠层级越低）
1. 正常流式布局，非 inline-block，无 position 定位（static除外）的子元素
1. 无 position 定位（static除外）的 float 浮动元素
1. 正常流式布局， inline-block元素，无 position 定位（static除外）的子元素（包括 display:table 和 display:inline ）
1. 拥有 z-index:0 的子堆叠上下文元素
1. 拥有正 z-index: 的子堆叠上下文元素（正的越低越堆叠层级越低）


## 特性
- 层叠上下文的层叠水平要比普通元素高；
- 层叠上下文可以阻断元素的混合模式；
- 层叠上下文可以嵌套，内部层叠上下文及其所有子元素均受制于外部的层叠上下文。
- 每个层叠上下文和兄弟元素独立，也就是当进行层叠变化或渲染的时候，只需要考虑后代元素。
- 每个层叠上下文是自成体系的，当元素发生层叠的时候，整个元素被认为是在父层叠上下文的层叠顺序中。