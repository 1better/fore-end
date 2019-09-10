## Vue的生命周期

+ beforeCreate

  实例还没有创建，data和methods中的数据还没有被初始化

+ created

  data和methods的数据已经被初始化了，调用methods中的方法或使用data中的数据最早只能在created中执行

+ beforeMount

  模板已经在内存中编辑好了，但是没有挂载到页面上

+ mounted

  模板已经挂载到页面上了

+ beforeUpdate

  模板数据有更新的时候，此时这个生命周期中页面显示的数据还没有更新，是之前的数据

+ updated

  页面显示的数据已经更新了

+ beforeDestory

  模板执行销毁操作之前还没有被销毁

+ destoryed

  模板销毁之后

## mixins

> ```js
> 			// 模态框
>    const Modal = {
>      template: '#modal',
>      data() {
>        return {
>          isShowing: false
>        }
>      },
>      methods: {
>        toggleShow() {
>          this.isShowing = !this.isShowing;
>        }
>      },
>      components: {
>        appChild: Child
>      }
>    }
> 
>    		// 提示框
>    const Tooltip = {
>      template: '#tooltip',
>      data() {
>        return {
>          isShowing: false
>        }
>      },
>      methods: {
>        toggleShow() {
>          this.isShowing = !this.isShowing;
>        }
>      },
>      components: {
>        appChild: Child
>      }
>    }
> 
> 		  // 两个组件基本逻辑一样 如上边 都有 isShowing toggleShow 可以合成一个mixins来用
>    
>     const toggle = {
>       data() {
>         return {
>           isShowing: false
>         }
>       },
>       methods: {
>         toggleShow() {
>           this.isShowing = !this.isShowing;
>         }
>       }
>     }
> 
>     const Modal = {
>       template: '#modal',
>       mixins: [toggle],
>       components: {
>         appChild: Child
>       }
>     };
> 
>     const Tooltip = {
>       template: '#tooltip',
>       mixins: [toggle],
>       components: {
>         appChild: Child
>       }
>     };
> ```
>
> 

## vue路由

> ```TypeError: Cannot read property '_c' of undefined```
>
> 原因
>
> ```js
> {
>    components: info
>  } 
> // 因为是单个路由。所以components不正确 应该去掉s
> ```
>
> 

## 延伸      

## 1.  Vue的路由导航守卫

> > ```js
> > router.beforeEach((to, from, next) => {
> > 	// ...
> > })
> > 
> > 	// to: Route: 即将要进入的目标 路由对象
> > 
> > 	// from: Route: 当前导航正要离开的路由
> > 
> > 	// next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
> > 
> > 	// next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
> > 
> > 	// next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
> > 
> > 	// next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项。
> > 
> > 	// next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。
> > ```
> >
> >   

## Computed和watch区别

>  Computed是被动依赖，依赖数据的改变而发生改变，当不改变时它不会改变（data中改变值它会随之改变），并且它改变一次，引用到它的多个地方都只改变一次
>
> watch是主动监听，主动监视数据的变化，只要有变化它就会随着变化

## 为什么computed的名字不可以和data里边名字重名

> 都是挂载到vm实例中，是不能一样的

## beforeEach和afterEach

> 路由跳转，一个是在跳转之前，一个是之后，只要有路由跳转它就会触发，可以做一些逻辑的处理 （导航守卫）
>
> > ```js
> > router.beforeEach(to,from,next)  // to 到哪个路由 from 从哪个路由跳转
> > router.afterEach(to,from,next)
> > ```

## Vue检测数组的变化

> ```js
> //当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
> //当你修改数组的长度时，例如：vm.items.length = newLength
> 
> var vm = new Vue({
>   data: {
>    	items: ['a', 'b', 'c']
>   }
> })
> vm.items[1] = 'x' // 不是响应性的
> vm.items.length = 2 // 不是响应性的
> ```
>
> ```js
> // Vue.set
> Vue.set(vm.items, indexOfItem, newValue)
> // Array.prototype.splice
> vm.items.splice(indexOfItem, 1, newValue)
> ```
>
>  

## Vue的表单

> 1. 在文本区域插值 (`<textarea>{{text}}</textarea>`) 并不会生效，应用 `v-model` 来代替
> 2. 多个复选框绑定一个数组
> 3. ```如果 `v-model` 表达式的初始值未能匹配任何选项，`<select>` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。```

## 条件渲染的key

> ``` Vue 提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 `key` 属性即可```
>
> ```html
> <template v-if="loginType === 'username'">
> <label>Username</label>
> <input placeholder="Enter your username" key="username-input">
> </template>
> <template v-else>
> <label>Email</label>
> <input placeholder="Enter your email address" key="email-input">
> </template>
> <!--
> 	添加了key之后每次渲染 输入框每次都会重新渲染
> -->
> ```
>
> 

