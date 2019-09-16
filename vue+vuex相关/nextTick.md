## *ref的时机与vue this.$nextTick的用法*

> ```js
> //ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此不应该试图用它在模板中做数据绑定。
> 
> // 当ref和v-for使用的时候，获取的引用是一个数组，包含和循环数据源对应的子组件实例
> ```
>
> 

> ```js
> // this.$nextTick的用法
> // 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。
> 
> // Vue.nextTick用于延迟执行一段代码，它接受2个参数（回调函数和执行回调函数的上下文环境），如果没有提供回调函数，那么将返回promise对象。
> ```
>
> 

> ```js
> // $refs 配合 v-for 使用
> <div id="app">
>  <button @click="add">添加</button>
>  <div  v-for="item in list" :key="item.id" :ref="'item'+item.id">
>    {{item.id}}
>  </div>
> </div>
> <script>
>  var vm = new Vue({
>    el: '#app',
>    data: {
>      list:[
>        {id:1},
>        {id:2}
>      ]
>    },
>    methods: {
>      add() {
>        this.list.push({id:3})
>        console.log(this.$refs.item3) // undefined
>        let self = this
>        this.$nextTick(function(){
>          console.log(self.$refs.item3[0]) // div
>          self.$refs.item3[0].style.color = 'red'
>        })
>      }
>    }
>  })
> </script>
> ```
>
> 

## *nextTick模拟*

> ```js
> var callback = []
>  function A() {
>    console.log(1)
>  }
>  function B() {
>    console.log(2)
>  }
>  function C() {
>    console.log(3)
>  }
>  callback.push(A)
>  callback.push(B)
>  callback.push(C)
>  function nextTick(cb) {
>    // 模拟 渲染函数
>    callback.forEach(item=>item())
>    cb()
>  }
>  nextTick(function(){
>    console.log('渲染完了我再执行')
>  })
> 
> // 1 2 3 渲染完了我再执行
> ```
>
> 

## *nextTick执行流程*

> ```js
> // nextTick() -> queueNextTick() -> timeFunc() -> nextTickHandler() -> callback()
> 
> // timeFunc()是关键，该函数起到延迟执行的作用
> 
> ```

## *timeFunc实现方式*

> 1. promise
> 2. MutationObserver
> 3. setTimeout
>
> ### MutationObserver 
>
> > 是HTML5中的新API，是个用来监视DOM变动的接口。他能监听一个DOM对象上发生的子节点删除、属性修改、文本内容修改等等。
> >
> > mdn MutationObserver 介绍  https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver





