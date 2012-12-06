---
layout: post
title: "BACKBONE.JS"
date: 2012-11-26 10:22
comments: true
categories: 
---

BACKBONE.js 

Before learning BACKBONE.js, we should know what extended javascript lib will be included in BACKBONE.js project.

1.JQuery
2.UNDERSCORE.js


BACKBONE.js brings the conception of MVC into front-end.
The major difference between BACKBONE.js MVC and Back-end MVC is the word of C. Generally,back-end C mean 'controller',but BACKBONE.js C mean 'Collection'. 


BACKBONE.JS core:


1.Router

	var url  = Backbone.Router.Extend({
	    routes:{
	      '/isdate': 'getPost',
	      '*actions': 'errorPage'
	    },
	    
	    getPost: function(id){
	      alert(id)
	    },
	    errorPage: function(actions){
	      alert('sorry we can not find'+actions+'page');
	    }
  	});


2.Model

When you are recieving a data from front-end and you want to build(or modify) this data to storage(or database).Of course, before you building this data into database, you must give this data some limitations and write in Model.

	var Todo = Backbone.Model.extend({

	    // Default attributes for the todo item.
	    //設定預設值
	    defaults: function() {
	      return {
	        title: "empty todo...",
	        order: Todos.nextOrder(),
	        done: false
	      };
	    },

	    // Ensure that each todo created has `title`.
	    //設定每一筆資料都不能為空，限制title不為空
	    initialize: function() {
	      if(!this.get("title")) {
	        this.set({
	          "title": this.defaults().title
	        });
	      }
	    },

	    // Toggle the `done` state of this todo item.
	    //這邊比較難理解，等等看view就會知道
	    toggle: function() {
	      this.save({
	        done: !this.get("done")
	      });
	    }
  	});

3.Collection

Like back-end controller, BACKBONE.js collection do parsing on database.However,BACKBONE.js collection can deal with all model or single model.It very convenient for coder to do more combination on front-end. 


	var TodoList = Backbone.Collection.extend({
	    model: Todo,
	  	
	  	// Save all of the todo items under the `"todos-backbone"` namespace.
	    localStorage: new Backbone.LocalStorage("todos-backbone"),
	  

	  	// Filter down the list of all todo items that are finished.
	    //從LocalStorage獲取已經完成的
	    done: function() {
	      	return this.filter(function(todo) {
	        	return todo.get('done');
	      	});
	    },
	    //補充filter函數就是做搜尋符合字串者就提出來
	    //http://documentcloud.github.com/underscore/#filter


	    //在資料表中獲取未完成數據
	    remaining: function() {
	    	return this.without.apply(this, this.done());
	    },
	    //without 即是把符合字串者的排除掉
	    //http://documentcloud.github.com/underscore/#without
	    //apply即傳入函數的意思，這是javascript 原生語法

	   
	    //獲得下一組的排序序號
	    nextOrder: function() {
	    	if(!this.length) return 1; 
	    	return this.last().get('order') + 1;
	    },

	    // 讓資料依據order來排列
	    comparator: function(todo) {
	    	return todo.get('order');
	    }
	   	//comparator為backbone的内置函数，作用就是collection中數據排序的依據
	    //function(model){
	    //  return todo.get('JSON中排序的依據');
	    //}
	    //http://documentcloud.github.com/backbone/#Collection-comparator
	
	});
	var Todos = new TodoList;
	//這個意思是只創建一個全局的todo  collection

4.View
In the demo,its view divid into two parts. 

Part1.

This view acting on every todo item

