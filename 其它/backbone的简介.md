## backbone

## 需要的js文件

> - Backbone.js
> - underScore.js
> - jquery.js

## 对backbone的理解

> backbone是mvc。有model view  以及controller
>
> ### 重要的方法
>
> Backbone.Model.extend()  
>
> > 1. 将obj可以变为model模型，并且拥有了model的方法事件
> > 2. new 出来的对象可以很方便和view进行联系
>
> Backbone.View.extend()
>
> > 1. 将model变成view，通过render函数进行渲染
> > 2. 有内置的方法，可以方便的进行dom元素操作
>
> Backbone.Collection.extend()
>
> > 1. model的集合
> > 2. 有一些自己的方法，可以方便的进行模型集合的操作

## Backbone.Model.extend的用法

> ```js
> var obj = {
>    'title': 'task1',
>    'description': 'description'
>  }
> 
>  var TodoItem = Backbone.Model.extend({})
> 
>  var todoItem = new TodoItem(obj)
> 
>  console.log(todoItem)
> 
>  // todoItem.JSON().title = 'task3' 并没有起作用  再次toJSON还是原来的数据
> 
>  // get 获取  set 对某一属性赋值
> ```
>
> ```js
> var obj = {
>    'title': 'task1',
>    'description': 'description'
>  }
> 
>  var TodoItem = Backbone.Model.extend({})
> 
>  var todoItem = new TodoItem(obj)
> 
> 
>  // 内部会进行属性的比较  如果没有变化不会触发change事件，需要属性变化才会触发change事件 
>  // set 中有一个option  slient:true 可以更改内容不触发onchange事件
>  todoItem.on('change',function(){
>    if(this.hasChanged('title')){
>      console.log('changed title')
>    }
>  })
>  // trigger可以触发自定义事件
> 
>  // on('change:title') 只针对title属性进行监听
> 
>  //取消监听  off() 没有参数就全部取消
> 
>  // off('change:title') 把这个监听的取消掉
> 
>  // once 只执行一次  相当于 on 了之后 就  off掉
> 
>  var todoItem2 = new TodoItem()
> 
>  // todoItem2监听todoItem  
>  todoItem2.listenTo(todoItem,'change:title',function(){
>    console.log(this) // 这个this指向 todoItem2
> 
>    // listenToOnce 
>  })
> ```
>
> 

## Backbone.View.extend的用法

> ```js
> var TodoItemView = Backbone.View.extend({
>      // 三个内置属性
>      tagName: "p",
>      className: "haha",
>      id: "pp",
>      //如果其它属性的话  需要在attributes里边写
>      // attributes:{
>      //   style: 'color:red;'
>      // },
>      // 页面上有值可以这样写
>      // el:'div#header',
>      render: function() {
>        this.$el.html("<span>1111</span>");
>        return this;
>      }
>    });
> 
>    var todoItemView = new TodoItemView({
>    });
> 
>    todoItemView.render().$el.appendTo("body");
> 
>    console.log(todoItemView);
> 
>    // todoItemView.remove()  这个方法内置 可以把界面上的元素移除
> ```
>
> ```js
> var ToDoItem = Backbone.Model.extend({
> 
>  })
> 
>  var todoItem = new ToDoItem({
>    title:'task',
>    des: 'des'
>  })
> 
>  var todoItem2 = new ToDoItem({
>    title:'task2',
>    des: 'des2'
>  })
>  
>  var  ToDoItemView = Backbone.View.extend({
>    tagName: 'div',
>    className: 'todo-Item',
>    // 触发事件
>    events: {
>      // 后边字符串就是定义的方法
>      // 'click': 'handleClick'
>      // 后边可以跟jq选择器来指定某一个元素使用这个用法
>      'click button': 'handleClick'
>    },
>    // 这一步写错了 怪不得  initialize 拼错  尴尬。。。
>    initialize: function() {
>      this.listenTo(this.model,'change',this.render)
>      this.listenTo(this.model,'destroy',this.remove)
>    },
>    render:function() {
>      var json = this.model.toJSON()
>      this.$el.html('<h3>'+json.title+'<br/>'+json.des+'</h3><button>delete</button>')
>      return this
>    },
>    handleClick: function() {
>      console.log('为什么不行啊')
>      console.log(this.model)
>      this.model.destroy()
>    }
> 
>  })
> 
>  var todoItemView = new ToDoItemView({
>    model:todoItem
>  })
> 
>  var todoItemView2 = new ToDoItemView({
>    model:todoItem2
>  })
> 
>  todoItemView.render().$el.appendTo($('body'))
>  todoItemView2.render().$el.appendTo($('body'))
> ```
>
>  

## Backbone.Collection.extend的用法