## 引入script

> ### 为什么不能直接在vue文件中引入数据
>
> > 这是.vue文件啊。是模版，怎么可能会有script src这样的 操作呢，我去，你动动脑子好不好！
>
> ### Vue引入的方式
>
> > 原生js添加script的方式(非vue)
> >
> > ```js
> > const oScript = document.createElement('script');
> > oScript.type = 'text/javascript';
> > oScript.src = '//g.alicdn.com/sd/smartCaptcha/0.0.1/index.js';
> > document.body.appendChild(oScript);
> > ```
> >
> > 1. 可以在vue的created或mounted生命周期内加入上述方式
> > 2. 使用creatElement方法
> >
> > ```js
> > // 利用一个组件
> > components: {
> >         'scriptLink': {
> >           // 利用render方法
> >           render(createElement) {
> >             return createElement(
> >               'script',
> >               {
> >                 attrs: {
> >                   type: 'text/javascript',
> >                   src: 'https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js',
> >                 },
> >               },
> >             )
> >           }
> >         }
> >       }
> > ```
> >
> > 3. 封装成一个组件
> >
> > ```js
> > <template>
> >     <remote-js src="//g.alicdn.com/sd/smartCaptcha/0.0.1/index.js"></remote-js>
> > </template>
> > <script>
> > export default {
> >     components: {
> >     'remote-js': {
> >       render(createElement) {
> >         return createElement('script', {attrs: {type: 'text/javascript', src: this.src}});
> >       },
> >       props: {
> >         src: { type: String, required: true}
> >       }
> >     }
> >   }
> > }
> > </script>
> > ```
> >
> >  

## slot插槽

> slot插槽 
>
> > 作用。可以将父组件的内容传递给子组件，让子组件根据不同的场景显示不同的内容
>
> 基本用法
>
> > ```html
> > <div id="app">
> >  <Child>
> >    {{msg}}
> >  </Child>
> > </div>
> > <template id="Child">
> >  <div>
> >    <slot>
> >      父组件未插入内容
> >    </slot>
> >    <br>
> >    {{msg}}
> >  </div>
> > </template>
> > <script>
> >  var vm = new Vue({
> >      el: '#app',
> >      data: {
> >        msg: '我是父组件通过slot插入的内容'
> >      },
> >      components: {
> >        Child:{
> >          template:'#Child',
> >          data() {
> >            return {
> >              msg: 111
> >            }   
> >          }
> >        }
> >      }
> >  })
> > 
> > </script>
> > 
> > <!--
> > 	通过slot就可以接受父组件传递过来的值
> > -->
> > ```
>
> 具名插槽
>
> > ```html
> > <div id="app">
> >  <Child >
> >    <h1 slot="header">标题</h1>
> >    <h2 slot="footer">底部</h2>
> >  </Child>
> > </div>
> > <template id="Child">
> >  <div>
> >    <div class="header">
> >      <!-- 这样333也不显示 -->
> >        <slot name="header">333</slot>
> >    </div>
> >    <div class="footer">
> >        <slot name="footer"></slot>
> >    </div>
> >  </div>
> > </template>
> > <script>
> >  var vm = new Vue({
> >    el: '#app',
> >    data: {
> >      msg: '我是父组件通过slot插入的内容'
> >    },
> >    components: {
> >      Child:{
> >        template:'#Child',
> >        data() {
> >          return {
> >            msg: 111
> >          }   
> >        }
> >      }
> >    }
> >  })
> > </script>
> > ```
> >
> > ```html
> > <!-- 页面显示 -->
> > <div id="app">
> >  <div>
> >    <div class="header">
> >      <h1>标题</h1>
> >    </div>
> >  </div>
> > </div>
> > ```
> >
> > 
>
> 作用域插槽
>
> > ```html
> > <!--子组件-->
> > <template>
> >   <div>
> >     我是作用域插槽的子组件
> >     <slot :data="user"></slot>
> >   </div>
> > </template>
> > 
> > <script>
> > export default {
> >   name: 'slotthree',
> >   data () {
> >     return {
> >       user: [
> >         {name: 'Jack', sex: 'boy'},
> >         {name: 'Jone', sex: 'girl'},
> >         {name: 'Tom', sex: 'boy'}
> >       ]
> > 		}
> > 	}
> > }
> > </script>
> > 
> > <!--父组件-->
> > <template>
> >   <div>
> >     我是作用域插槽
> >   	<slot-three>
> >   	<template slot-scope="user">
> >   	<div v-for="item in user.data" :key="item.id">
> >   	{{item}}
> >   </div>
> > </template>
> > </slot-three>
> > </div>
> > </template>
> > ```
> >
> > 在父组件上使用slot-scope属性，user.data就是子组件传过来的值