<img src='/images/post_img/BACKBONE_JS/backbone_demo_view1.png'>

	var TodoView = Backbone.View.extend({

	   	//... is a list tag.
	    tagName: "li",
	    //作用:  把<script type="text/template">標籤的html碼放在這之中

	    // Cache the template function for a single item.
	    template: _.template($('#item-template').html()),
	    //抓取id 為item-template 的<script type="text/template">裡面的html碼

	    // The DOM events specific to an item.
	    events: {
	      	"click .toggle": "toggleDone",
	      	"dblclick .view": "edit",
	      	"click a.destroy": "clear",
	      	"keypress .edit": "updateOnEnter",
	      	"blur .edit": "close"
	      	// blur 指的是輸入域失去焦點時就動作
	      	//請參考jquery http://www.w3school.com.cn/jquery/event_blur.asp
	    },
	    //above all 可以參考這個BACKBONE api說明
	    //http://documentcloud.github.com/backbone/#View-extend


	    // The TodoView listens for changes to its model, re-rendering. Since there's
	    // a one-to-one correspondence between a **Todo** and a **TodoView** in this
	    // app, we set a direct reference on the model for convenience.
	    //初始化
	    initialize: function() {
	      	this.model.on('change', this.render, this);
	      	//http://documentcloud.github.com/backbone/#Model-changed
	      	this.model.on('destroy', this.remove, this);
	      	//http://documentcloud.github.com/backbone/#Model-destroy
	    },

	    // Re-render the titles of the todo item.
	    render: function() {
	      	this.$el.html(this.template(this.model.toJSON()));
	      	this.$el.toggleClass('done', this.model.get('done'));
	      	//$el 是召喚jQuery 
	      	this.input = this.$('.edit');
	      	return this;
	    },

	    // Toggle the `"done"` state of the model.
	    toggleDone: function() {
	      	this.model.toggle();
	    },
	    //*******************************
	    // 回到Model的toggle function
	    //*******************************


	    // Switch this view into `"editing"` mode, displaying the input field.
	    edit: function() {
	      	this.$el.addClass("editing");
	      	this.input.focus();
	      	//在textfield裡面出現可以打字狀態
	    },

	    // Close the `"editing"` mode, saving changes to the todo.
	    close: function() {
	      	var value = this.input.val();
	      	if(!value) {
	        	this.clear();
	      	} else {
	        	this.model.save({
	          	title: value
	        	});
	        	this.$el.removeClass("editing");
	      	}
	    },

	    // If you hit `enter`, we're through editing the item.
	    updateOnEnter: function(e) {
	    	if(e.keyCode == 13) this.close();
	    },

	    // Remove the item, destroy the model.
	    clear: function() {
	      	this.model.destroy();
	    }

	  });

part2.

<img src='/images/post_img/BACKBONE_JS/backbone_demo_view2.png'>

	var AppView = Backbone.View.extend({

	    // Instead of generating a new element, bind to the existing skeleton of
    	// the App already present in the HTML.
    	el: $("#todoapp"),
    	//把下面事件綁定在todoapp裡面

    	// Our template for the line of statistics at the bottom of the app.
    	statsTemplate: _.template($('#stats-template').html()),

	    // Delegated events for creating new items, and clearing completed ones.
	    events: {
	      	"keypress #new-todo": "createOnEnter",
	      	"click #clear-completed": "clearCompleted",
	      	"click #toggle-all": "toggleAllComplete"
	    },

	    // At initialization we bind to the relevant events on the `Todos`
	    // collection, when items are added or changed. Kick things off by
	    // loading any preexisting todos that might be saved in *localStorage*.
	    initialize: function() {

	      	this.input = this.$("#new-todo");
	      	this.allCheckbox = this.$("#toggle-all")[0];

	      	Todos.on('add', this.addOne, this);
	      	Todos.on('reset', this.addAll, this);
	      	Todos.on('all', this.render, this);

	      	this.footer = this.$('footer');
	      	this.main = $('#main');

	      	Todos.fetch();
	      	//讓你知道目前從local storage取出的最新狀態
	    },

    	// Re-rendering the App just means refreshing the statistics -- the rest
    	// of the app doesn't change.
	    render: function() {
	      	var done = Todos.done().length;
	      	var remaining = Todos.remaining().length;

	      	if(Todos.length) {
	        	this.main.show();
	        	this.footer.show();
	        	this.footer.html(this.statsTemplate({
	          		done: done,  //傳值到html
	          		remaining: remaining //傳值到html
	        	}));
	      	} else {
	        	this.main.hide();
	        	this.footer.hide();
	        	//如果都沒有的話main 和footer div都會消失
	      	}
      		this.allCheckbox.checked = !remaining;
    	},

    	// Add a single todo item to the list by creating a view for it, and
    	// appending its element to the `<ul>`.
	    addOne: function(todo) {
	      	var view = new TodoView({
	        	model: todo
	      	});

	    	//新增一個上面提的TodoView 
	    	this.$("#todo-list").append(view.render().el);
	    },

    	// Add all items in the **Todos** collection at once.
    	//把Todos中的所有数据渲染到页面,页面加载的时候用到  
    	addAll: function() {
      		Todos.each(this.addOne);
    	},

    	// If you hit return in the main input field, create new **Todo** model,
    	// persisting it to *localStorage*.
    	createOnEnter: function(e) {
	      	if(e.keyCode != 13) return;
	      	if(!this.input.val()) return;

	      	Todos.create({
	        	title: this.input.val()
	      	});
	      	this.input.val('');
    	},

	    // Clear all done todo items, destroying their models.
	    clearCompleted: function() {
	      	_.invoke(Todos.done(), 'destroy');
	      	return false;
	    },

	    toggleAllComplete: function() {
	      	var done = this.allCheckbox.checked;
	      	Todos.each(function(todo) {
	        	todo.save({
	          		'done': done
	        	});
	      	});
	    }

	});
  	// Finally, we kick things off by creating the **App**.
  	var App = new AppView;
  	//別忘了要創建出這個view

Front-end layout:

1. Template:



