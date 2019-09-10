## *基本用法*

```js
// 函数需要这样命名
function *a() {}
// next方法会走到没有下一个yield的地方
function *go() {
  console.log('开始')
  yield 3
  console.log('走')
  yield 4
  console.log('继续走')
  yield 5
  console.log('还要走')
  yield 6
  console.log('结束')
}

let g = go()  // 开始
console.log(g.next())  // { value: 3, done: false } 
console.log(g.next()) // 走 {value: 4, done: false}
console.log(g.next()) // 继续走 {value: 5, done: false}
console.log(g.next()) // 还要走 {value: 6, done: false}
console.log(g.next()) // 结束 {{ value: undefined, done: true }}
console.log(g.next()) // {{ value: undefined, done: true }}
```

```js
// return 会不执行下边的yield
function *go() {
  console.log('开始')
  yield 3
  console.log('走')
  yield 4
  console.log('继续走')
  return ;
  yield 5
  console.log('还要走')
  yield 6
  console.log('结束')
}

let g = go()
console.log(g.next())
console.log(g.next())
console.log(g.next())
console.log(g.next())
console.log(g.next())

/*
开始
{ value: 3, done: false }
走
{ value: 4, done: false }
继续走
{ value: undefined, done: true }
{ value: undefined, done: true }
{ value: undefined, done: true }
*/
```

```js
// 可以使用for of 来遍历
function *go() {
  // console.log('开始')
  yield 3
  // console.log('走')
  yield 4
  // console.log('继续走')
  yield 5
  // console.log('还要走')
  yield 6
  // console.log('结束')
}

let g = go()

for(let i of g) {
  console.log(i)
}

/*
3
4
5
6
*/
```

```js
// next中可以传递参数
function *go(x,y) {
  let z = yield x+y
  // z = 13
  console.log(z)
  let a = yield z*x
  // a = 12
  console.log(a)
}
let g = go(5,6)
console.log(g.next(11))
console.log(g.next(13))
console.log(g.next(12))

// { value: 11, done: false }
// 13
// { value: 65, done: false }
// 12
// { value: undefined, done: true }
```

## *简单应用*

```js
// generator的一个使用方式
// 分配任务
// 一道流程 a1->b1->a2->b2->a3

function* a() {
  yield 'a'
  console.log('a1')
  yield 'b'  //该b做了
  console.log('a2')
  yield 'b'  //该b做了
  console.log('a3')
}
function* b() {
  yield 'b'
  console.log('b1')
  yield 'a'  //该a做了
  console.log('b2')
  yield 'a'  //该a做了
}

let aGo = a()
let bGo = b()
function run(pramas) {
  let p = pramas.next()
  console.log(1)
  if(p.value=='a') {
    run(aGo)
  }else if(p.value=='b'){
    run(bGo)
  }
}
run(aGo)

// 准备工作
// 只要调用了next就会往下走
let p = aGo.next() 
console.log(p) // { value: 'a', done: false }
let p2 = aGo.next()
console.log(p2) // { value: 'b', done: false }

// 但是这样调用就不行了
let p3 = a().next() // { value: 'a', done: false }
console.log(p3)
let p4 = a().next() // { value: 'a', done: false }
console.log(p3)
```



