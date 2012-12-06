---
layout: post
title: "Javascript Patterns"
date: 2012-11-12 21:01
comments: true
categories: 
---




About Javascript Patterns , you can see this github:

> http://shichuan.github.com/javascript-patterns/


<h1>Issue1:Function Declarations</h1>

- creating anonymous functions and assigning them to a variable
> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/function-declarations.html

在談談這個主題之前，我想要先了解js架構：

	function a(){
		alert('123');
	}
	console.log(a);

這時候如果把console.log(a)這行搬到第一行時，也就是如下：

	console.log(a);
	function a(){
		alert('123');
	}


他仍輸出一樣的結果，原因是js在讀取時會先把所有funciton先讀，然後再讀其他的東西.

不夠清楚？

在舉個例子，如果今天出現如下狀況：

	console.log(a.toString());
	function a(){
		alert('123');
	}
	function a(){
		alert('567');
	}

則出來的結果是：
	
	function a(){
		alert('456');
	}
	[Finished in 0.1s]

也就是說 如果遇到兩個都是相同命名的function時，後面的會覆蓋掉前面的同名function.

OK，直接切入這個主體，先看看這個程式碼：

	function getData() {
	}

這個程式碼本身來說是沒有問題的，但是原因在於js開發者必須要盡量把所有定義的東西用object去定義，這是一個良好的習慣，因為這樣可以幫助開發者去理解和使用他，本主題有討論兩種改善方式：

第一種：

	var getData = function () {
				};

第二種：
	
	var getData = function getData() {
			};

第二種的最大好處是在於，可以讓getData function 可以去做內部call迴圈，比如說：

	var a = function a(){
		console.log('123'+a);
	};
	a();

結果為：

	123function a(){
		console.log('123'+a);
	}

但是，有件奇怪的事情來了，如果我有個代碼如下：

	console.log(a.toString());
	var a = function a(){
		alert('123');
	};

竟然出現錯誤!若改成：

	var a = function a(){
		alert('123');
	};
	console.log(a.toString());

則成功，這樣的例子可以說明，js在讀取時事先把function先讀完，而已經被定義過的object不會先讀取.


<h1>Issue2:Conditionals </h1>
- pattern and antipattern of using if else

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/conditionals.html

先來看第一個程式碼：

	if (type === 'foo' || type === 'bar') {
	}

=== 其實就是"格式符合"且"內容物完全一樣"則就是true 不符合則就是false

console.log(1 === 1) 結果就是true 因為兩邊都是number

console.log("1" === 1) 結果就是false 因為前者是string 後者為number

console.log("1" === "2") 結果就是false，雖然都是string，但是string 的1與2不同

作者建議的寫法一：

	if (/^(foo|bar)$/.test(type)) {
			}

這部份就是regex test，詳情我建議看這網站，很詳細

> http://blog.roodo.com/rocksaying/archives/2670695.html

這個作法其實就是直接轉成machine language，好處是在簡短的句子比對效能很快，但是他的時間複雜度為O(N)，所以在長句子的比較會不見得比第一種或第二種快，可以參考這個demo:

> http://jsbin.com/uzuxi4/4/edit

不過我覺得這個在解讀程式上時真的有點難懂，在寫時還是要考慮一下你的partner看不看的懂

作者建議寫法二：

	if (({foo:1, bar:1})[type]) {
			}

其實這個就是最簡單的hash table search，他的時間複雜度平均為O(1)，在分析超長子句時在理論來說較上述來的好，以上相關的討論可以參考這個，非常的詳細：

> http://stackoverflow.com/questions/3945092/why-the-third-option-is-better-than-regex

接下來在line36～97部分在講關於BST(Binary search tree)，作者範例很清楚就不多說，line103~114部分，則是建議使用array的形式取代掉多種if else的假設，我認為這個方式對於工程師解讀上是很有幫助的。

另外除了array方式，我也推薦另外一種作法：

	var hi = {
		a:function () {
			console.log("123")
		},
		b:function(){
			console.log("456")
		}
	}
	hi.a()

印出的結果為：
	
	123

這樣的形式也可以幫助工程師能夠理解，而line120~138部分則是盡量使用logical operators可以幫助我們解讀code.

我在參加js討論會，講者tonyQ提過這個有趣的example：

	function test(option){
		option = option || {};
		console.log(option.start)
	}
	test(null);

這個印出來的結果是：
	
	undefined

原因是null不屬於obj，裡面也沒有start參數，當然undefined，若改成：

	function test(option){
		//option = option || {};
		option.test = option.test || {};
		console.log(option.test.start)
	}
	test({test:{start:1}});

則印出的結果是1，這個方法可以幫助我們快速去理解obj內部的解析，值得學習！另外有個議題還蠻有趣的，請看：
	
	console.log(true || false);
	console.log(true && false);

以上的結果竟是....

	true
	false

不難理解，請自行想想 ...



<h1>Issue3:Access to the Global Object </h1>
- access the global object without hard-coding the identifier window

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/access-to-global-object.html

這邊是指如何去高效率的調用global object，並且讓他們可以通過ES3, ES5 and ES5-strict.

	var global = (function () {
		return this || (1, eval)('this');
	}());

