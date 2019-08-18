* async和await，比起星号和yield，语义更清楚了。
* ``async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。``
## 引入async & await
```js
const readFile = function (fileName) {   //异步操作
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};


const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');   //等待该异步执行完毕
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
* async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。
* ``saync的返回值是Promise``,可以用then方法直送下一步的操作


## 基本用法

* async 函数返回的promise对象
    * async函数内部 ``return语句返回的值，会成为then方法回调函数的参数。``
```js
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```
* async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。
    * ``只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。``
* await命令
    *  正常情况下，await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。
```js
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```
* await返回错误
    * 任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。
    * 有时，我们希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行。
    * 或者在await后面再跟一个catch方法
```js
//----------------------函数中断-----------------------
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}
//-------------------try-catch捕获错误-----------------
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world
//--------------promise.catch 捕获错误----------------
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// 出错了
// hello world
```

* 注意了！！ await命令只能用在async函数之中