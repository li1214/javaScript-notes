##                                                   99%前端工程师不知道的js

### 1.求下列表达式的结果；
```
	typeof(likeyou);                         
	typeof(typeof(likeyou))                 
	typeof(null);
	null instanceof Object							
	typeof(NaN);							
	typeof([])                          
        isNaN('abc'); 
        isNaN(undefined)
```
### 2.写出console.log()的打印值；
```
	a(102);
	function a (a) {
	    console.log(a)                                                     
	    var a = 101;
	    console.log(a);                                                    
	    var a = function(a) {
	        console.log(a);                                                
	    }();
	    console.log(a);                                                   
	    function a() {};
	    console.log(a);                                                    
	}
```
### 3.写出下面函数的运行结果，如果报错用err表示，如果没有结果，用null表示
```
	function b () {
	    var d = 123;
	    console.log(d)
	};
	b(1);

	function b1 (d) {
	    var d = 123;
	    console.log(d)
	}()

	function b2 (a,b) {
	    console.log(a + b)
	}(1, 2);

	var r3 = function b3 () {
	    var d = 123;
	    console.log(d)
	}();
```
### 4.下面是立即执行函数的是哪些
```
	(function() {})();
	(function() {}());
	! function() {}();
	[function() {}()];
	+function() {}();
	-function(){}();
	* function() {}();
	/ function(){}();                                                                        
	false||function() {}();
        true ||function() {}();
```
### 5.写出f的类型,x的值；
```
	var f = (
	    function f () {
	        return '1';
	    },
	    function g () {
	        return 2
	    }
	)();
	typeof(f);                          

	var x = 1;
	if( function h (){} ){
		x += typeof h;
	}
	console.log(x)              
```

### 6.写出下列构造函数的返回的对象；
```
	function Person (name, sex){
		this.name = name,
		this.sex = sex;
	}
	var person= new Person('小王','男');
	console.log(person);                        


	function Person1 (name.sex){
		this.name = name,
		this.sex = sex;
		return {};
	}
	var person1= new Person1('小王','男');
	console.log(person1);                         


	function Person2 (name.sex) {
		this.name = name,
		this.sex = sex;
		return 1;
	}
	var person2= new Person2('小王','男');
	console.log(person2); 
```
### 7.显出下面console.log()的打印值，如果报错用err表示,没有用null表示
```
	var str = 'abcd';
	console.log(str.length)                        
	str.length = 2;
	console.log(str)                                
	str + - + + + - + - - + 1;
	var test = typeof(str);
	if(test.length == 6){
		test.type = 'string'                         
	}
	console.log(test.type)
```
### 8.创建一个没有原型的obj



### 9.写出下了表达式的结果
```
	 null == undefined;
	 null === undefined;
	 NaN == NaN;
	 NaN === NaN;
	 [] == {}
	 [] == ''
	 ({}) == ''
	 '10' > 9;
	 '10' < '9';
	 [[2]] == 2
	 [2] == 2
	 3.toString()
     3..toString()
     3...toString()

```
### 10.写出console.log()的打印值
```
	var obj={
		"2":'a',
		"3":'b',
		"length":2,
		"push":Array.prototype.push,
		"splice":Array.prototype.splice
	}
	obj.push('c');
	obj.push('d');
	console.log(obj)			
```															
### 11.this指向问题
```
	var name = '222';
	var a = {
		name : '111',
		say : function () {
			console.log(this.name)
		}
	}
	var fun = a.say;
	fun();														
	a.say();													
	var b = {
		name : '333',
		say : function(fun){
			fun();
		}
	}
	b.say(a.say);												
	b.say = a.say;
	b.say();													
```
### 12.要求输入一串低于10位的数字，输出这串数字的中文大写。
    eg: input => 10010010010  ouput => 一百亿一千万零一万零一十

### 13.写一个函数实计算现斐波那契数列第n位
```
//eg f(n) = f(n-1) + f(n-2); f(1) = 1;f(2) = 1;
```
### 14.js中怎么获取下面div伪元素的宽度？。
```
div::affter{
    content:'';
    width:'200px';
    height:'200px';
    display:'block';
    backgroundColor:'#f60';
}
```
### 15.把输入的字符串类型数字从末尾起3位添加一个','分割（代码长度不超过3行）。
```
eg: 100000000 ==> 100,000,000
```

### 16.写出下列表达式的结果
```
var end = Math.pow(2, 53);
var start = end - 100;
var count = 0;
for (var i = start; i <= end; i++) {
    count++;
}
console.log(count);
```
### 17.以下代码输出的是什么。
```
var length = 10;
function fn () {
console.log(this)
    console.log(this.length)
}
var obj = {
    length : 5,
    method : function (fn) {
        fn();
        arguments[0]();
    }
}
obj.method(fn, 1);
```

