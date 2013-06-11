---
layout: post
title: "一些關於javascript OO (Object Oriented) issue"
date: 2013-01-09 13:54
comments: true
categories: 
---

不用this (oo)的寫法：

	var yoyo ={

		a:function(){
			//blabla...
		}
		b:function(){
			//blabla...
		}

	}
	yoyo.a();

用this (oo)的寫法：

	var hihi = function (){
		this.a = function(){
			console.log('a');
		}
		this.b = function (){
			console.log('b');
		}
	}
	var hihi2 = new hihi;  //注意  這邊寫 new hihi(); 也可以
	hihi2.a();   // a


prototype 繼承演示

	var hihi = function (){
		this.a = function(){
			//blabla
		}
		this.b = function (){
			//blabla
		}
	}

今天若想繼承上列，再加個hihi內個下列類別：

	var yoyo = function(){
		this.c = function (){
			console.log('c');
		}
	}

要先
	
	yoyo.prototype = new hihi;
	var show = new yoyo; 

	show.a(); // a  
	show.c(); // c

網路上看過一些繼承.洗掉問題

	var ccc = function (){};
	ccc.prototype.yoyo = function(){
		console.log('123')
	}
	ccc.prototype.titi = function(){
		console.log('456');
	}
	//ccc.prototype={};//因為這個已經把東西洗掉，所以其他人沒得繼承
	var eee = new ccc();
	var fff = new ccc();
	eee.yoyo = function(){
		console.log('777');
	}
	ccc.prototype={};  //因為先前已經繼承過了，所以東西還在
	eee.yoyo();
	fff.yoyo();


再來，想來聊聊效能問題，網路上有人寫了這篇測試文

> http://jsperf.com/prototype-operator-performance

他比較了三種方法：

	A: 正是我上述提到的this(OO)寫法
	B: 與我上述提到的繼承部分，額外加類別進prototype方法
	C: 一般的prototype寫法

有興趣可以看這作者blog的比較文

> http://fred-zone.blogspot.tw/2012/04/javascript-class.html

直接提結論：

	單純比較 呼叫方法(Call Method) ，不使用 Prototype 略勝一疇
	比較建立實例(Instance) 的速度，使用 Prototype 快過其他實作近 20 倍，且B方法速度也快過其他方式

