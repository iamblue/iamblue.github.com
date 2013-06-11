---
layout: post
title: "[Project]About issue workflow"
date: 2012-11-02 10:16
comments: true
categories: 
category_icon: /images/back-end.png
---

本篇想來探討關於issue的workflow，記錄解決issue的流程必須非常清楚，這有許多好處：

	1.方便後人閱讀,維護
	2.對於程式設計師來說，這是一個很好的記錄
	3.讓團隊工作系統更有條理

這是我目前"記錄issue"主要的五個項目：

	1.Observation:（觀察） 觀察問題，描述問題
	2.Analysis & Design: （分析）  想想可能得解決辦法，或者是可能的狀況
	3.Scope:（記錄）  施工，並且在這個項目記下有更改過的檔案路徑
	4.Done:（完成） 完成，並寫下完成的路徑以及最主要更改過的檔案
	5.Bottleneck:（瓶頸）這部份記錄關於此次issue所遇到的瓶頸和解決心得。



以下是範例
Issue name:

	Set up footer div and eanable footer page.
	footer page's content on google drive "WhatsWorthy User Agreement & Privacy Policy".

--

Author:Blue Chen
Created:	11/31
Start:	11/15 08:47
End:		11/15 16:30
Duration:	1d

--

Observation:
	Most of Our pages have the functionality of receiving data from external sites, it have resulted in some of the dynamic height .
	According to web architecture,css and html will be read before javascript,we can't receive correct page's height to make footer div.So, we should build a way to solve this problem.


Analysis & Design:
	1.Design footer layout
	2.set regular footer style of css
	3.Relate to dynamic height,we should set special javascript on every page.

Scope:(記錄更改過的檔案)
	1./theme/2/css/main_layout.css
	2./theme/2/layout/default.ctp

Done:
	1./theme/2/css/main_layout.css
	2./theme/2/layout/default.ctp

Bottleneck:
	1. Setting regular footer style of css