效能展示可以看這個：

> http://jsperf.com/globalx

我們來看這個例子：
	
	var x = 'outer';
	(function() {
	  var x = 'inner';
	  eval('console.log("direct call: " + x)'); 
	  (1,eval)('console.log("indirect call: " + x)'); 
	})();

輸出的結果為：

	direct call: inner
	indirect call: outer

我曾經想過如果把(1,eval)的1改成null or 0 or undefined 結果通通都是這個，至於為何以下的討論串就有行這樣說：

> http://stackoverflow.com/questions/9107240/1-evalthis-vs-evalthis-in-javascript

"The expression (1, eval) is just a fancy way to force the eval to be indirect and return the global object."




<h1>Issue4:Single var Pattern </h1>
- use one var statement and declare multiple variables

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/single-var-pattern.html

一般而言，我們通常會這樣定義變數：

	function func() {
		var a = 1,
		var	b = 2,

		// function body...
	}
這是沒有問題的，但是對於多寫一個var實在沒有額外好處，因此建議這樣寫：

	function func() {
		var a = 1,
			b = 2,

		// function body...
	}

這樣的好處可以幫助自己去強迫把宣告變數的地方放在同一個，可以幫助理解，也可以幫助自己減少忘記宣告變數的動作，
那如果今天宣告的變數裡面有長字串（或者是包html的話），因為包這些字串每次讀取會增加很多空間，因此也建議類似這種東西要先去做宣告變數，如下：

	function updateElement() {
		var el = document.getElementById("result"),
		style = el.style;
		// do something with el and style...
	}


<h1>Issue 5:Hoisting</h1> 
- var statements anywhere in a function act as if the variables were declared at the top of the function

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/hoisting.html



<h1>Issue 6:for loops</h1> 
- optimized for loops

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/hoisting.html

這是一個很基本的javascript 變數宣告的觀念，首先，看如下代碼：

	myname = "global"; // global variable
	function func() {
		console.log(myname); // "undefined"
		var myname = "local";
		console.log(myname); // "local"
	}
	func();

第三行會出現undefined，這是因為在function外宣告的global variable並不會被導進function內變成local variable，因此第三行的console.log因為在他之前沒有宣告local variable，已故呈現undefined

	myname = "global"; // global variable
	function func() {
		var myname; // same as -> var myname = undefined;
		console.log(myname); // "undefined"
		myname = "local";
		console.log(myname); // "local"
	}
	func();

 這個情況是，他先宣告了某個local variable，但是因為他並沒有定義這個變數，以至於第四行的console.log輸出為undefined.

<h1>Issue 7:for-in loops </h1>
- optimized for-in loops

<h1>Issue 8:(Not) Augmenting Built-in Prototypes </h1>
- only augment built-in prototypes when certain conditions are met

<h1>Issue 9:switch Pattern</h1> 
- improve the readability and robustness of your switch statements

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/switch-pattern.html

盡量多使用switch 方式，也請謹守下面switch的格式，break;和default:千萬別忘記打

	var inspect_me = 0,
			result = '';
	switch (inspect_me) {
		case 0:
			result = "zero";
			break;
		case 1:
			result = "one";
			break;
		default:
			result = "unknown";
	}

<h1>Issue 10:Implied Typecasting</h1> 
- avoid implied typecasting

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/avoiding-implied-typecasting.html

其實這邊在討論javascript中，要注意隱式類型的轉換，比如說0在==比較中代表的是false，而1代表的是true，他的範例如下：

	var zero = 0;
	if (zero == false) {
		console.log('123');
	}

輸出的結果為：123，另外我試個有趣的實驗，代碼：

	var zero =[] ;
	if (zero == false){
		console.log('23');
	}
	//Output:23

以上當我設定var zero ={} 或者 '' 或者 []時全部都是一樣的結果，那麼若我試試:

	var zero =['123'] ;
	if (zero == false){
		console.log('23');
	}
	//竟然沒有console.log的值！在試試true
	if (zero == true){
		console.log('23');
	}
	//一樣沒有console.log的值！

同樣試過string.object也是一樣，這個小實驗蠻有趣的!

<h1>Issue 11:Avoiding eval() </h1>
- avoid using eval()

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/avoiding-eval.html

避免使用eval();



<h1>Issue 12:Number Conversions with parseInt() </h1>
- use the second radix parameter

<h1>Issue 13:Minimizing Globals </h1>
- create and access a global variable in a browser environment

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/minimizing-globals.html

盡量減少宣告global variable，否則很容易經過多種方法就讓他被讀出來

	myglobal = "hello"; // antipattern
	console.log(myglobal); // "hello"
	console.log(window.myglobal); // "hello"
	console.log(window["myglobal"]); // "hello"
	console.log(this.myglobal); // "hello"

<h1>Issue 14:The Problem with Globals</h1> 
- various problems with globals

> https://github.com/shichuan/javascript-patterns/blob/master/general-patterns/globals.html

這是補充issue13

	function sum(x, y) {
		// implied global
		result = x + y;
		return result;
	}
	sum(1,2);
	console.log(result);



