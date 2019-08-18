## 与传统写法的对比
* commonJS
```js
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```
1. 实质先整体加载fs模块(加载fs所有的方法)
2. 生成一个对象，在对象上读取3个方法
3. 运行时加载，只有在运行的时候才能得到这个对象
4. 输出的是缓存的值，不存在动态更新

* export 暴露输出   import输入
```js
// ES6模块
import { stat, exists, readFile } from 'fs';
```
1. 实质是从fs模块加载3个方法，其他方法并不加载
2. 编译时加载
3. 存在动态更新

*注意：``ES6 的模块自动采用严格模式``，严格模式下，禁止this指向全局对象。

    * ES6 模块之中，顶层的this指向undefined，即不应该在顶层代码使用this。

## 模块化概念

一个模块就是一个独立的文件。文件内部的所有变量，外部无法获取。

#### export
你可以通过使用export 暴露出你想要外部访问的变量

可以通过as关键字来重名变量名 （原变量 as 重名后）

* html中引入js模块
```html
<script src="./two.js" type="module"></script>  <!-- 模块引入形式 -->
<script nomodule src="./two.js"></script>       <!-- 如果编译器不支持ES6语法将转化为ES5编译 -->
```

* export引用位置
    *  export命令可以出现在模块的任何位置，只要处于模块顶层就可以。
    *  如果处于块级作用域内，就会报错，
```js
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```
*注：下面的写法会报错
```js
// 报错
export 1;

// 报错
var m = 1;
export m;
```

#### import 

使用export命令定义了模块的对外接口以后，其他 JS 文件就可以通过 ``import命令加载这个模块``。

* import中可以使用as关键字，将输入的变量重命名
* import命令具有提升效果，会提升到模块的头部，首先执行
* import 执行所加载的模块
```js
import 'lodash';   //仅仅执行该模块  但是不输入任何值
```
* 因为import在静态解析阶段执行，所以它是一个模块之中 ``最早执行`` 的。
* 模块的整体加载 ``*``
```
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}


// main.js-----------传统的写法------------------

import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

//------------------整体加载 *------------------
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
```