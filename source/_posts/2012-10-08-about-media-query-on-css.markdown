---
layout: post
title: "About media query on CSS"
date: 2012-10-08 23:25
comments: true
categories: 
---
現今mobile系統越來越多，web應用在mobile系統上的技術也越來越多元，因此寫下此篇來專心探討這部份

首先，想必各位都會有個問題，如果是用web寫程式，那麼手機瀏覽器要如何去感應手機方向呢?

談到此議題之前，我想先來提viewport概念
	<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
viewport即是手機browser把web page放在一個虛擬window之中，而這個window就是viewport，你可以想像他是web page的最外圍frame。

不過此code需注意，只需要注意width部分即可，我曾經試過在加上height=device-height，不過這樣會造成瀏覽器判定你要針對此width和此height去做等比變化，因此會造成你在Portrait(直立)轉向Landscape（橫向）操作時，畫面右邊會出現一大片黑背景，而不會針對各種操作方向螢幕做變化。

面對Web在行動系統或者是五花八門的螢幕時，都會需要針對特定螢幕呈現
，那麼如何去針對個螢幕調整css呢？

在css中，最常見使用的方式就是利用＠Media query，用法有兩種：

1.在Html中：
 	<link rel="stylesheet" media="screen and (min-width: 400px) and (max-width: 700px)" href="yourcssfile.css" />

2.在CSS中嵌入：
 	@media screen and (min-width: 400px) and (max-width: 700px) {
	your code....
	}

以上嵌入code的意思是說在螢幕視窗寬度介在400px~700px之間時，就套用這些css

這個方法好處，在於可以在瀏覽器的外框隨意調整視窗大小，他所對應的視窗大小的css也會有所呈現，
這樣就可以隨意來模擬各種mobile系統螢幕長寬所看到的模擬畫面
下面有幾個常見的mobile系統的視窗大小：

	/* Smartphones (portrait and landscape) ----------- */
	@media only screen 
	and (min-device-width : 320px) 
	and (max-device-width : 480px) {
	/* Styles */
	}

	/* Smartphones (landscape) ----------- */
	@media only screen 
	and (min-width : 321px) {
	/* Styles */
	}

	/* Smartphones (portrait) ----------- */
	@media only screen 
	and (max-width : 320px) {
	/* Styles */
	}

	/* iPads (portrait and landscape) ----------- */
	@media only screen 
	and (min-device-width : 768px) 
	and (max-device-width : 1024px) {
	/* Styles */
	}

	/* iPads (landscape) ----------- */
	@media only screen 
	and (min-device-width : 768px) 
	and (max-device-width : 1024px) 
	and (orientation : landscape) {
	/* Styles */
	}

	/* iPads (portrait) ----------- */
	@media only screen 
	and (min-device-width : 768px) 
	and (max-device-width : 1024px) 
	and (orientation : portrait) {
	/* Styles */
	}

	/* Desktops and laptops ----------- */
	@media only screen 
	and (min-width : 1224px) {
	/* Styles */
	}

	/* Large screens ----------- */
	@media only screen 
	and (min-width : 1824px) {
	/* Styles */
	}

除了上述使用寬度來判別之外，在new pad推出時因為帶來更高的解析度，因此也有人使用解析度來判別：

	@media only screen and (-webkit-min-device-pixel-ratio: 1.5),
	only screen and (-moz-min-device-pixel-ratio: 1.5),
	only screen and (-o-min-device-pixel-ratio: 3/2),
	only screen and (min-device-pixel-ratio: 1.5) 
當然，要如何排放這些特殊系統的css也是有一門學問，如果你想了解詳情，可以參考此篇文章：
> http://blog.hinablue.me/entry/css-media-query-tips

我也來分享一下我目前的經驗，我建議一開始（沒有media query包住的css）請以smartphone為優先設定
<br/>假設你的css是設定：
	在Desktop螢幕中設定是能看到一個大div(id:a)裡面包住四個小div
	在smartphone螢幕中設定是能看到一個大div(id:a)裡面包住四個小div
那麼你在沒有包media query部分的css如果是以Desktop為設定時，你在smartphone上因為下載速度較慢，他優先套用Desktop的設定，所以螢幕所看到的會先是"一個大div有四個div"轉變成smartphone真正要看到的樣子，這對於美觀來說並不好看。

再來，如果是用此方法在mobile系統上會有個問題必須解決，那就是效能。mobile在使用行動網路時，有時候讀取速度會非常緩慢，這時候front-end developer必須要針對code進行一些清潔，最常見的方法就是減少mobile系統讀取display:none的div，對於這個議題，這篇blog文章有詳細分析各種方法在瀏覽器上讀取的情況。
<br/>
請參考：
> http://www.qianduan.net/media-query-and-http-requests.html

觀察他的測試，可以得到一些結論

	1.如果在圖片的div(最好是用background-image方法呈現圖片)的父親div設置display:none
	2.利用media query針對各螢幕大小去區分圖片呈現

第2點我認為是觀念最簡單且最容易上手的方式，但有個缺點，在早期的瀏覽器（IE8是無法支援media query方式）不過這是可以利用條件注釋來解決
