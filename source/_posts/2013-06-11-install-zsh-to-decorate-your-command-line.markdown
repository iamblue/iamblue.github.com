---
layout: post
title: "Install Zsh to decorate your command line"
date: 2013-06-11 18:16
comments: true
categories: 
---
zsh feature:(oh my zsh --> agnoster themeplate)

1. 只要按tab鍵即可快速顯示該層資料夾，並可用鍵盤去做選擇
2. 可自定義快捷鍵

	ex. git commit -am 'update'可去自行設成 git ac 'update'

3. 常用語法打錯會有防錯機制
	
	ex. ls打成sl他會自動判定成ls

4. 明確的顏色顯示該專案是否有東西更新（需要commit)，以及現在的分支是master還是其他分支


Install steps:

1. sudo apt-get install zsh
2. curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sudo sh
3. vim ~/.zshrc 改這行：ZSH_THEME="agnoster"
4. 若不喜歡中間的pwd路徑的話
	去 vim ~/.oh-my-zsh，把
	
	# Dir: current working directory

	prompt_dir() {
		prompt_segment blue black '%~' 
	}
	的'%~'改成'%c' 

5. 其他參照：
https://gist.github.com/agnoster/3712874
6. 文字亂碼部分：
https://gist.github.com/qrush/1595572

7. 遇到的ubuntu上亂碼問題：
https://github.com/robbyrussell/oh-my-zsh/issues/1906
	
	cd ~/.oh-my-zsh/themes/
	git checkout  d6a36b1 agnoster.zsh-theme