```js
var objList = [
   {title:'aaa'},
   {title:'bb'},
   {title:'cccc'},
 ]

 var MyList = Backbone.Collection.extend({

   // sort函数进行排序   必须要有一个返回值  只是单纯的返回一个 true或者false值的话不调用这一个函数
   // return model.get('title').length < model2.get('title').length 这样不行
   comparator: function(model,model2) {
     if(model.get('title').length < model2.get('title').length)
       return -1
     else if(model.get('title').length = model2.get('title').length)
       return 0
     else {
       return 1
     }
   }
   // 为什么不排序啊啊啊？
   // comparator: function(model,model2) {
   //   console.log(model)
   //   console.log(model2)
   //   return model.get('title').length 
   //   /* console.log(model.get('title'))
   //   console.log(model2.get('title')) */
   // }
   /* function(a,b) {
     return a.get('title') < b.get('title')
   } */
 })

 var aList = new MyList(objList)

 var ItemView = Backbone.View.extend({
   tagName: 'li',
   className: 'list-item',
   render() {
     // get方法
     this.$el.html(this.model.get('title'))
     return this
   }
 })

 //   方法
 var ListView = Backbone.View.extend({
   initialize: function() {
     if(this.collection) {
       this.byId = {}
       this.views = []
       // each方法 为每一个都注册上
       this.collection.each(this.registerView,this)
       this.listenTo(this.collection,'add',this.addView)
       this.listenTo(this.collection,'remove',this.removeView)
       this.listenTo(this.collection,'reset',this.resetView)
       this.listenTo(this.collection,'change',this.updateView)
       this.listenTo(this.collection,'sort',this.resetView)
     }
   },
   registerView(model) {
     var view = new ItemView({model})
     // cid 是每个model的唯一值
     this.byId[model.cid] = view
     this.views.push(view)
   },
   render() {
     var _this = this
     this.$el.empty()
     _.each(this.views,function(view){
       $_el = view.render().$el
       _this.$el.append($_el)
     })
     return this
   },
   addView(model) {
     // 只要下边添加  model中就有了
     var view = new ItemView({model})
     var at = this.collection.indexOf(model)
     this.byId[model.cid] = view
     var $before = this.views[at-1].$el
     $before.after(view.render().$el)
     this.views.splice(at,0,view)
   },
   removeView(model) {
     // 这个是新添加一个用的  明明是删除  怎么可以这样用呢？？？  尴尬
     // var view1 = new ItemView({model})
     var view = this.byId[model.cid]
     // model删除了  不能用collection 来查找   view删除  this.views也同步删除
     view.remove()
     delete this.byId[model.cid]
     // 因为这个时候已经删除了 所以下边这样找是找不到的  需要使用views来寻找这个
     // var index = this.collection.indexOf(model)
     var index = _.indexOf(this.views,view)
     this.views.splice(index,1)
   },
   resetView() {
     console.log(1)
     // 重新清空  再来一次
     this.byId = {}
     this.views = []
     this.collection.each(this.registerView,this)
     this.render()
   },
   updateView(model) {
     var view = this.byId[model.cid]
     view.render()
   }
  
 })

 var aView = new ListView({el:'#aView',collection:aList})

 aView.render()

 aList.add({
   title: 'title.add', 
 },{at:2})

 aList.add({
   title: 'title.add2', 
 },{at:3})

 var newObj = [
   {title: '6666'},
   {title: '66666'},
   {title: '777777'},
 ]
```

## backbone与服务器交互常用方法（Restful风格）

> ```js
> var Task = Backbone.model.extend({
>    // 
>    // idAttribute:'guid', 唯一标识更改  默认是id
>    urlRoot: 'url' // 必须  与远程服务器进行连接
>  })
> 
> 
> 
>  var task = new Task({
>    
>    'title':'course1',
>    'description':'des1'
>  })
> 
>  task.save() // 没有id值时 发送 POST请求  给一个id
>  // 有 id值时 发送PUT或者PATCh请求
> 
>  task.save('title','course2') // 更改值
> 
>  task.fetch() //抓取远程服务器的内容  GET请求
> 
>  task.destroy() //删除  PUT请求
> 
>  // sync 事件 只要和服务器交互都会触发
> ```
>
> 

## 延伸

## 1. 什么叫restful风格

> 非restful示例
>
> ```js
> http://localhost:8080/employee/get/{id} GET
> http://localhost:8080/employee/delete/{id} POST/GET
> http://localhost:8080/employee/create POST
> http://localhost:8080/empliyee/update POST
> ```
>
> restful示例
>
> ```js
> http://localhost:8080/employee/{id} GET
> http://localhost:8080/employee DELETE
> http://localhost:8080/employee POST
> http://localhost:8080/empliyee PUT
> ```
>
> 就是按照严格的http请求来处理  get 获取。post 增加。delete 删除 put / patch 更改
>
> 这样有利于前端和后端的交互就有了一个标准，便于之间的交互以及错误的查找

