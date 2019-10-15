## addEventListener的第三个参数以及执行顺序

```js
// addEventListener函数的第三个参数设置为false说明不为捕获事件，即为冒泡事件
// 无论是冒泡事件还是捕获事件，元素都会先执行捕获阶段
// 所有事件的顺序是：其他元素捕获阶段事件 -> 本元素代码顺序事件 -> 其他元素冒泡阶段事件 
// 上述就是事件流 事件捕获 -> 元素本身 -> 事件冒泡
```

## addEventListener的参数里一次绑定多个事件

```js
window.addEventListener('pushState', function () {
    fun();
});
window.addEventListener('popstate', function () {
    fun();
});
window.addEventListener('replaceState', function () {
    fun();
});
```

变通的方法 （变成一个数组，使用forEach来循环绑定）

```js
['pushState','popstate','replaceState'].forEach(function(item,index){
     window.addEventListener(item, fun);
})
```

## Array.sort实现

```js
//  数组长度小于等于 22 的用插入排序，其它的用快速排序
```







