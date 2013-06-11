---
layout: post
title: "onclick and addEventListner"
date: 2012-12-06 23:18
comments: true
categories: 
---


onclick event and addEventListner("click",fuc(),boolin)
The major difference of theme is their DOM type."Onclick event" is DOM 0,and addEventListner("click",fuc(),boolin) is DOM 2.DOM 0 means it controls a single event, and DOM 2 controls a multiple events,


Most of using javascript to build app platforms have forbidden operating DOM 0.Like as winJS,you can see following code:

	<Button id="yourid" onclick="yourcodename();" value="hihi">

	<script>
		function yourcodename(){
			//your code
		}
	</script>

Above call behavior is not used on winJS,we must change DOM 0 type to DOM 2 type:

	<Button id="yourid" value="hihi">
	<div id="showfn">
	</div>
	<script>
		var listnerbtn = document.getElementById('yourid');
		listnerbtn.addEventListner('click', ,false)
		function show(){
			var addhtmltagcontent = '';
			addhtmltagcontent += '<div>I am Blue Chen!</div>';
			document.getElementById('showfn').innerHTML= addhtmltagcontent ;
		}
	</script>

Above code will be used!However, if you want to call a function which is bringing a variable:

		
	<Button id="yourid" onclick="yourcodename(str)" value="hihi">
											  ^^^^
You can do that:

	<Button id="yourid" value="hihi">
	<div id="buttonidvariable" style="display:none">
		document.write(str);
	</div>
	<script>
		var listnerbtn = document.getElementById('yourid');
		listnerbtn.addEventListner('click', getvalue,false)
		function getvalue(){
			var getstrvalue = document.getElementById('buttonidvariable').innerHTML
			doyouwanttodofn(getstrvalue );
		}
		function doyouwanttodofn(){
			//your code
		}
	</script>

Enjoy it!