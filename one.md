### es6

兼容性
ES6(ES2015)——IE10+，Chrome，FireFox，移动端，NodeJS

编译，转换
1.在线转换
2.提前编译

```js
bable == brower.js
//在 IE10 以下也可以显示（在线编译）
<script src="browser.js" charset="utf-8"></script>

<script type="text/bable">
let a=12;
alert(a);
</script>   
```

***
## 变量
var ``（作用域是函数）``
1.可以为一个变量重复声明
```js
var a=10;
var a=0;
consolr.log(a);   //不会报错 打印出 0
```
2.无法限制是否修改
3.没有块级作用域 
{}  语法块可以独立存在  这就是一个块

> let  不能重复声明，变量—可以修改，块级作用域
const  不能重复声明，变量—不可以修改，块级作用域

* 块级作用域可以有效地 **解决闭包**
```js
<body>
	<input type="button" value="按钮1">
	<input type="button" value="按钮2">
	<input type="button" value="按钮2">

<script type="text/javascript">  //解决此处闭包 方法1 立即执行函数
    var input=document.getElementsByTagName('input');
   
   

    for(var i=0;i<input.length;i++)
    {
    	(function(i){
    		input[i].onclick=function(){
    		alert(i);
    	    }; 	
    	 (i)};
    };
    
</script>
</body>
//方法2  用let声明构成块级作用域
for(let i=0;i<input.length;i++)
    {	
    		input[i].onclick=function(){
    		alert(i);
    	};  	
    };

```
***
## 函数
箭头函数 ``()=>{}``
1.如果只有一个参数（）可以省 (注意：此处必须有且只有一个参数)
2.如果只有一个return {}连带return可以省

***
## 函数参数
1.参数扩展/展开
2.默认参数

参数扩展：
1.收集剩余的参数
functrion show(a,b,...args){}   *Rest Parameter必须是最后一个


```js
function show(a,b,...args){   //最后一位为剩余参数 参数名任意
    alert(a);  //12
    alert(b);  //15
    alert(...args);  //8
    alert(args);  //8,9,20,21,90
}
show(12,15,8,9,20,21,90);
```
2.展开数组
展开后的效果就和直接把数组写在那一样
```js
let arr=[1,2,3];

show(...arr);  //此处...arr相当于参数1,2,3
function show(a,b,c){
	alert(a);  //1
	alert(b);  //2
	alert(c);  //3
}

a=...arr;
alert(a);  //报错!!!不能这样写
```
默认参数
可传可不传，你传就听你的
```js
function show(a,b=54,c=12){
			console.log(a,b,c); 
		}
	show(99);  //99,54,12
	show(99,98,97);  //99,98,97
```
***
## 解构赋值
(左边是什么东西，右边就是什么东西)

1. 左右两边结构必须一样 （类型一致）
2. 右边必须是个东西 （合法的东西）
3. 声明和赋值不能分开 （声明和赋值同时进行）
```js
let[a,b,c]=[12,3,4];
let{a,b,c}={a:12,b:3,c:4}; 
console.log(a,b,c);  //12,3,4

let arr=[12,3,4];
let json={a:12,b:3,c:4};
console.log(arr,json);  //[12, 3, 4] {a: 12, b: 3, c: 4}

let[{a,b},[n1,n2,n3],num,str]=[{a:12,b:5},[12,5,7],8,"hello"];
console.log(a,b,n1,n2,n3,num,str)  //12 5 12 5 7 8 "hello"

let {a,b}={12,3};
console.log(a,b) //报错！！右边不是一个合法的类型
```
***
## 数组
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

