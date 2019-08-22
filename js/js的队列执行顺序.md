## js任务队列的执行顺序

> **整个的js代码macrotask先执行，同步代码执行完后有microtask执行microtask，没有microtask执行下一个macrotask，如此往复循环至结束**
>
> ```js
> //网上的一段执行顺序代码
> var setTimeout1 = setTimeout(function () {
>   console.log('---1---')
> }, 0) 
> 
> var setTimeout2 = setTimeout(function () {
>   Promise.resolve().then(() => {
>     console.log('---2---')
>   })
>   console.log('---3---')
> }, 0)   // 3 2 
> 
> new Promise(function (resolve) {
>   console.time("Promise")
>   for (var i = 0; i < 1000000; i++) {
>     i === (1000000 - 1) && resolve()
>   }
>   console.timeEnd("Promise")
> 
> }).then(function () {
>   console.log('---4---')
> });
> 
> console.log('---5---')
> 
> //执行顺序为 Promise内的执行时间 5 4 1 3 2
> ```
>
> > 过程分析
> >
> > 1. 异步代码加入任务队列 同步代码执行完 打印Promise内的执行时间 和 5
> > 2. 异步队列 微任务 console.log(4) 宏任务 set1 set2
> > 3. 执行完微任务 打印4 再按照顺序执行宏任务 set1 set2 set1执行完打印1
> > 4. set2里边有同步代码 console.log(3) 微任务(console.log(2))
> > 5. 先打印3 再打印 2
>
> 小结
>
> > - console.time 和 console.timeEnd 可以看到执行时间
> > - 一个事件循环(event loop)会有一个或多个任务队列(task queue)
> > - 每一个 event loop 都有一个 microtask queue
> > - task queue == macrotask queue != microtask queue
> > - 一个任务 task 可以放入 macrotask queue 也可以放入 microtask queue 中
> > - 调用栈清空(只剩全局)，然后执行所有的microtask。当所有可执行的microtask执行完毕之后。循环再次从macrotask开始，找到其中一个任务队列执行完毕，然后再执行所有的microtask，这样一直循环下去
> >
> > > microtasks（微任务）:
> > >
> > > > - process.nextTick
> > > > - promise
> > > > - Object.observe
> > > > - MutationObserver
> > >
> > > mascrotask（宏任务）：
> > >
> > > > - setTimeout：
> > > > - setInterval
> > > > - setImmediate
> > > > - I/O
> > > > - UI渲染



