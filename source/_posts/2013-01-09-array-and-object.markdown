---
layout: post
title: "Array and Object"
date: 2013-01-09 10:49
comments: true
categories: 
---

Let's talk about Array and Object on data access application (by javascript).

----Array 

Basic operating:
Read a array:

	var user = ['blue','tony','chen'];
	console.log(user[0]);//'blue'


Add a element into a array:

	var aaa = new Array();
	aaa.push('123');
	aaa.push('456'); 
	console.log(aaa)// ['123','456']

----
Object

Basic operating:
Read a Object:
	
	var info = { user:'blue'};
	console.log(info.user);//'blue';

add a Object:
	
	var info ={}
	info.user = 'blue';
	console.log(info);//{user:'blue'}

If you have a complete data and you want to merge into a one line,you can do that:

	//combine with array and object

	var alldata = [
		{user:'blue',sex:'male'},
		{user:'Tony',sex:'male'}
	]

	//Read
	console.log(alldata[0].user) // 'blue'
	//etc...


If you have a complex data ,you will be confused about these complex code.So, you can use "break point" tools to read this data.There are lots of tools support "breakpoint":

google chrome

> https://developers.google.com/chrome-developer-tools/docs/scripts-breakpoints

visual studio 

> http://msdn.microsoft.com/en-us/library/02ckd1z7.aspx

Have Fun!


	
