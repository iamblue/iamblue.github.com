---
layout: post
title: "About different users"
date: 2012-10-11 21:41
comments: true
categories: 
---
gethostname()





.htaccess
另外還有在.htaccess裡設定的，.htaccess是在Apache HTTP Server下的一個對於系統目錄進行各種權限規則設置的文件，最常見的是設置404頁面，301轉向等等，http://dev-tips.com/featured/redirect-iphone-blackberry-palm-requests-with-htaccess 有一份.htaccess設置的實例：

1: #redirect mobile browsers
2: RewriteCond %{HTTP_USER_AGENT} ^.*iPhone.*$
3: RewriteRule ^(.*)$ http://mobile.yourdomain.com [R=301]
4: RewriteCond %{HTTP_USER_AGENT} ^.*BlackBerry.*$
5: RewriteRule ^(.*)$ http://mobile.yourdomain.com [R=301]
6: RewriteCond %{HTTP_USER_AGENT} ^.*Palm.*$
7: RewriteRule ^(.*)$ http://mobile.yourdomain.com [R=301]
