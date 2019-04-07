# 数组

数组里面的四个常用 ``方法``
```js

map     映射          一个对一个 （可以将数据以某种特定的方法处理）
        let arr=[12,5,8];
        let result=arr.map(function(item){
        	return item*2;
        })
        alert(result);  //24,10,16
    
        let score=[0,60,50,90];
    	let result=score.map(item=>item>=60?'及格':'不及格');
    	alert(result);  //不及格,及格,不及格,及格

  
reduce  汇总   一堆出来一个
        算个总数   //最总结果  每个数 下标
        let arr=[12,69,180,8763];
    	let result=arr.reduce(function(tmp,item,index){
    		return tmp+item;
    	})
    	alert(result);   
      
    
filter  过滤器  通过true和false保留一部分，具有数据的筛选功能
        let arr=[12,34,5,10,18];
    	let result=arr.filter(function(item){
    		if(item%5==0)
    		return true;
    		else 
    		return false;
    		
    		//return item%5;
    	})
	
    	//简写  ==本身返回的就是布尔值
        let result=arr.filter(item=>item%5==0)
    	alert(result); //筛选出能被 5整除的
	
	
forEach 循环（迭代） //和平常的for循环一样，将数组走一遍
        arr.forEach((item,index)=>{
    		alert(index+':'+item); //index记录数的序列，可加可
    	})
```
***
## 字符串
1. 多了两个新方法
    startsWith  以XX开始
    endsWith     以XX结束
```js
let str="http://192.168.37.1..";
	if(str.startsWith('http://'))
	{
		alert("这是一个普通网址！"); //判断该字符串中是否以http://开头
	}
```



## 扩展运算符
扩展运算符就是三个点（...）**将一个``数组``转为用逗号分隔的``参数序列``**
```
function add(x, y) {
  return x + y;
}

const numbers = [4, 38,21];
add(...numbers) // 42  //根据函数的参数只传入两个参数进去

```
* 不同于不定参数 扩展运算符在函数参数的位置中未作规定

* 注意，扩展运算符如果放在括号中，JavaScript 引擎就会认为这是函数调用。如果这时不是函数调用，就会报错。
```
console.log((...[1, 2]))
// Uncaught SyntaxError: Unexpected number

console.log(...[1, 2])  //函数调用
// 1 2
```

## 扩展运算符的应用
（1）复制数组
``concat() 方法``
concat() 方法用于连接两个或多个数组。
该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。
```
const a1 = [1, 2];
const a2 = a1;  //此处相当于只是复制了至数组的指向

a2[0] = 2;
a1 // [2, 2]
```
```
const a1 = [1, 2];
const a2 = a1.concat();

a2[0] = 2;
a1 // [1, 2]
```
上述代码修改a2或a1都不会另一个产生影响

（2）引用值与初始值的理解  （合并数组）
```
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

arr4=arr1.concat(arr2, arr3)
arr4.push(23);
// ES5 的合并数组
console.log(arr4);  //["a", "b", "c", "d", "e"]
console.log(arr1);  //["a", "b"]
```

```
let arr1 = ['a', 'b'];
let arr2 = ['c'];
let arr3 = ['d', 'e'];

arr4=arr1.concat(arr2, arr3)

// ES5 的合并数组
console.log(arr4);  //["a", "b", "c", "d", "e"]
arr2[0]='0';
console.log(arr2)   //["0"]
console.log(arr4);  //["a", "b", "c", "d", "e"]
```
```
let arr1 = [{a:1}];
let arr2 = [{a:2}];
let arr3 = [{a:3}];

arr4=arr1.concat(arr2, arr3)
alert(JSON.stringify(arr4));  //[{"a":1},{"a":2},{"a":3}]

arr2[0].a=0;
alert(JSON.stringify(arr2))   //[{"a":0}]
alert(JSON.stringify(arr4));  //[{"a":1},{"a":0},{"a":3}]
```
```
let arr1 = [{a:1}];
let arr2 = [{a:2}];
let arr3 = [{a:3}];

arr4=arr1.concat(arr2, arr3)

console.log(arr4);  //[{a:1},{a:0},{a:3}]
arr2[0].a=0;
console.log(arr2)   //[{a: 0}]
console.log(arr4);  //[{a:1},{a:0},{a:3}]
```
按理说上面这个栗子第一次打印处理啊的结果应该为 [{a:1},{a:2},{a:3}]啊~~

我们尝试将下面的内容加到定时器内 打印出来瞬间点开对象发现就是我们想要的结果~~
```
console.log(arr4);  //[{a:1},{a:2},{a:3}]

setTimeout(()=>{
	arr2[0].a=0;
	console.log(arr2)   //[{a: 0}]
	console.log(arr4);  //[{a:1},{a:0},{a:3}]
},10000)
```
总的来说我们在控制台上看到的结果（对象）点开后请求的是最新的对象信息，所以在未赋值后点开 值会更新。

