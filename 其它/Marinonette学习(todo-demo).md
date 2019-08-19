## Marinonette学习(todo-demo)

## 基本流程

> 1. 使用Application来控制程序的运行 （在开始之前将根布局注册，开始时将控制器注册）
>
>    ```js
>    (function() {
>      "use strict";
>    
>      // Application 
>      var TodoApp = Mn.Application.extend({
>        setRootLayout: function() {
>          // 将根布局注册
>          this.root = new TodoMVC.RootLayout();
>        }
>      });
>    
>      TodoMVC.App = new TodoApp();
>    
>      //    开始之前调用一下
>      TodoMVC.App.on("before:start", function() {
>        TodoMVC.App.setRootLayout();
>      });
>    })();
>    ```
>
>    
>
> 2. 控制器操作页面某个布局的展示 (用Mn.object.extend({})) 
>
>    ```js
>    (function() {
>      'use strict'
>    
>      //  为TodoMVC设定一个完成项的参数
>      TodoMVC.CompletedNumber = 0
>    
>      TodoMVC.Controller = Mn.MnObject.extend({
>        initialize() {
>          // 测试数据
>          var list = [
>            {title: '222',done: false}
>          ]
>          this.todoList = new TodoMVC.TodoList(list);
>        },
>        
>        // start方法来控制组件的显示
>        start() {
>          // 展示头部组件
>          this.showHeader(this.todoList)
>          // 展示尾部组件
>          this.showFooter(this.todoList)
>          // 展示中间组件
>          this.showMain(this.todoList)
>          // 判断todoList的长度 
>          this.ifShow(this.todoList)
>        },
>        
>        // 判断是否需要显示中间和底部组件
>        ifShow(todoList) {
>          if(todoList.length) {
>            $('#main').show()
>            $('footer').show()
>          }else {
>            $('#main').hide()
>            $('footer').hide()
>          }
>        },
>    
>        // 头部组件 渲染显示
>        showHeader(todoList) {
>          var header = new TodoMVC.HeaderLayout({
>            collection: todoList
>          })
>    
>          TodoMVC.App.root.showChildView('header',header)
>        },
>        
>        // 底部组件 渲染显示
>        showFooter(todoList) {
>    
>          var footer = new TodoMVC.FooterLayout({
>            collection: todoList
>          })
>    
>          TodoMVC.App.root.showChildView('footer',footer)
>        },
>        // 中间组件的显示
>        showMain(todoList) {
>          var main = new TodoMVC.MainLayout({
>            collection: todoList
>          })
>    
>          TodoMVC.App.root.showChildView('main',main)
>        }
>      })
>    
>    
>    })();
>    
>    
>    ```
>
>    
>
> 3. 布局之间的显示以及逻辑
>
>    ```js
>    (function (){
>      'use strict'
>    
>      // 整体的组件
>      TodoMVC.RootLayout = Mn.View.extend({
>        el: '#todoapp',
>    
>        // 代替的区域
>        regions: {
>          header: 'header',
>          main: 'section#main',
>          footer: 'footer'
>        }
>      })
>      // 头部组件   View需要有一个集合
>      TodoMVC.HeaderLayout = Mn.View.extend({
>    
>        template: _.template($('#header-template').html()),
>    
>        // 为要处理的对象起一个别名
>        ui: {
>          input: '#new-todo'
>        },
>        // 事件
>        events: {
>          'keydown @ui.input':'handleAdd'
>        },
>        handleAdd(e) {
>          if (e.keyCode === 13) {
>            if(!this.collection.length) {
>              $('footer').show()
>              $('#main').show()
>            }
>            if (e.target.value.trim() == "") {
>              alert("内容为空不可以输入");
>              return;
>            }
>            var item = {title:e.target.value,done:false}
>            // 添加这个item
>            this.collection.add(item);
>            e.target.value = "";
>            
>          }
>          
>        }
>      })
>    
>      TodoMVC.FooterLayout = Mn.View.extend({
>        template: _.template($('#footer-template').html()),
>    
>        // 为模版添加属性 可以这样写
>    		serializeData: function () {
>    			var completedNumber = 0;
>    			var leftNumber = this.collection.length;
>    			return {
>            completedNumber,
>            leftNumber
>    			};
>    		},
>    		initialize: function () {
>          // 监听collection的改变的事件 重新渲染
>          this.listenTo(this.collection,'add',this.reRender),
>          this.listenTo(this.collection,'change',this.reRender),
>          this.listenTo(this.collection,'reset',this.reRender)
>        }, 
>        
>        // 为要处理的对象起一个别名
>        ui: {
>          input: '#clear-completed'
>        },
>        // 事件
>        events: {
>          'click @ui.input':'handleFilter'
>        },
>        // 每次添加触发这个来重新渲染
>        /* onRender(){
>        }, */
>        reRender() {
>          var msg = {completedNumber: TodoMVC.CompletedNumber,leftNumber: this.collection.length - TodoMVC.CompletedNumber};
>          this.$el.html(this.template(msg))
>        },
>        // 处理事件
>        handleFilter() {
>          var collection = this.collection.where({done:false})
>          TodoMVC.CompletedNumber = 0
>          this.collection.reset(collection)
>          new TodoMVC.Controller().ifShow.call(this,this.collection)
>        }
>      })
>    
>      // main部分
>      TodoMVC.MainLayout = Mn.View.extend({
>        template: _.template($('#main-template').html()),
>        events: {
>          'click #toggle-all': 'handleAllChange'
>        },
>        regions: {
>          listView: {
>            el: 'ul',
>            replaceElement: true
>          }
>        },
>        initialize() {
>          this.listenTo(this.collection,'change',this.reRender),
>          this.listenTo(this.collection,'add',this.reRender)
>        },
>        onRender() {
>          this.showChildView('listView', new TodoMVC.ListView({
>            collection: this.collection
>          }))
>        },
>        handleAllChange(e) {
>          var check = e.target.checked
>          //  应该是同步的 我觉得
>          if(check) {
>            TodoMVC.CompletedNumber = this.collection.length
>          }else {
>            TodoMVC.CompletedNumber = 0
>          }
>          this.collection.each(model=>{
>            model.set('done',check)
>          })
>          
>        },
>        reRender() {
>          if(this.collection.length == TodoMVC.CompletedNumber) {
>            $('#toggle-all').prop('checked',true)
>          }else {
>            $('#toggle-all').prop('checked',false)
>          }
>        }
>      })
>    })()
>    ```
>
>    
>
> 4. TodoList (Main组件包含的逻辑)
>
>    ```js
>    (function() {
>      TodoMVC.TodoList = Backbone.Collection.extend({});
>    
>      TodoMVC.ItemView = Mn.View.extend({
>        template: _.template($('#li-template').html()),
>        tagName: 'li',
>        // ui
>        events: {
>          'click .toggle': 'onHandleCheck'
>        },
>        initialize() {
>          this.listenTo(this.model,'change',this.reRender)
>        },
>        onHandleCheck(e) {
>          var check = e.target.checked
>          if(check) {
>            TodoMVC.CompletedNumber ++
>          }else {
>            TodoMVC.CompletedNumber -- 
>          }
>          this.model.set('done',check)
>        },
>        reRender() {
>          this.$el.toggleClass('done')
>          this.$el.find('.toggle').prop('checked',this.model.get('done'))
>        }
>      })
>    
>      TodoMVC.ListView = Mn.CollectionView.extend({
>        childView: TodoMVC.ItemView,
>        tagName: 'ul',
>        id: 'todo-list'
>      })
>    })();
>    ```
>
>    
>
> 5. 页面启动的控制（TodoApp）
>
>    ```js
>    (function() {
>      "use strict";
>    
>      TodoMVC.App.on("start", function() {
>        // 开始的时候注册一个 控制器  
>        var controller = new TodoMVC.Controller();
>        // start调用  组件进行渲染
>        controller.start();
>      });
>      // 开始
>      TodoMVC.App.start();
>      
>    })();
>    ```
>
>     