2. 字符串模板
```js
let a=12;
let str=`a${a}bc`;
alert(str);  //a12bc

//${a}  //将a变量加入字符串中

//1.直接可以把东西加入字符串里面  ${东西}
//2.可以折行
eg：let str=`a${a}
		bc
		11`;
    alert(str);  //a12
                 //bc
                 //11
```
***
## ES6的面向对象
1. 多了class关键字,有了专门的构造器，构造器和类分开了
2. class里面直接加方法，不用再用原型加方法了
```js
function User(name,pass)  //在原来的js中类和构造函数样子一样 原来的写法
	{
		this.name=name;
		this.pass=pass;
	}
	User.prototype.showName=function(){
		alert(this.name);
	}
	User.prototype.showPass=function(){
		alert(this.pass);
	}
	
	var ul=new User("xiaoming","123");
	ul.showName();
	ul.showPass();
	
	
	class User{
		constructor(name,pass){  //构造器,构造函数
			this.name=name;
			this.pass=pass;
			}
		showName(){
			alert(this.name);
		}
		showPass(){
			alert(this.pass);
		}
	}
	var ul=new User('blue','123456');
	ul.showName();
	ul.showPass();
```
继承：
```js
//普通的继承
	function User(name,pass)  //在原来的js中类和构造函数样子一样
	{
		this.name=name;
		this.pass=pass;
	}
	User.prototype.showName=function(){
		alert(this.name);
	}
	User.prototype.showPass=function(){
		alert(this.pass);
	}
	
	function VipUser(name,pass,level)
	{
		User.call(this,name,pass);  //this对象调用User对象的方法
		
		this.level=level;
	}
	//继承方法
	VipUser.prototype=new User();  //？？
	VipUser.prototype.constructor=VipUser;  //？？
	
	VipUser.prototype.showLevel=function(){
		alert(this.level);
	}
	
		var vl=new VipUser("xiaoming","123","3");
		vl.showName();
		vl.showPass();
		vl.showLevel();
		
//es6中的继承--------------------------------------------------------------------------------
class User{
		constructor(name,pass){  //构造器,构造函数 相当于对象自己的初始化
			this.name=name;
			this.pass=pass;
			}
		showName(){
			alert(this.name);
		}
		showPass(){
			alert(this.pass);
		}
	}
   //继承方法
	class VipUser extends User{   //VipUser  User
		constructor(name,pass,level){
			super(name,pass);//super ——超类 类似于执行父类的构造函数
			
			this.level=level;
		}
		
		showLevel(){    //新增的方法
			alert(this.level);
		}
	}
	
	var vl=new VipUser("xiaoming","123",3);
	vl.showName();
	vl.showPass();
	vl.showLevel();
```

***
## json
1.JSON对象
    JSON.stringify
    JSON.parse
2.简写
    名字跟值（key和value）一样的     留一个就行
    方法     ：function一块删掉
```js
    let a=12;  //名字和变量一样时可以只写一位
	let b=5;
	let json={a,b,c:5};
	console.log(json);  //{a: 12, b: 5, c: 5}
	
	let json={
		a:12,
		show(){   //方法不用再加函数  show：function(){};以前的写法
			alert(this.a);
		}
	};
	json.show();
```
json的标准写法
1.只能用双引号          egt： '{"a":12,"b":5}'
2.所有的名字都必须用引号包起来
***
## Promise 
Promise ——承诺

异步：操作之间没啥事，同时进行多个操作
同步：同时只能做一件事

异步：代码更复杂
同步：代码简单

Promise ——消除异步操作
* 用同步一样的方法，来书写异步代码 
eg：之前在ajax代码中要加入代码实现代码的异步操作，就必须使用嵌套。而怎样使得具有异步功效的代码可以像同步代码一样书写一条一条并列的代码，方便用户理解。
```js
let p1=new Promise(function(resolve,reject){  //resolve——成功了   reject——失败了
		ajax({
			url:"",
			dataType:"",
			success(arr){
				resolve(arr);  //返回成功过的结果
			},
			error(err){
				reject(err);   //返回失败的结果
			}
		})
	})
	//Promise执行有结果了就调用then
	p1.then(function(){  //可以加参数
		alert('成功了')
	},function(){
		alert('失败了')
	});  
	
	//两个极其以上---------------------------------------------------------------------
	let p2=new Promise(function(resolve,reject){  //resolve——成功了   reject——失败了
		ajax({
			url:"",
			dataType:"",
			success(arr){
				resolve(arr);  //返回成功过的结果
			},
			error(err){
				reject(err);   //返回失败的结果
			}
		})
	})
	
	Promise.all([
		p1,p2 //只有当两个都成功了才返回会面对成功函数
	]).then(function(){
		alert('全部成功了')
	},function(){
		alert('至少有一个失败了')
	})
	
	//简化封装---------------------------------------------------------------------------
	function createPromise(url){
		return new Promise(function(resolve,reject){  //resolve——成功了   reject——失败了
			ajax({
				url:url,  //url
				dataType:"",
				success(arr){
					resolve(arr);  //返回成功过的结果
				},
				error(err){
					reject(err);   //返回失败的结果
				}
			})
		})
		//Promise执行有结果了就调用then
		p1.then(function(){  //可以加参数
			alert('成功了')
		},function(){
			alert('失败了')
		});  
	}
	
	Promise.all([
		createPromise(".."),
		createPromise("..")
	]).then(function(){
		alert('全部成功了')
	},function(){
		alert('至少有一个失败了')
	})
```
***



