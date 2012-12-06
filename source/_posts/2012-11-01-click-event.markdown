---
layout: post
title: "[Front-end optimization]Click event"
date: 2012-11-01 10:26
comments: true
categories: 
---


http://csc-studio.tumblr.com/post/25840306358



Now, we talk about "click event"
					/*function step1(times){	
						if (step_times == 4 ){
							jQuery('.ggp_text2').click(function(){
					        	jQuery('.ggp_select_fr').hide(100);
					        	jQuery('.ggp_occ').show(100);
					        //	jQuery(this).addClass('ggp_effect1');
					        	step_times =1;
					        	console.log(step_times);
					        	return(step);
					       		});	 
							jQuery('.ggp_next_div').click(function(){
					       		jQuery('.ggp_select_fr').hide(100);
					       		jQuery('.ggp_occ').show(100);
					       		step_times =1;
					       		return(step);
					       	})  
						}; 	
						if (step_times == 0 ){
							jQuery('.ggp_text2').click(function(){
					        	jQuery('.ggp_select_fr').hide(100);
					        	jQuery('.ggp_occ').show(100);
					        //	jQuery(this).addClass('ggp_effect1');
					        	step_times =1;
					        	console.log(step_times);
					        	return(step);
					       		});	
					       	jQuery('.ggp_text3').click(function(){
					        	jQuery('.ggp_select_fr').hide(100);
					        	jQuery('.ggp_price').show(100);
					        //	jQuery(this).addClass('ggp_effect1');
					        	step_times =2;
					        	console.log(step_times);
					        	return(step);
					       		});	
					       	jQuery('.ggp_next_div').click(function(){
					       		jQuery('.ggp_select_fr').hide(100);
					       		jQuery('.ggp_occ').show(100);
					       		step_times =1;
					       		return(step);
					       	})  
							
						}; 
						if(step_times ==1){
							jQuery('.ggp_text3').click(function(){
								jQuery('.ggp_occ').hide(100);
								jQuery('.ggp_price').show(100);
							//	jQuery('.ggp_text3').removeClass('ggp_effect1');
							//	jQuery('.ggp_text2').removeClass('ggp_effect1');
								step_times =2;
								console.log(step_times);
								return(step);
							});
							jQuery('.ggp_text1').click(function(){
								jQuery('.ggp_occ').hide(100);
								jQuery('.ggp_select_fr').show(100);
							//	jQuery('.ggp_text3').removeClass('ggp_effect1');
							//	jQuery('.ggp_text2').removeClass('ggp_effect1');
								step_times =0;
								console.log(step_times);
								return(step);
							});
							jQuery('.ggp_next_div').click(function(){
					       		jQuery('.ggp_occ').hide(100);
					       		jQuery('.ggp_price').show(100);
					       		step_times =2;
					       		return(step);
					       	})  
					       	jQuery('.ggp_pre_div').click(function(){
					       		jQuery('.ggp_occ').hide(100);
					       		jQuery('.ggp_select_fr').show(100);
					       		step_times =0;
					       		return(step);
					       	}) 
							
						};

						if(step_times ==2){
							jQuery('.ggp_text1').click(function(){
								jQuery('.ggp_price').hide(100);
								jQuery('.ggp_select_fr').show(100);
							//	jQuery('.ggp_text3').removeClass('ggp_effect1');
							//	jQuery('.ggp_text2').removeClass('ggp_effect1');
								step_times =0;
								console.log(step_times);
								return(step);
							});
							jQuery('.ggp_text2').click(function(){
								jQuery('.ggp_price').hide(100);
								jQuery('.ggp_occ').show(100);
							//	jQuery('.ggp_text3').removeClass('ggp_effect1');
							//	jQuery('.ggp_text2').removeClass('ggp_effect1');
								step_times =1;
								console.log(step_times);
								return(step);
							});
							jQuery('.ggp_pre_div').click(function(){
					       		jQuery('.ggp_price').hide(100);
					       		jQuery('.ggp_select_fr').hide(100);
					       		jQuery('.ggp_occ').show(100);
					       		step_times =1;
					       		return(step);
					       	})  
						}
					}*/