---
layout: post
title: "about AJAX"
date: 2012-10-26 21:19
comments: true
categories: 
---

For javascript language ,anything must become JSON from Object!We can see following code:

Demo:

Before select one friend:

<img src="/images/post_img/about_AJAX/1.png" />

After select one friend,and ...AJAX ! (catch his occasions)

<img src="/images/post_img/about_AJAX/2.png" />

Before show my code , I assume some variable.
	1.SITE_URL: is our website's URL
<br/>
\[Html\]:

	<select class="select_fr_box" tabindex="1" onchange="getOccasion(this,'100000882157254,100002707491356&','GroupGiftProjectFacebookFriends');" id="GroupGiftProjectFacebookFriends">
		<option value="">Select friends</option>
		<option value="">blabla...</option>
	</select>

\[Javascript & jQuery\]:

	function getOccasion(ctrl,fb_user_friends,id){
		$.ajax({
			type: 'POST'
			,url: SITE_URL+'/groupgiftprojects/getOccasion'
			,data: {fb_id:ctrl.value}
			,success:
				function(data) {
					var myObject = eval('(' + data + ')');
					console.log(myObject);
					var string ='<option value="">Select One</option>';
					
					(jQuery).each(myObject, function(i,value){
						string+="<option value='"+(value.id).toString()+"'>"+(value.name).toString()+"</option>";
					});
					string+='<option value="Other">Other</option>';
					$('#GroupGiftProjectOccasionId').html(string);
					document.getElementById('id_other_text').style.display = 'none';
					document.getElementById('app_user').style.display = 'block';
					}
				})
	}	

值得注意的一點，關於line5 的data部分是要輸入和種格式？為此我去查找jQuery Lib，這是輸入JSON格式.
講到JSON其實有很多可以去討論的，先來講幾個基本的東西

首先，在javscript中轉變成JSON之前，他必須是個物件
	var a ={};
	a.username="blue";
	console.log(a.username) //blue
	JSON.stringify(a);// 這時候就是個JSON格式'{"username":"blue"}'

但要怎麼從JSON轉回javscript可用的物件呢？

	var a ='{"username":"blue"}';// 假設有個JSON
	var b = JSON.parse(a); //把JSON轉成物件
	這時候就可以像obj一樣導用JSON的函式
	console.log(b.username); //"blue"

繼續[javascript部分]：line6的success函式function(data)，這時候的data指的是回傳的JSON(從controller回傳的）再用你喜歡的方法去抓想要的JSON內部值.

此外，補充一些在用facebook api的JSON心得，關於facebook graph的GET與POST中,jQuery 的AJAX只可有POST and GET,那麼DELETE要如何去傳送呢？
其實不只有jQuery的AJAX無法直接傳送，連browser也需最近幾年的才有DELETE協定，不過這部份不列入我目前想探討的部份，用jQuery AJAX傳送DELETE可以先透過POST外加method方法，如下：
（比方說做個"取消likes"的功能）

	$.ajax({
		type: 'POST',
		url: 'https://graph.facebook.com/' + post_id + '/likes?method=delete',
		data: {
			"access_token": access_token,
			"_method": "delete"
		}
	});

[php]（controller)：

	public function getOccasion(){
		$fb_id =$_POST['fb_id'];
		$today_date = gmdate('Y-m-d',mktime(gmdate("H")+$this->me['timezone']));
		$occasion_list = $this->Occasion->query("SELECT  * FROM occasions as Occasion WHERE (Occasion.fb_id=".$fb_id." AND Occasion.recurs_annually =1) OR (Occasion.fb_id=".$fb_id." AND Occasion.recurs_annually=0 AND Occasion.starting_date >='".$today_date."' )");
		$occasion_arr =array();
		foreach( $occasion_list as $list){
			$temp =array();
			$occasion_name = $list['Occasion']['name'];
			$occasion_date = $list['Occasion']['starting_date'];
			$temp['fb_id']=$list['Occasion']['fb_id'];
			$temp['id']=$list['Occasion']['id'];
			$temp['name']=$occasion_name." - ".gmdate('F d, Y',strtotime($occasion_date));
			$occasion_arr[]=$temp;
		}
		echo json_encode($occasion_arr);
		exit;
	} 

這段php，對於AJAX來說，最重要的是line2部分:$_POST['']

這是負責抓取JSON給controller端的php函式，而最後第二行是從php轉成JSON代碼傳回給前端.

最後，來談談關於嚴謹的程式架構，我記得之前在研討會上，就有如此的疑問：

AJAX在controller端直接讀取json這個行為，究竟有沒有違反MVC架構?
我找了許多資料，嚴格的MVC架構中的C唯一的出口就是view，而這個行為就是只有從Controller順向過去View端（在呈現data時的流程）這是不可逆的，，因此證明這個答案是違反的！

但是這部份因為一樣都可以跑出結果，因此沒有受到廣泛注意，也有一些人都把此行為定義可彈性化的MVC架構，對於我來說這是有點雞肋，如果每個規範都彈性化，那規範真的就不較規範了!而這部份該如何去改善，其實現今各大主流的MVC框架都有相關作法，有空多讀讀核心原始碼即可知道!

舉例來說，以cakephp為例：

我們會在controller端把值傳到某個AJAX套用的ctp中，controller端會多以下function:

	$this->set('cookieValue', $cookieValue);
	$this->layout = 'ajax';
	(就跟平常傳值到某ctp方法一樣)

而view端會在鄉對應的ctp中去echo出值，而AJAX就直接到這ctp裡面抓值，這就完成嚴謹的MVC架構的AJAX!


	