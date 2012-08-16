---
layout: post
title: "github on coworking and Mac"
date: 2012-08-16 22:35
comments: true
categories: 
---
工作至今在coworking上
一直會出現與同事的issue文件上傳衝突的問題

從之前推測許多原因，找了許多方式解決並在此記錄一下一點心得：

1.master與branch問題
因為之前專案還算小，所以大家都在dev（dev為本公司網站的主幹，以下簡稱dev)上開發
等到人多（例如新加入一位夥伴）或者是專案進行複雜（同時有兩個夥伴正在進行某個issue）時
就容易發生conflict狀況

這時候必須要善用git的branch
branch其實就是coder各自在自己的branch上工作
等到自己的專案結束時在把專案丟回主幹，這樣的好處有以下：
	1)以後公司更新時（或者是想換某個branch 上的plugin時)可以很快的切換過去
	2)而在後續的專案追蹤上也可以利用branch去區隔程式

不過請注意一下這時候的branch的pull與push觀念步驟：

在此之前先提一下在主幹（dev）操作的過程是commit -> pull ->push

第一部分：
在branch上製作的專案請先commit

第二部分（先從github的主幹上面下載別人做的東西，跟在dev做的pull即是一樣的道理）
	1) switch to Local/dev: git checkout dev
	2) pull: git pull
	3)switch to Local/[topic] branch: git checkout [topic]
	4) merge Local/dev branch into currently checked out branch (topic): git merge dev

第三部分（把自己的branch與主幹merge並丟上github跟push道理是一樣的)

	1)switch to Local/dev branch: git checkout dev
	2)pull: git pull
	3)merge topic branch into currently checked out branch (Local/dev): git merge [topic]
	4)push (if ready to share): git push
	5)switch back to Local/topic branch to continue development: git checkout [topic]



github on Mac 