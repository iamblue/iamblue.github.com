---
layout: post
title: "detec embedded img "
date: 2012-10-26 21:20
comments: true
categories: 
---

http://design2u.me/blog/606/javascript-10-js-jquery-performance-tuning

onload 問題
http://www.cnitblog.com/CoffeeCat/archive/2008/02/01/39533.html

最好的方法
http://www.planeart.cn/?p=1121

傳統作法
http://itseer.blogspot.tw/2012/04/jquery-imagesloaded.html


change error img


Usually, when we load broken images,there have follow error messages:

	1)404 can not find
	2)503 Undefined variable

404 means it has error image website or have no image.
503 means it has correct image website,but we can't show this image.

Now, there have many tech appling Extended API

現今有很多web技術應用到某個網站的API，進而讀取該網站圖片，而不把圖片存放在自己的database之中。這麼做的確可以降低database的資源使用量，但是容易發生到在load這些外部網站的圖片時會發生一些不知名錯誤，有時候甚至在mobile系統上因為網路速度太慢，直接在讀取的過程中broken。

	<script type="text/javascript">
	jQuery(document).ready(function(){
	var detec_img=jQuery("img");
	for(i=0;i<detec_img.length;i++){
		detec_img[i].onerror=function(){
		this.src="替換的圖片網址"
		}
	}
	});
	</script>

	<script type="text/javascript">
	jQuery(document).ready(function(){
		jQuery('img').attr('onError','this.src="替換圖片的網址"');
	});
	</script>

	另外，如果只是純粹不取代圖片，只是改個樣式的話，可以參考這個
	
	http://stackoverflow.com/questions/4361973/jquery-catching-an-image-load-event-error-404-can-it-be-done