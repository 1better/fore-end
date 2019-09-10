## call,bind以及apply

## *call与apply的实现*

> ```js
> Function.prototype.myCall = function(self,...value) {
>   self = self || window
>   self.fn = this
>   console.log(...value) // 1 ,3
>   var result = self.fn(...value)
>   delete self.fn
>   return result
> }
> 
> Function.prototype.myApply = function(self,arr) {
>   self = self || window
>   self.fn = this
>   console.log(...arr) // 1，3
>   var result = self.fn(...arr)
>   delete self.fn
>   return result
> }
> 
> var f = function() {
>   // return [].slice.myCall(arguments,1,3)
>   return [].slice.myApply(arguments,[1,3])
> }
> 
> console.log(f(1,3,4,5,6,7))
> ```
>
>  

## *bind的简易实现*

> ```js
> Function.prototype.myBind = function(self,...arg) {
>     self = self || window
>     self.fn = this
>     return function(...arg2) {
>       arg = arg.concat(arg2)
>       var result = self.fn(...arg)
>       return result
>     }
> }
> 
> var f = function() {
>   return [].slice.myBind(arguments,1)
> }
> console.log(f(1,3,4,5,6,7)(3))
> ```
>
>  

## *mdn bind的实现*

> ```js
> if (!Function.prototype.bind) {
>   Function.prototype.bind = function(oThis) {
>     if (typeof this !== 'function') {
>       // closest thing possible to the ECMAScript 5
>       // internal IsCallable function
>       throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
>     }
> 
>     var aArgs   = Array.prototype.slice.call(arguments, 1),
>         fToBind = this,
>         fNOP    = function() {},
>         fBound  = function() {
>           // this instanceof fBound === true时,说明返回的fBound被当做new的构造函数调用
>           // 这里的判断是当当作new的构造函数使用的时候返回this
>           return fToBind.apply(this instanceof fBound
>                  ? this
>                  : oThis,
>                  // 获取调用时(fBound)的传参.bind 返回的函数入参往往是这么传递的
>                  aArgs.concat(Array.prototype.slice.call(arguments)));
>         };
> 
>     // 维护原型关系
>     if (this.prototype) {
>       // Function.prototype doesn't have a prototype property
>       fNOP.prototype = this.prototype; 
>     }
>     // 下行的代码使fBound.prototype是fNOP的实例,因此
>     // 返回的fBound若作为new的构造函数,new生成的新对象作为this传入fBound,新对象的__proto__就是fNOP的实例
>     fBound.prototype = new fNOP();
> 
>     return fBound;
>   };
> }
> ```
>
>  

## *new的本质*

> ```js
> // 实现
> function _new(fun) {
>   var obj = {}
>   obj.__proto__ = fun.prototype
>   fun.call(obj)
>   return obj
> }
> ```
>
> #### *使用call的时候执行如下操作(apply也类似)*
>
> > ```js
> > var obj = {}
> > obj.F = function() {
> >   this.name = 'zs'
> >   this.age = 18
> > }
> > obj.F()
> > delete obj.F
> > console.log(obj)
> > 
> > //{ name: 'zs', age: 18 }
> > ```
> >
> > 
>
> #### *作用*
>
> > 1. 新生成了一个对象
> > 2. 新对象隐式原型链接到函数原型
> > 3. 调用函数绑定this
> > 4. 返回新对象