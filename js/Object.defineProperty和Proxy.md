## *Object.defineProperty和Proxy*

## *Object.defineProperty*

vue用Object.defineProperty来实现数据的监听

```js
Object.keys(obj).forEach(key => {
  Object.defineProperty(obj, key, {
    // ...
  })
})

//	当一个对象为深层嵌套的时候,必须进行逐层遍历，直到把每个对象的每个属性都调用 Object.defineProperty() 为止。 
```

### 存在的缺点

> - 不能监听数组的变化
> - 必须遍历对象的每个属性
> - 必须深层遍历嵌套的对象

## *Proxy*

```js
let obj = {
  name: 'aaa',
  age: 30
}
let handler = {
  get (target, key, receiver) {
    console.log('get', key)
    return Reflect.get(target, key, receiver)
  },
  set (target, key, value, receiver) {
    console.log('set', key, value)
    return Reflect.set(target, key, value, receiver)
  }
}
let proxy = new Proxy(obj, handler)
proxy.name = 'bbb' // set name bbb
console.log(proxy.age) // get age      30
```

## 优点

> - 针对对象:针对整个对象,而不是对象的某个属性
> - 支持数组:不需要对数组的方法进行重载，省去了众多 hack
> - 嵌套支持: get 里面递归调用 Proxy 并返回