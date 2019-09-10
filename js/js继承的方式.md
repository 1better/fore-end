## *构造函数继承*

> ```js
> function Person(name,age) {
>   this.name = name
>   this.age = age
> }
> 
> Person.prototype.showName = function() {
>   return this.name
> }
> 
> function Man(name,age) {
>   	Person.apply(this,[name,age])
> }
> 
> // man.showName is not a function 构造函数不能继承原型上的方法
> console.log(man.showName())
> ```
>
> + 利用call,apply或者bind改变this的指向
> + 不能继承原型链的方法
> + 可以传递参数（这样在Man中的属性有name和age） 

## *原型链继承*

> ```js
> Man.prototype = new Person()
> ```
>
> + 可以用父person的原型上的方法
> + 属性挂载到了原型上，实例有一个更改都会更改

## *组合继承*

> ```js
> //  构造函数继承
> function Person(name,age) {
>   this.name = name
>   this.age = age
> }
> 
> Person.prototype.showName = function() {
>   return this.name
> }
> 
> function Man(name,age) {
>   Person.apply(this,[name,age])
> }
> // 原型链继承
> Man.prototype = new Person()
> 
> var man = new Man('zs',18)
> console.log(man.name)
> console.log(man.age)
> // man.showName is not a function 构造函数不能继承原型上的方法
> console.log(man.showName())
> 
> ```
>
> + 原型链继承和构造函数继承的结合
> + 缺点是属性创建了两次，一次在apply的时候实例上有属性，后边原型链继承上又有了一次挂载到原型上

## *寄生组合继承*

> ```js
> function Person(name,age) {
>   this.name = name
>   this.age = age
> }
> 
> Person.prototype.showName = function() {
>   return this.name
> }
> 
> function Man(name,age) {
>   Person.apply(this,[name,age])
> }
> // 原型链继承
> Man.prototype = Object.create(Person.prototype)
> Man.prototype.constructor = Man
> 
> var man = new Man('zs',18)
> console.log(man.name)
> console.log(man.age)
> // man.showName is not a function 构造函数不能继承原型上的方法
> console.log(man.showName())
> 
> 
> ```
>
> + 原型链继承上调用一个Object.create方法，这样只继承原型上的方法，没有继承属性 

## *延伸*

### 为什么Man.prototype = Person.prototype这样不可以

> 还是和栈和堆有关系，原型对象这样相等就指向了同一个堆地址，修改Man.prototype时Person.prototype也改变