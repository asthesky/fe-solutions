#modularity

>模块化是一种处理复杂系统分解成为更好的可管理模块的方式，它可以把系统代码划分为一系列职责单一，高度解耦且可替换的模块，系统中某一部分的变化将如何影响其它部分就会变得显而易见，系统的可维护性更加简单易得。

## 命名空间模块化
这是一种比较早的模块化方法，基本上实现的方式是全局注册一个变量，单例或者继承，通过this访问不同的方法和属性。能够避免污染更多的全局变量。特别像一些基础的类库都是通过这种方式实现。这种方式基本上无法实现动态依赖加载。
```js
module1.fn={};
module1.fn.Utils={};
module1.fn.Utils.module=function(){
    console.log("I am module1.js");
}
//
jQuery.fn.exntend1 = function(){}
```

## CommonJs模块化
CommonJS是一种规范思想，它的终极目标是使应用程序开发者根据CommonJS API编写的JavaScript应用可以在不同的JavaScript解析器和HOST环境上运行。Nodejs就是基于这种规范实现的。
1. 所有代码都运行在模块作用域，不会污染全局作用域。
2. 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
3. 模块加载的顺序，按照其在代码中出现的顺序。

```js
var path = require('path');
module.exports = {}
```

## AMD模块化(RequireJs)
AMD是Asynchronous Module Definition的缩写，意思就是异步模块定义。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

```js
 //定义模块
define('name',['dep1','dep2'],function(dep1,dep2){...});
//初始调用
require(['dep1','dep2'], function (dep1,dep2) {...});
```

## CMD模块化(SeaJs)
淘宝的玉伯写了seajs，就是遵循他提出的CMD规范，与AMD不同的CMD的依赖模块是像commonjs一样是当代码执行到模块的require时，才动态执行依赖模块中的内容。

```js
//定义模块
define('name',['dep1','dep2'],function(require,exports,module){...});
//初始调用
seajs.use(['dep1','dep2'], function(dep1,dep2) {...});
```

##UMD模块化
CommonJS与AMD分属于浏览器端和服务器端，有些代码想要在两端通用，因而出现了一种写法，它兼容了AMD和CommonJS，同时还支持老式的“全局”变量规范。

```js
(function (factory) {
    var root = (function () { return this }).call();
    var moduleName = 'name';
    if (typeof define === 'function') {
        if (define.amd) {//amd
            define(moduleName,['jquery'], factory);
        }
        if (define.cmd) { //cmd 
            define(moduleName,["jquery"], function (require, exports, module) {
                var $ = require("jquery");
                return factory($);
            });
        }
    } else if (typeof exports === 'object') { // commonjs for pack
        module.exports = factory();
    } else {
        root[moduleName] = factory(root.jQuery || root.$);
    }
})(function ($) {...})
```

## ES2015模块化
modules是ES6引入的最重要一个特性，通过export关键字将变量或者函数导出，通过default关键字可以设置为默认导出


```
//依赖模块
export const sqrt = name
...
export default {
    name: 'test',
    say(){
        console.log(this.name);
    }
}
...
export { name as default };

//引用模块
import test from './module'
...
import * as lib from './module'
...
import {name} lib from './module'
...
import { default as foo } from 'lib';
```