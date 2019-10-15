## *vue 的导航守卫 （子组件中的用法）*

> ### beforeRouteEnter 进入该组件之前
>
> ```js
> // 进入该组件之前被调用，组件实例还没有被创建，不能使用 this关键字
> 
> // 可以通过传一个回调给 next来访问组件实例，也就是说可以通过 next 来回调实例化后的组件，用next函数的 vm 参数充当 this
> 
> beforeRouteEnter:(to,from,next)=>{
>        //此时该组件还没被实例化
>  alert(this.infor);       //弹出消息框信息为 undefined
>  next(vm =>{
>    //此时该组件被实例化了
>    alert(vm.infor);         //弹出消息框信息为 111
>  })
> }
> 
> 
> ```
>
> ### beforeRouteLeave 离开组件之后
>
> ```js
> // 离开组件之后调用，此时可以调用 this 关键字
> 
>  export default {
>    name: "aaa",
>    beforeRouteLeave(to,from,next){
>        if(confirm("确定离开吗？") == true){
>          next()   //跳转到另一个路由
>        }else{
>          next(false);//不跳转
>        }
>    }
>  }
> ```
>
> ### **beforeRouteUpdate** 组件更新
>
> ```js
> // 该组件被复用时调用，可以调用 this 关键字
> beforeRouteUpdate (to, from, next) {
>  // 在当前路由改变，但是该组件被复用时调用
>  // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
>  // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
>  // 可以访问组件实例 `this`
> }
> ```
>
> 