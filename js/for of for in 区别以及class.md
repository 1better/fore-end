## *for in 与 for of 区别*

+ for..of适用遍历数/数组对象/字符串/map/set等拥有迭代器对象的集合.但是不能遍历对象,因为没有迭代器对象.与forEach()不同的是，它可以正确响应break、continue和return语句

+ for-of循环不支持普通对象，如果想迭代一个对象的属性，可以用for-in循环（这也是它的本职工作）或内建的Object.keys()方法

  ### *for in 遍历数组的缺点*

+ index索引为字符串型数字，不能直接进行几何运算

+ 遍历顺序有可能不是按照实际数组的内部顺序

+ 使用for in会遍历数组所有的可枚举属性，包括原型。例如原型方法中的method和name属性
  所以for in更适合遍历对象，不要使用for in遍历数组。 

## *class*

```js
// class是语法糖
class Person {
  constructor() {} 
  toString() { }
  toValue() { }
}
// 等同于
Person.prototype = {
	constructor() {},
	toString() {},
	toValue() {},
};

// 实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上)
class Person {
  constructor(x,y) {
    this.x = x
    this.y = y
  }

}
let p = new Person(1,2)
console.log(p.x,p.y)

// 可以使用表达式
let methodName = "getTitle";
class Person {
  [methodName]() {
    return 'title'
  }
}

let p = new Person()
console.log(p.getTitle())

// 立即执行 class
let person = new class{
  constructor(x) {
    this.x = x
  }
}('go')
console.log(person.x) // go

// 类的方法内部如果含有this，它默认指向类的实例 (类似于react的绑定this)
class Person {
  constructor() {
    console.log(this) // Person
    // 没有下边这句话会报错
    //this.getName = this.getName.bind(this) // 已经将this绑定到上边了，执行环境如何改变都可以
  }
  getName() {
    console.log(this) 
    console.log(this.name())
  } 
  name() {
    return '测试name'
  }
}
let p = new Person()
let {getName,name} = p
// 
getName() // undefined Cannot read property 'name' of undefined 报错


// static关键字
class Person {
  static getName() {
    console.log('静态方法得到的name')
  }
}
let p = new Person()
// 需要Person.getName() 调用
p.getName() // p.getName is not a function
Person.getName() // 静态方法得到的name

// 子类也可以使用
class Man extends Person {

}
Man.getName() // 静态方法得到的name
```

## *再次学习(通过类再看react的bind(this))*

### 为什么react需要绑定this (类似class这样)

> 简单来说，就是react在调用render方法的时候，会先把render里的方法赋值给一个变量（比如变量foo），然后再执行foo()。
>
> 例子
>
> > 具体来说，以典型的绑定点击事件为例
> >
> > ```html
> > <div onClick={this.clickHandler}></div>
> > ```
> >
> > react构建虚拟DOM的时候，会把this.clickHandler先赋值给一个变量。我们假设是变量clickFunc = this.clickHandler;
> > 然后，把虚拟DOM渲染成真实DOM的时候，会把onClick属性值替换成onclick，并给onclick赋值clickFunc
> >
> > 在复杂的情况中，可能存在多次传递，如果不进行bind，那么this的指向是一定会丢失的。

### *为什么react不自己集成bind到生命周期里？*

> 1. 没有特别合适集成bind的地方
> 2. 并不是所有的函数都需要bind
> 3. 随意集成bind可能导致效率低下





