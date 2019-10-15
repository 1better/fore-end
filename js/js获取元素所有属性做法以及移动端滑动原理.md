## 获取元素所有属性的兼容性做法

```js
let getStyle = function(dom,style) {
    if(dom.currentStyle) {
       return dom.currentStyle[style]
    }else {
       return window.getComputedStyle(dom)[style]
    }
}
```

## 移动端手势滑动

```js
let bindSwipeEvent = function(dom, leftCallback, rightCallback) {
  let start = 0,distance = 0,isMove = false;
  dom.addEventListener("touchstart", function(e) {
    start = e.touches[0].clientX;
  });
  dom.addEventListener("touchmove", function(e) {
    isMove = true;
    distance = e.touches[0].clientX - start;
  });
  dom.addEventListener("touchend", function(e) {
    console.log(this)
    if (isMove && Math.abs(distance) > 50) {
      if (distance > 0) {
        rightCallback && rightCallback.call(this);
      } else {
        leftCallback && leftCallback.call(this);
      }
    }
    /*重置参数*/
    isMove = false;
    start = 0;
    distance = 0;
  });
};


```



