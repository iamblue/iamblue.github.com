---
layout: post
title: "about AJAX"
date: 2012-10-26 21:19
comments: true
categories: 
---

Demo:

Before select one friend:

<img src="/images/post_img/about_AJAX/1.png" />

After select one friend,and ...AJAX ! (catch his occasions)

<img src="/images/post_img/about_AJAX/2.png" />

Before show my code , I assume some variable.
	1.SITE_URL: is our website's URL
<br/>
\[Html\]:

	<select name="data[GroupGiftProject][facebook_friends]" class="select_fr_box" tabindex="1" onchange="getOccasion(this,'100000882157254,100002707491356&','GroupGiftProjectFacebookFriends');" id="GroupGiftProjectFacebookFriends">
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

	