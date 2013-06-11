---
layout: post
title: "Nodejs-socket.io"
date: 2012-11-10 21:17
comments: true
categories: 
---
(待寫完)

'Real Time', this conception is widely used on the web.If it combine with Nodejs, they will load more users and get higher performance.

So I introduce this module "socket.io" on Node.js.


First situation,  you sent a message, people receive .


Second situation, you sent a message,people receive except yourself.


Compare these two situations,there are lots of discuss on the forum.
However,I think that second situation is more logical than first.Because I think



Let's talk about its frame.

socket.io divid into two parts, the Client and the Server. In the Server,there are three main js(manager.js,socket.io.js,socket.js,namespace.js) to build server service.Now, I will analyze the relationship between them.

"socket.io.js" is the closet to the Client.In line 40~79,all of the Client requests will through this entering Server.From the point of view of the Server,it listens Client requests and go to execute this code.


	exports.listen = function (server, options, fn) {
		...
		return new exports.Manager(server, options);
	};

One thing to be noted the end of this code,it means call "manager.js"(we can see line 87 "exports.Manager = require('./manager');"to understand)

Before "manager.js",we see follow code :

	io.sockets.on('connection', function (socket){
		           ^^^^^^^^^^
	...
	})

Above code is exuted in Client,"io" means call a manager object,after "io" texts mean call a manager event ,and we see follow code on manager.js:
	
	(call a manger object)
	function Manager (server, options) {
	  this.server = server;
	  this.namespaces = {};
	  this.sockets = this.of('');
	  this.settings = {
		...
	  };

	(call a manager event)
	this.sockets.on('connection', function (conn) {
    	self.emit('connection', conn);
  	});

From above code,we can understand all of connections will be managed by a null string(named namespaces) 


ddd

s









manager.js
socket.io.js
socket.js



















