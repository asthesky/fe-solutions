
# 1px 解决方案


## Border: 0.5px

* 优点
1. 简单

* 缺点
1. 兼容性差，目前只有少部分浏览器支持
2. 3倍屏,全部阵亡

想让代码兼容性更强，可以这么写

```js
if (window.devicePixelRatio && devicePixelRatio >= 2) {
  var testElem = document.createElement('div');
  testElem.style.border = '.5px solid transparent';
  document.body.appendChild(testElem);
  if (testElem.offsetHeight == 1){
    document.querySelector('html').classList.add('hairlines');
  }
  document.body.removeChild(testElem);
}
```
然后再通过`hairlines`类名调成大小。

## Scale up and down

* 优点
1. 绝对的1px
2. 可圆角，但是看起来不舒服

* 缺点
1. 如果页面结合rem可能会’露馅’
2. 需要把内容放大再缩放，但元素的占位不变，通常用伪元素解决

```css
.absolute-1px {
    display: block;
    &.border-all {
        &:after {
            border: 1px solid #ebebeb;
        }
    }
    &.border-top {
        &:after {
            border-top: 1px solid #ebebeb;
        }
    }
    &.border-right {
        &:after {
            border-right: 1px solid #ebebeb;
        }
    }
    &.border-bottom {
        &:after {
            border-bottom: 1px solid #ebebeb;
        }
    }
    &.border-left {
        &:after {
            border-left: 1px solid #ebebeb;
        }
    }
    &:after {
        content: ' ';
        position: absolute;
        left: 0;
        top: 0;
        box-sizing: border-box;
        transform-origin: 0 0;
        z-index: 2;
        font-size: 0;
        pointer-events: none;
        @media only screen and (-webkit-min-device-pixel-ratio: 1.5) {
            width: 150%;
            height: 150%;
            transform: scale(0.66666);
        }
        @media only screen and (-webkit-min-device-pixel-ratio: 2) {
            width: 200%;
            height: 200%;
            transform: scale(0.5);
        }
        @media only screen and (-webkit-min-device-pixel-ratio: 3) {
            width: 300%;
            height: 300%;
            transform: scale(0.33333333);
        }
    }
}
```
注： 实现的时候需要通过 js 判断 `devicePixelRatio`,或者css的 `-webkit-device-pixel-ratio`, `-webkit-min-device-pixel-ratio`, `-webkit-max-device-pixel-ratio`来判断加载不同的资源（下面其他例子类似，不再描述）

## Box shadow

* 优点
1. 使用方便，直接作用在wrapper上
2. 不需要额外的元素
3. 可以圆角

* 缺点
1. 偏好性能
2. 只是模拟，不是1px
3. 边缘会模糊

```css
box-shadow: 0 1px 1px -1px rgba(200, 199, 204, .5);
```

## Linear-gradient

* 优点
1. 简洁
* 缺点
1. 兼容性差
2. 不能圆角

```css
background-size: auto 1px;
background-image: linear-gradient(to bottom, blue 0%, blue 51%, transparent 51%);
```
## Svg, Img

* 优点
简单

* 缺点
1. 无法圆角
2. 麻烦，修改颜色需要去图形工具内编辑
3. 压缩图片会导致图片出现边界色差

```css
.svg {
    background: repeat-x top left url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1' height='1'><rect fill='#ff0000' x='0' y='0' width='1' height='0.5'/></svg>");
}
.img {
    background: repeat-x bottom left
      url(data:image/gif;base64,R0lGODlhAQACAPABAMjHzP///yH/C1hNUCBEYXRhWE1QAz94cAAh+QQFAAABACwAAAAAAQACAAACAgwKADs=);
    background-size: 100% 1px;
}
```

结合`border-image`可以实现四条边都是物理像素1px

## Border image
>准备一张8*8的小图，将中间6*6区域挖空.
[absolute-1px-slided](./absolute-1px-slided.png)

```css
border-width: 1px;
border-image: url(border.gif) 2 repeat;
```
可以使用工具`border-image`来帮你生成代码

所有解决方案中最适用的应该是`transform`的方案
```less
/*retina 1px border start*/
.retinabt,.retinabb,.retinabl,.retinabr,.retinab { position: relative;}
.retinabt:before,.retinabb:after {pointer-events: none;position: absolute;content: ""; height: 1px; background: rgba(32,35,37,.24);left: 0;right: 0}
.retinabt:before {top: 0}
.retinabb:after {bottom: 0}
.retinabl:before,.retinabr:after {pointer-events: none;position: absolute;content: ""; width: 1px; background: rgba(32,35,37,.24); top: 0; bottom: 0}
.retinabl:before {left: 0}
.retinabr:after {right: 0}
.retinab:after {position: absolute;content: "";top: 0;left: 0; -webkit-box-sizing: border-box; box-sizing: border-box; width: 100%; height: 100%; border: 1px solid rgba(32,35,37,.24); pointer-events: none}

@media (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5),(min-resolution: 144dpi),(min-resolution:1.5dppx) {
.retinabt:before,.retinabb:after {-webkit-transform:scaleY(.5);transform: scaleY(.5) }
.retinabl:before,.retinabr:after {-webkit-transform: scaleX(.5); transform: scaleX(.5) }
.retinab:after { width: 200%; height: 200%;-webkit-transform: scale(.5); transform: scale(.5) }
.retinabt:before,.retinabl:before,.retinab:after {-webkit-transform-origin: 0 0;transform-origin: 0 0}
.retinabb:after,.retinabr:after { -webkit-transform-origin: 100% 100%;transform-origin: 100% 100%}
}

@media (-webkit-device-pixel-ratio:1.5) {
.retinabt:before,.retinabb:after { -webkit-transform: scaleY(.6666); transform: scaleY(.6666) }
.retinabl:before,.retinabr:after {-webkit-transform: scaleX(.6666); transform: scaleX(.6666)}
.retinab:after {width: 150%; height: 150%;-webkit-transform: scale(.6666); transform: scale(.6666) }
}

@media (-webkit-device-pixel-ratio:3) {
.retinabt:before,.retinabb:after { -webkit-transform: scaleY(.3333); transform: scaleY(.3333)}
.retinabl:before,.retinabr:after { -webkit-transform: scaleX(.3333); transform: scaleX(.3333)}
.retinab:after {width: 300%;height: 300%; -webkit-transform: scale(.3333);transform: scale(.3333)}
}
```