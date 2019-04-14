# for of
ES6引入``for..of``循环，作为遍历所有数据结构的统一方法。

一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口，就可以用for...of循环遍历它的成员。也就是说，``for...of循环内部调用的是数据结构的Symbol.iterator方法，遍历的其实是对象的Symbol.iterator属性``。

所以 for...of 循环可以使用的范围包括：

* 数组

* Set

* Map

* 类数组对象，如 arguments 对象、DOM NodeList 对象

* Generator 对象

* 字符串

## 对象
对于普通的对象，for...of结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用。

for...in循环可以便利对象的键名
```js
let obj={
	name:"xiaoming",
	sex:"nan",
	play:"running"
}
for(let i in obj){
	console.log(i)
}
// name
// sex
// play
for(let i of obj){
	console.log(i)  //报错
}
```


解决方法 1：使用Object.keys方法将对象的键名生成一个数组，然后再直接便利这个数组
```js
let obj={
	name:"xiaoming",
	sex:"nan",
	play:"running"
}

//Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组
for(let key of Object.keys(obj)) 
{
	console.log(key+":"+obj[key]);
}
// name:xiaoming
// sex:nan
// play:running
```
## 与其他遍历语句的比较
* ``forEach``  数组提供的内置方法

```js
array.forEach(function(currentValue, index, arr), thisValue)  

//当前元素    当前元素的索引    当前元素所属的数组对象    传递给函数中this的值
```
缺点：无法中途跳出forEach循环，break命令或return命令都不能奏效。

* ``for...in``  循环遍历对象的属性
JavaScript 原有的 ``for...in循环，只能获得对象的键名``，不能直接获取键值。而ES6提供的for...of循环，允许遍历获取对象的键值

```js
var arr = ['a', 'b', 'c', 'd'];

for (let a in arr) {
  console.log(a); // 0 1 2 3
}

for (let a of arr) {
  console.log(a); // a b c d
}
```

for...of循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟for...in循环也不一样。
```js
let arr = [3, 5, 7];
arr.foo = 'hello';

for (let i in arr) {
  console.log(i); // "0", "1", "2", "foo"
}

for (let i of arr) {
  console.log(i); //  "3", "5", "7"
}
```
* for...in循环有几个缺点。

  - 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等等。
  
  - for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
  
  - 某些情况下，for...in循环会以任意顺序遍历键名。