---
layout: post
title: "Browser Compatibility on Html5"
date: 2012-11-01 20:55
comments: true
categories: 
---


The major difference between Html5 and xHTML is Html5 add more tag element,such as  '&lt;header&gt;' , '&lt;nav&gt;'....and so on.

We use these tag elements ,we also notice these tag element whether use in old browser at the same time.
There I have one way to solve this problem.

First, we can use javascript to create element. Second,use css make this element become a "block" element(In other words,they can display on the web page)

As follows：

[javascript]
	<!--[if IE]-->  
	<script type="text/javascript">
	document.createElement("header");
	document.createElement("footer");
	document.createElement("section");
	document.createElement("aside");
	document.createElement("nav");
	document.createElement("article");
	document.createElement("hgroup");
	document.createElement("time");
	</script>
	<!--[endif]-->


[css]
 	section,header,footer,section,aside,nav,article,hgroup,time{
 	display:block
	}