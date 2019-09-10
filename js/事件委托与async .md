## *事件委托与async*

## *事件委托*

优点:

> 1. 提高性能:每一个函数都会占用内存空间，只需添加一个事件处理程序代理所有事件,所占用的内存空间更少。
> 2. 动态监听:使用事件委托可以自动绑定动态添加的元素,即新增的节点不需要主动添加也可以一样具有和其他元素一样的事件。
>    例子解析: 

示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>0</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
  </ul>
  <script>
    // 利用事件冒泡  当点击li的时候冒泡至ul中进行统一的处理
    let ul = document.querySelector('ul')
    ul.addEventListener('click',function(e){
      alert(e.target.innerHTML)
    })
  </script>
</body>
</html>
```

## *async简单使用*

```js
let fs = require('fs')

function readFile(file) {
  return new Promise(reslove => {
    fs.readFile(file, (err, data) => {
      reslove(data.toString())
    })
  });
}

async function awaitReadFile(arr) {
  let data
  for(let i of arr) {
    data = await readFile(i)
    console.log(data)
  }
}

awaitReadFile(['./test/a.txt','./test/b.txt'])
```