## 2. jquery操作dom表单

> ```js
> // jQuery1.6以后的版本用到的是prop。用attr的话不会多次实现，因为attr不会记录当前checkbox的选中状态
> ```
>
> 遇到的坑
>
> ```js
> this.collection.each(model=>{
>          // 这个model改变
>          model.set("done",check);      
>        })
>        $('.toggle').prop('checked',check)
>        // 当用下面的方法的时候 显示有bug 可能是因为attribute和属性值的问题        
>        //$('.toggle').attr('checked',check)
> ```
>
> 原生js 的属性和attributes的区别
>
> > ```html
> > <input type="text" id='mytext'>
> > <script>
> > 		var mytext = document.getElementById('mytext')
> >  console.log(mytext.id) //mytext
> >  console.log(mytext.value) // ''
> >  console.log(mytext.aaa) // undefined
> > 
> >  /* mytext.setAttribute('value','111')
> >  console.log(mytext.getAttribute('value')) // 111
> >  console.log(mytext.value) // 111
> >  mytext.value = '222'
> >  console.log(mytext.getAttribute('value')) // 111 */
> > 
> >  /* mytext.value = 111
> >  console.log(mytext.getAttribute('value')) //null */
> > 
> > /*  mytext.setAttribute('aaa','333')  // 这样页面上是 <input type="text" id='mytext' 		aaa='333'>
> >  console.log(mytext.getAttribute('aaa')) // 333
> >  console.log(mytext.aaa) // undefined */
> > 
> >  /* mytext.aaa = 333 // 这样页面上还是 <input type="text" id='mytext'>
> >  console.log(mytext.getAttribute('aaa')) // null
> >  console.log(mytext.aaa)  // 333 */
> > 
> > // 只要点语法或者中括号语法能获取到不是undefined的值的时候 用setAttribute改变这样属性值也改变
> > 
> > // setAttribute改变的值或者添加的属性会反映到页面上。而属性操作的方式不会反映到页面上
> >  
> > // 它们两个的操作除了这个互不影响，所以最好如果用属性值添加时就用属性值获取，setAttribute的方式就用get。。来获取
> > </script>
> >  
> > ```
> >
> > 复选框
> >
> > > ```html
> > > <input type="checkbox" id="mycheck1" >111
> > > <script>
> > >  var check = document.getElementById('mycheck1')
> > >  /* console.log(check.checked) false
> > >  check.checked = true  // 这样可以更改 */
> > > 
> > >  console.log(check.getAttribute('checked')) // null
> > >  check.setAttribute('checked',false) // 设定什么值都会改变为true
> > > 
> > >  console.log(check.checked)  // 得到的一直是true
> > >  check.checked = false  // 只要这样操作了 setAttrbute就不管用了
> > >  
> > >  check.setAttribute('checked','222') 
> > >  
> > >  // 可能因为这个jquery操作时有了显示的bug
> > > </script> 
> > > ```
> > >
> > > 

## Backbone的事件原型的原理 （简单）

> ```js
> var Cat = function() {
> 
>  }
>  // 都是继承 Backbone.Events 来实现的事件的监听以及绑定(on listenTo off ...)
>  _.extend(Cat.prototype,Backbone.Events)
> 
>  var jerry = new Cat()
>  var tom = new Cat()
>  tom.on('run',function(){
>    console.log('tom run')
>  })
>  jerry.listenTo(tom,'run',function(){
>    console.log('jerry watch tom run')
>  })
> ```
>
> ```js
> var jerry = {}
> var tom = {}
> _.extend(tom,Backbone.Events)
> _.extend(jerry,Backbone.Events)
> 	jerry.listenTo(tom,'run',function(){
>    console.log('jerry watch tom run')
>  })
> ```
>
> 

## Backbone的路由简单学习

> ```js
> <!-- backbone 的 路由的用法 -->
> <script>
>  var appRouter = Backbone.Router.extend({
>    routes: {
>      'page1': 'showPage1',
>      'page2': 'showPage2',
>    },
>    initialize() {
>      $('#page1').hide()
>      $('#page2').show()
>    },
>    showPage1() {
>      $('#page2').hide()
>      $('#page1').show()
>    },
>    showPage2() {
>      $('#page2').show()
>      $('#page1').hide()
>    },
>  })
>  
>  var app = new appRouter()
> 
>  Backbone.history.start()
> 
>  // pushState 导航 
>  // 服务端固定返回某个页面 
>  // a连接导航 会刷新页面
>  // Router.navigate导航 不会刷新页面
> 
>  // hash # 导航
>  // a 连接导航 url 不会刷新页面
>  // Router.navigate导航  url会自动添加 #
> ```
>
> 

