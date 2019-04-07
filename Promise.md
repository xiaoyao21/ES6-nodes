## ES6 Promise
### 什么是Promise？
从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。
通俗的解释，Promise就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

### 使用Promise有什么用？
有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。
几个栗子吧：如果我们现在实现将大象放入冰箱的操作（此处需求操作必须是异步的），首先需要执行一个操作 
1. 打开冰箱门，成功打开冰箱门后再 2. 将大象放入冰箱，大象完全进入冰箱后才能 3. 将冰箱门关闭。

对于异步操作照我们之前的写法这3个方法是嵌套实现的。。那如果有很多操作呢，这样嵌套写下来你一定会崩溃的！！！！

### 使用Promise的缺点
就拿ajax的请求来说，后一个请求需要依赖于前一个请求成功后，会导致多个ajax请求嵌套，代码不够直观。


### Promise 对象
Promise对象（代表一个异步操作，异步操作才可改变其状态）有三种状态：
pending（进行中）、fulfilled（已成功）和rejected（已失败）

Promise对象的状态改变，只有两种可能：从pending-->fulfilled 和从 pending-->rejected。

Prommise 的缺点：无法取消Promise，一旦新建它就会立即执行，无法中途取消。

## 创建 未完成的Promise

Promise对象是一个构造函数，用来生成Promise实例。

构造函数接受一个参数：包含初始化Promise代码的 ``执行器函数``。

执行器接受两个参数，resolve()函数和reject()函数。执行器成功完成时调用resolve()函数，失败时调用reject()函数。

Promise一大特点：``一旦创建，Promise的执行器会立即执行``
```js
//创建一个Promise实例
const promise = new Promise(function(resolve, reject) { 
    //该function函数也叫执行器函数
    // ... some code

  if (/* 异步操作成功 */){  
    resolve(value); //pending --> resolved 成功
  } else {
    reject(error); //pending --> rejected 失败
  }
});
```
### Promise.then
Promise实例生成以后，可以用then方法 ``分别指定resolved状态和rejected状态的回调函数``。这两个参数都是可选的
```js
promise.then(function(contents) {  
  // success
}, function(contents) {
  // failure
});

promise.then(function(contents) {  
  // success
});

promise.then(null,function(contents) {  
  // failure
});
```

``Promise.catch``
Promise还有一个catch方法，相当于只给其传入拒绝处理程序的then()方法。
```js
promise.then(null,function(err){
	//拒绝
});
promise.catch(function(err){
	//拒绝
});
```
一般情况下我们将 ``then()方法和catch()方法一起使用来处理异步操作的结果`` 可以更清楚的指明操作结果是成功还是失败的。
## 
```js
let promise=new Promise(function(resolve,reject){
	console.log("Promise")
	
	resolve();
})

promise.then(function(){
	console.log("then");
})

console.log("Hi!");

//Promise
//Hi!
//then
```
调用resolve()后会出发一个异步操作，传入then()和catch()方法的函数会被添加到任务队列中并异步执行。

完成处理程序 resolve() 和拒绝处理程序 reject()

总是在执行器完成后被添加到任务队列的末尾。（与执行器不同，then 方法没有被立即执行）

```js
//栗子2
const p1 = new Promise(function (resolve, reject) {
  // ...
});

const p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})
```
（栗子2）上面代码中，p1和p2都是 Promise 的实例，但是p2的resolve方法将p1作为参数，即一个异步操作的结果是返回另一个异步操作。

注意：这时p1的状态就会传递给p2，也就是说，p1的状态决定了p2的状态。如果p1的状态是pending，那么p2的回调函数就会等待p1的状态改变；如果p1的状态已经是resolved或者rejected，那么p2的回调函数将会立刻执行。  ?????

## 创建已处理的Promise
 ``Promise.resolve()``
只接受一个参数并返回一个完成态的Promise 该promise的状态不会被改变，所以其拒绝处理程序永远不会被调用。
```js
let promise=Promise.resolve(42);

promise.then(null,function(){
	console.log("123");  //不打印
})
```

``promise.reject()``
同理可以通过promise.reject()创建已拒绝的promise

## 相应多个Promise
``Promise.all()方法``
promise.all方法只接收一个参数并返回一个Promise，参数为多个should监视Promise的可迭代对象（eg:数组）

只有可迭代对象中所有的Promise都被解决后返回的Promise才会被解决。

只要有一个被拒绝，返回的Promise没等所有的可迭代对象都执行完就立即被拒绝。

```
let p1=new Promise(function(resolve,reject){
	resolve(1);
})

let p2=new Promise(function(resolve,reject){
	resolve(2);
})

let p3=new Promise(function(resolve,reject){
	resolve(3);
})
let p4=Promise.all([p1,p2,p3]);
p4.then(function(value){
	console.log(value);  //[1, 2, 3]
})
```
上面的栗子：传入p4完成处理程序的结果是一个包含每个解决值的数组，这些值按照Promise被解决的顺序存储

``Promise.race()方法``
不同于Promise.all()方法， Promise.race()只要有一个Promise被解决返回的Promise就被解决。

传入Promise.race()方法的Promise会进行竞选，如果先解决的是已完成的Promise，则返回已完成的Promise，如果先解决的是已拒绝的Promise，则返回已拒绝的Promise。