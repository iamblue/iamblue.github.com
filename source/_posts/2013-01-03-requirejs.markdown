---
layout: post
title: "RequireJS"
date: 2013-01-03 00:14
comments: true
categories: 
---

RequireJS

在進入RequireJS之前，我想先帶各位了解requireJS的優點，以及為何我們要用requireJS?

1.可以"快速"且"清楚"了解這個function引用了哪些外部呼叫js

首先，假如我們有很多很多的function在同一頁中，且這些function都彼此間互相引用或者是大量引用外部呼叫的js文件時，這對於後續維護非常麻煩（特別是每次修function要去打開一堆相關的外部呼叫文件找），因此requireJS的function架構上，可以讓我們快速了解引用了哪些外部文件，例如：

	require(["jquery.alpha","jquery.beta"], function(aaa,bbb) {
		//your code
	});

有此式我們可以得知這個function引用了jquery.alpha和jquery.beta兩個外部js檔.


2.可以讓你直覺且客觀的減少定義到全局變量
這部份就是指javascript很基本的全局和區域觀念，不多說.

3.實作：

[html端]
head引用：

    <script data-main="scripts/main" src="scripts/require-jquery.js"></script>

請注意此main是指main.js，他的路徑必須在 scripts/ 之下，而其他爾後要引用的js檔也盡量都放在 scripts/ 之下，這樣之後若引用出某文件時，可以不用再調整絕對路徑.

body引用：

    require(["jquery.alpha","jquery.beta"], function(a,b) {
         b.addit();
	     a.alpha(h1);
	});

請注意，引用進來的一個外部js必須要給他一個namespase（比如說jquery.alpha的namespace就是a）而若今天想引用外部文件(jquery.alpha)的某一function叫做addit()，則寫a.addit();

[js端]
js端可以define也可以reqiure文件，一般而言外部js內常寫define，如下（請注意看我註解的部份）：

	define(function () {
	   function tools(str) {
	      console.log(str);
	    };

	  return {
	    alpha: function (str) {
	      console.log(str);
	      tools(str)  //這時候call的tools function是有作用的
	      yoyo(str)   //請注意！這時候call的yoyo是沒有作用的
	    }
	    yoyo: function (str){
	    	console.log(str);
	    }
	  }
	});

也就是說，在return以上的function是可以在同一個return內部同一function裡相互間call,但是寫在其他地方的function則無法call，基於這個原因，我的建議是，這部份盡量採取把"執行"和"call"分開架構格式，像是：
	
	define(function () {
	   function tools(str) {
	      console.log(str);
	    };
	    //以上寫執行內容

	  return {
	    tools: function (str) {
	      tools(str);
	    }
	    //以上負責call function
	  }
	});

首先define到return之間，把它想像成專放負責"執行"的程式，而return以下的就是專門放"call" functions，這樣可以很清楚的分類架構.


補充幾點：

若你在html端上有設定onclick事件（DOM 0級的），像是：

	<button id="thisbtn" onclick="hihi();">yoyo</button>

請注意這時候的hihi  function一定要在require外面，他才會被導向此function，像是:
  	
  	function yoyhihi(){
            alert('yoyo');
    };
    require(["jquery.alpha","jquery.beta"], function(a,b) {
        //不能放這裡面，他會抓不到       
	});

這樣的話代碼變成很難控管，所以盡量不要用DOM 0級方法寫，用addEventListner去做監聽event方法較佳！


4.結論

根據上述初步學習requireJS的調用架構，我們可以有一些心得，RequireJS可以讓你快速看出程式來源.方便修改，不過相對的代價就是，在建立function時會比直接寫還要花較多的時間，因此：

	1.如果你的專案前端的js function非常的少.很輕量那麼我建議不要用RequireJS，這會大大降低開發速度

	2.如果你的專案前端js系統非常複雜，日後維護已經達到找文件都非常耗時時，就用它吧！

至於專案的大小區別，最好的方式就是一開始盡量評估準確！