## 延伸  学习到的方法

> ```js
> 	var list = [
>       {title: '222',done: false}
> 	]
> 	this.todoList = new TodoMVC.TodoList(list);
> // 和 backbone一样 也需要collection 集合model事件
> ```
>
> ```js
> 	showFooter(todoList) {
>       var footer = new TodoMVC.FooterLayout({
>         collection: todoList
>       })
> 
>       TodoMVC.App.root.showChildView('footer',footer)
>     },
>  // 底部组件的渲染 需要传入collection
> ```
>
> ```js
> TodoMVC.FooterLayout = Mn.View.extend({
>     template: _.template($('#footer-template').html()),
> 
>     // 为模版添加属性 可以这样写
> 		serializeData: function () {
> 			var completedNumber = 0;
> 			var leftNumber = this.collection.length;
> 			return {
>         completedNumber,
>         leftNumber
> 			};
> 		},
> 		initialize: function () {
>       // 监听collection的改变的事件 重新渲染
>       this.listenTo(this.collection,'add',this.reRender),
>       this.listenTo(this.collection,'change',this.reRender),
>       this.listenTo(this.collection,'reset',this.reRender)
>     }, 
>     
>     // 为要处理的对象起一个别名
>     ui: {
>       input: '#clear-completed'
>     },
>     // 事件
>     events: {
>       'click @ui.input':'handleFilter'
>     },
>     // 每次添加触发这个来重新渲染
>     /* onRender(){
>     }, */
>     reRender() {
>       var msg = {completedNumber: TodoMVC.CompletedNumber,leftNumber: this.collection.length - TodoMVC.CompletedNumber};
>       this.$el.html(this.template(msg))
>     },
>     // 处理事件
>     handleFilter() {
>       var collection = this.collection.where({done:false})
>       TodoMVC.CompletedNumber = 0
>       this.collection.reset(collection)
>       new TodoMVC.Controller().ifShow.call(this,this.collection)
>     }
>   })
> 
> 
> // 底部组件的逻辑。可以监听collection事件
>   
> // template模板引擎
> template: _.template($('#footer-template').html()),
> // 对模版的渲染 需要添加一个默认值。这样写。 不然一直报错 
> serializeData: function () {
> 			var completedNumber = 0;
> 			var leftNumber = this.collection.length;
> 			return {
>         completedNumber,
>         leftNumber
> 			};
> 		},
> ```
>
> ```js
> regions: {
>       listView: {
>         el: 'ul',
>         replaceElement: true
>       }
>     },
>  // main组件中。这是添加一个区域。 并且replace.. 可以替代父组件的ul内容 （子组件把这部分内容都替代了）
>  // 展示的demo
>  onRender() {
>       this.showChildView('listView', new TodoMVC.ListView({
>         collection: this.collection
>       }))
>     },
>  // onRender 是一个渲染函数。在页面加载过程中会执行一次
> ```
>
> ```js
> TodoMVC.ItemView = Mn.View.extend({
>     template: _.template($('#li-template').html()),
>     tagName: 'li',
>     // ui
>     events: {
>       'click .toggle': 'onHandleCheck'
>     },
>     initialize() {
>       this.listenTo(this.model,'change',this.reRender)
>     },
>     onHandleCheck(e) {
>       var check = e.target.checked
>       if(check) {
>         TodoMVC.CompletedNumber ++
>       }else {
>         TodoMVC.CompletedNumber -- 
>       }
>       this.model.set('done',check)
>     },
>     reRender() {
>       this.$el.toggleClass('done')
>       this.$el.find('.toggle').prop('checked',this.model.get('done'))
>     }
>   })
> 
>   TodoMVC.ListView = Mn.CollectionView.extend({
>     childView: TodoMVC.ItemView,
>     tagName: 'ul',
>     id: 'todo-list'
>   })
> // 这个是marinonette的写法 比backbone 少了一个需要自己渲染页面的步骤
> ```
>
>  
