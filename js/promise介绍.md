## *promise简单介绍*

> ####  三种状态
>
> > + *pending* 既不是成功，也不是失败状态
> > + *fulfilled*  意味着操作成功完成
> > + *rejected* 意味着操作失败
> >
> > 状态不可逆，pending可以变成fullfilled或者rejected，但是不能由后边的状态变成前边的状态 
>
> #### 使用
>
> ```js
> let p = new Promise((reslove,reject)=>{
>   setTimeout(
>     ()=>{reslove(1)},5000)
>   })
> 
> p.then(data=>console.log(data))
> 
> // new 一个 Promise对象 reslove或者reject数据
> // then 方法 第一个参数打印成功的数据，第二个参数打印失败的数据
> ```
> 
>#### 方法
> 
>> catch 添加一个拒绝(rejection) 回调到当前 promise, 返回一个新的promise
> >
> > Promise.all方法  接受一个promise对象数组，这些数组的数据都执行完返回一个数组(包含的是成功的数据)
> >
> > Promise.race 接受一个promise对象数组，返回执行最快的数组对象的数据 

## *promise实现*

### *class实现*

```js
class MyPromise {
  constructor(fn) {
    this.status = 'pending'
    this.successArr = []
    this.failArr = []
    this.data = ''
    fn(this.reslove.bind(this),this.reject.bind(this))
  }
  reslove(value) {
    this.status = 'fullfilled'
    this.data = value
    this.successArr.map(fn=>fn(value))
  }
  reject(value) {
    this.status = 'rejected'
    this.data = value
    this.failArr.map(fn=>fn(value))
  }
  all(arr) {

  }
  then(successCallback,failCallback) {
    successCallback = (typeof successCallback == 'function' ) ? successCallback : v => v
    failCallback = (typeof failCallback == 'function' ) ? failCallback : v => v

    if(this.status == 'pending') {
      this.successArr.push(successCallback)
      this.failArr.push(successCallback)
    }else if(this.status == 'fullfilled') {
      successCallback(this.data)
    }else {
      failCallback(this.data)
    }
  }
}
```

### *promise.all简易版*

```js
MyPromise.all = arr => {
  let result = []
  return new MyPromise((reslove,reject)=>{
    let i = 0
    next()
    function next() {
      // 判断arr[i]是不是Promise实例
      if(arr[i] instanceof MyPromise) {
        arr[i].then(data=>result.push(data))
      }else {
        result.push(arr[i])
      }
      i++
      if(i==arr.length) {
        reslove(result)
      }else {
        next()
      }
    }
  })
}

// 未考虑异步的状态
```

### *promise.all复杂*

```js
MyPromise.all = arr => {
  let result = []
  return new MyPromise((reslove,reject)=>{
    let times = 0
    for(let i=0;i<arr.length;i++) {
      if(arr[i] instanceof MyPromise) {
        arr[i].then(data=>{next(i,data)})
      }else {
        next(i,arr[i])
      }
    }
    function next(index,value) {
      result[index] = value
      times++
      if(times == arr.length) {
        reslove(result)
      }
    }
  })
}
// 考虑异步
```

### *promise.race* 

```js
MyPromise.race = (arr) =>  {
  let result = []
  let raceTime = 0
  return new MyPromise(reslove=>{
    arr.forEach( (p,i) => {
      if(p instanceof MyPromise) {
        p.then(data=>next(i,data))
      }else {
        next(i,p)
      }
    })
    function next(i,data) {
    	result[i] = data
      if(raceTime==0) {
        for(let i=0;i<result.length;i++) {
          if(result[i]) {
            reslove(result[i])
            raceTime ++
            break
          }
        }
      } 
  	}
  })
}
```



