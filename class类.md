
## class 的基本用法
```js
//ES5
	function Point(x,y){
			this.x=x;
			this.y=y;
		}
		
		Point.prototype.toString=function(){
			return '('+this.x+','+this.y+')';
		};
		
		var p=new Point(1,2);
```
```js
//ES6
class Point {
				constructor(x, y) { //constructor 这是构造方法
					this.x = x; //this 关键字代表实例对象
					this.y = y;
				}

				toString() {
					return '(' + this.x + ',' + this.y + ')';
				}
			}

			var p = new Point(1, 2);
```

* ES5 的构造函数Point，对应 ES6 的Point类的构造方法。

* 方法之间不用，分隔
* ES6的类可以看作构造函数的另一种写法 (与构造函数用法相同)，类的数据类型是函数

* 类的所有方法都定义在类的 ``prototype`` 属性上面

```js
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```

* 类实例的生成要用 ``new``命令


* *注：* 类不存在变量提升
```js
new Foo(); // ReferenceError
class Foo {}
```
## Class 的继承

* 关于super
 
子类必须在constructor方法中调用super方法，否则新建实例时会报错。

这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。

如果不调用super方法，子类就得不到this对象。

* ES6 继承的实质
     * 先将父类实例对象的属性和方法，加到this上面（调用 ``super`` 方法）
     * 然后再用子类的构造函数修改this。

- 在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例。
- 其他时刻方法内部的`this`指向实例