## *盒模型以及js如何获取宽高*

## *盒模型*

### *分类*

> 1. 标准模型 : 盒子的宽高为内容的宽高
> 2. ie模型： 盒子的宽高为 border+padding+content

### *设置*

> ```css
>  /*标准盒模型*/
> box-sizing: content-box
>   /*ie盒模型*/
> box-sizing: border-box
> ```
>
>  

## *js如何获取宽高*

> 1. dom.style.width/height.  只能获取内联样式的宽高
> 2. dom.currentStyle.width/height  哪一种方式都可以获取，只支持ie
> 3. Window.getComputedStyle(dom).width/height  和2类似， 兼容多浏览器
> 4. dom.getBoundingClientRect().width 根据元素的绝对位置来获取
> 5. dom.offsetWidth/offsetHeight  兼容性最好

## *获取元素所有属性的兼容性做法*

```js
let getStyle = function(dom,style) {
    if(dom.currentStyle) {
       return dom.currentStyle[style]
    }else {
       return window.getComputedStyle(dom)[style]
    }
}
```

