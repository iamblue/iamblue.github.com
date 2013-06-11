---
layout: post
title: "validate on different format count"
date: 2012-12-06 08:32
comments: true
categories: 
---

About count format.

In America, most of people are used to using “1,000.00” format.However,this format is not written in the database directly.In today’s common MVC framework. They have some similar attempts to prevent this format write in the database.But these attempts often build in back-end.Standing on the user’s view,if you type this format and sent this page. You will see an error page after few second and you will type it again…The phenomenon is not a good UI.So, I suggest we should do validation on the front-end and solve this problem.

View

<img src="/images/post_img/validate_on_count/product_info_1.png"/>

We have four form to type in.First form is Estimated Price. Second form is Estimated shipping.Third form is Estimated Tax.Fourth form is Total cost which is combine above (three) form' value.When we type one of three forms ,the "Total cost" form will replace its value.

Validate code(javascript)

1.capture form value

	var EstimatedPrice =document.getElementById("EstimatedPrice").value,
		EstimatedShipping =document.getElementById("EstimatedShipping").value,
		EstimatedTax=document.getElementById("EstimatedTax").value;	

hint! the type of this value is Number. We have to change the type to "String".

	var RegEstimatedPrice = EstimatedPrice.toString(),
		RegEstimatedTax= EstimatedTax.toString(),
		RegEstimatedShipping= EstimatedShipping.toString();

After change the type to string, we can use "replace" function to exclude ","

	RegEstimatedPrice = RegEstimatedPrice.replace(',','');
	RegEstimatedTax = RegEstimatedTax.replace(',','');
	RegEstimatedShipping = RegEstimatedShipping.replace(',','');

We want to check the content of string,don't allow non-number context.(Using the regular expression)

	var score = [RegEstimatedPrice ,RegEstimatedTax,RegEstimatedShipping],
		regexCheck = /^-?\d+\.?\d*?$/;

	var checktotal =0;
	if (score[0].match(regexCheck)==null || score[1].match(regexCheck)==null ||score[2].match(regexCheck)==null){ 
		    checktotal=1;
	}

If the string pass above validation, we change its type(string to Number).why? See follow.

	RegEstimatedPrice = Number(RegEstimatedPrice);
	RegEstimatedTax = Number(RegEstimatedTax);
	RegEstimatedShipping = Number(RegEstimatedShipping);
	
Follow function will be excute when the type of variable is number .

	var total = RegEstimatedTax+RegEstimatedShipping+RegEstimatedPrice;

2.Currency

	currency = '';
	
	if(document.getElementById("Currency").value == "CAD")
		currency = "$";		
	else if(document.getElementById("Currency").value == "EUR")
		currency = "&euro;";		
	else if(document.getElementById("Currency").value == "GBP")
		currency = "&pound;";
	else if(document.getElementById("Currency").value == "JPY")
		currency = "&yen;";		
	else
		currency = "$";
		
3.total count

	if(checktotal==0){
		document.getElementById("id_total_amt").innerHTML= "<b>"+currency+""+total.toFixed(2)+"</b>";
		document.getElementById('needprice_estimate').value = total;
	}
	else{
		document.getElementById("id_total_amt").innerHTML=currency+"NaN";
		document.getElementById('needprice_estimate').value = currency+"0.00";
	}


