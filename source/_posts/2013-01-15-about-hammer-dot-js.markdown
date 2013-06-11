---
layout: post
title: "About Hammer.js"
date: 2013-01-15 10:27
comments: true
categories: 
---


淺談Hammer.js

首先，先提一下為何用Hammer.js的原因：

	1.他把在Html5裡的gesture和touch事件與原來傳統mouse事件做結合
	2.有針對各手指去做追蹤
	3.多功能的event事件

最後，結合以上....只需要2KB，實在沒有不用它的道理!

關於第一點，我們來看看他的lib (hammer.js)

在line 556開始，我們可以看到它把mouse event 與其他事件綁在一起

	//line 561.562
	case 'mousedown':
	case 'touchstart':

	//line 585.586
	case 'mousemove':
	case 'touchmove':

	//line 609~612
	case 'mouseup':
	case 'mouseout':
	case 'touchcancel':
	case 'touchend':

簡單來講，這樣的好處就是，今天只要寫一行 event 就可以把用滑鼠的user與用觸控板的user發生的行為考慮進去

第二點，對於個別手指的追

	//line 561.562
	case 'mousedown':
	case 'touchstart':

	//line 585.586
	case 'mousemove':
	case 'touchmove':

這兩行在處理的第一件事情，就是去做計算各手指，如下：

	count = countFingers(event);

	//來追蹤一下countFingers在做什麼事情

    function countFingers( event )
    {
        return event.touches ? event.touches.length : 1;
    }

但是請注意它的註解
    // there is a bug on android (until v4?) that touches is always 1,
    // so no multitouch is supported, e.g. no, zoom and rotation...

這點我在於android 2.7的確是array為1 ，因此我建議，每次在取個手指的行為時，盡量用迴圈去偵測它（從0開始）

接下來，每個事件可以取用的行為資料也不一樣，舉例來說：
	
	//line 561開始
	case 'mousedown':
	case 'touchstart':

	triggerEvent("dragend", {
		originalEvent   : event,
		direction       : _direction,
		distance        : _distance,
		angle           : _angle
	});
	//可取的資料就是direction , distance , angle

其他同理可看出，整理如下

	1.mousedown , touchstart部分：
		direction : 上下左右  其他方向不行
		distance : 距離 （單位象素.有浮點數）
		angle ：角度

	2.mousemove , touchmove':
		無

	3.mouseup , mouseout , touchcancel , touchend :
		這邊判斷式稍多，主要是先偵測他之前的行為是什麼？(drag? or transform?)
		之後呈現：
			position ： X.Y 座標

至於，在View端的簡單範例寫法如下，：

	(function ($) {
	    var $sw = $('#swipeme'),
	        $output = $('#output');

	    $sw.on('hold tap swipe doubletap transformstart transform transformend dragstart drag dragend swipe release', function (event) {
	        event.preventDefault();

	        $output.prepend("Type: " + event.type + ", Fingers: " + event.touches.length + ", Direction: " + event.direction + ", distance: "+ event.distance +"<br/>");
	    });
	    //上述你可以加進想要呈現的data
	    // this is how you unbind an event
	    /*$sw.on('swipe', function (event) {
	        event.preventDefault();

	        $sw.off('tap');
	    });*/
	}(jQuery));

有人會問，為何要寫"event.preventDefault();"？

因為我們在使用觸控螢幕時，有些手勢是該平板預設的放大縮小行為，可是這個行為在瀏覽器上我們不打算這樣做，用這個語法可以防止這種現象。

