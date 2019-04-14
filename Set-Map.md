# Set和Map数据结构

## Set
类似于数组，但是成员的值都是唯一的，没有重复的值。(Set本身就是一个构造函数，用来生成Set数据结构)

``forEach``

     arr.forEach(callback[, thisArg]);
forEach 方法按升序为数组中含有效值的每一项执行一次callback 函数

* 数组去重

```js
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x))
console.log(s);    //Set(4) {2, 3, 5, 4}  数组

const set = new Set([2, 3, 5, 4, 5, 2, 2]);
console.log([...set])   //(4) [2, 3, 5, 4]  Set

```
数组去重
```js
[...new Set(array)]  
```
字符串去重
```js
console.log([...new Set('aabcaab')].join(''));  //abc
```
``join()``
join() 方法用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的。

* 两个对象不相等，他们被视为两个值  （注意：引用值与原始值）

```js
let set = new Set();

set.add({});
set.size // 1

set.add({});
set.size // 2
```


* 向 Set 加入值的时候，不会发生类型转换


## Set实例上的属性和方法
* Set 结构的实例有以下属性。

``Set.prototype.constructor``：构造函数，默认就是Set函数。
``Set.prototype.size``：返回Set实例的成员总数。


* Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。


    * 操作方法：
``add(value)``：添加某个值，返回 Set 结构本身。
``delete(value)``：删除某个值，返回一个布尔值，表示删除是否成功。
``has(value)``：返回一个布尔值，表示该值是否为Set的成员。
``clear()``：清除所有成员，没有返回值。
      * Set转化为数组  ``Array.from``方法可以将 Set 结构转为数组。
      ```
      const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
      ```
    
    * 遍历方法：
``keys()``：返回键名的遍历器
``values()``：返回键值的遍历器
``entries()``：返回键值对的遍历器
``forEach()``：使用回调函数遍历每个成员

* Set 结构的键名就是键值

## WeakSet
WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

* **WeakSet 的成员只能是对象**，而不能是其他类型的值

* WeakSet 中的对象都是弱引用,如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。**WeakSet 不可遍历。**

## Map
它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

## Set实例属性和方法
* ``size 属性``
返回Map结构的成员总数

```js
const map=new Map();
map.set({name:"xiaoming"},"student");
map.set({name:"xiaoming"},"student");
console.log(map.size);  //2
```
* ``set(key,value)``
set方法设置键名key对应的键值为value，然后返回整个 Map 结构。(重复设置键值可被更新)

```js
let map = new Map()
  .set(1,'a')
  .set(2,'b')
  .set(3,'c');
  console.log(map);  //Map(3) {1 => "a", 2 => "b", 3 => "c"}
```
set方法返回当前的Map对象，所以可以采用链式写法。

* ``get(key)``

```js
const map=new Map();
map.set({name:"xiaoming"},"student");
map.set({name:"xiaoming"},"teacher");
console.log(map.size);  //2
console.log(map.get({name:"xiaoming"}));  //undefined
```
get方法读取key对应的键值，如果找不到key，返回undefined。

* ``delete(key)``
delete方法删除某个键，返回true。如果删除失败，返回false。

* ``clear()``
clear方法清除所有成员，没有返回值。

### Map的遍历方法
``keys()`` 返回键名的遍历器。
```js
const map = new Map([
	[{name:"xiaoming"},{name:"xiaozhang"}],
	[{name:"xiaohong"},{name:"xiaoxiao"}]
]);

console.log(typeof(map.keys()),map.keys());  

object   MapIterator {{…}, {…}} 
//里面有原型上的属性,其实例上   [[Entries]]:Array(2)  的属性是一个数组
```
``values()`` 返回键值的遍历器。
``entries()`` 返回所有成员的遍历器。
``forEach()`` 遍历 Map 的所有成员。
```js
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```
Map 结构转为数组结构，比较快速的方法是使用扩展运算符（...）
```js
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

console.log([...map.keys()]);  //["F", "T"]
```
## WeakMap
* WeakMap ``只接受对象作为键名（null除外），不接受其他类型的值作为键名``。(注：接收字符串都会报错)

```js
const map = new WeakMap();
map.set(1, 2)
// TypeError: 1 is not an object!
map.set("1", 2)
// TypeError: 1 is not an object!
map.set(Symbol(), 2)
// TypeError: Invalid value used as weak map key
map.set(null, 2)
// TypeError: Invalid value used as weak map key
```

* **WeakMap 的键名所引用的对象是弱引用。**
谈一谈弱引用

        在计算机程序设计中，弱引用与强引用相对，是指不能确保其引用的对象不会被垃圾回收器回收的引用。 一个对象若只被弱引用所引用，则被认为是不可访问（或弱可访问）的，并因此可能在任何时刻被回收。

上面的官方解释是不是蛮绕的~~~

我们再举例说明一下：

在js中我们创建一个对象，都是在建立一个强引用
```js
var obj = new Object();
```

只有当我们手动设置 ``obj=null`` 时，垃圾回收机制才可能回收obj所引用的对象。

而如果我们能创建一个弱引用的对象：
```js
//eg: 用 WeakMap 创建一个弱引用
var obj = new WeakObject();
```
我们什么都不用做，只用静静的等待垃圾回收机制执行，obj 所引用的对象就会被回收。

```js
//栗子 1
    const map = new Map();
	let key = {"name":"xiaoming"};
	
	//创建了map对key的所引用对象的强引用
	map.set(key, 1);
	console.log(map);  //Map(1) {{…} => 1}
	//key=null不会导致key的原引用对象被回收
	key = null;
```
此时在控制台输入 console.log(map)  打印输出 Map(1) {{…} => 1}
上面的栗子可以这样解释 key与对象有一个指向关系，map与对象也存在指向关系，若设置 key=null，因为强引用，此时key的指向切断了，但是map仍存在指向关系。

如果你想要让 Obj 被回收掉，你需要先 delete(key) 然后再 key = null
```js
    const map = new Map();
	let key = {"name":"xiaoming"};
	map.set(key, 1);
	map.delete(key);
	key = null;
```
此时在控制台打印输出为 Map(0) {}

```js
    const wm = new WeakMap();
	let key = {"name":"xiaoming"};
	wm.set(key, 1);
	//wm.delete(key);
	console.log(wm);  //WeakMap {{…} => 1}
	key = null;
```
此时在控制台上打印输出 WeakMap {} 或者 WeakMap {{…} => 1} 结果是多少这就得看垃圾回收机制了 啊哈哈~~~

总结这个弱引用的特性，就是 WeakMaps 保持了对键名所引用的对象的弱引用，即垃圾回收机制不将该引用考虑在内。只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。

