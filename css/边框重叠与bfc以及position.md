## *边框重叠与bfc以及position*

## *边框重叠*

### *例子*

> 1. 当父元素没有设置margin-top，而子元素设置了margin-top;父元素也一起有了边距
> 2. 两个div，上边设置margin-bottom 下边设置margin-top 它们之间会取最大的值
>
> 

## *bfc(边框重叠解决方案)*

### *bfc的原理*

> 1. 内部的box会在垂直方向，一个接一个的放置
> 2. 每个元素的margin box的左边，与包含块border box的左边相接触（对于从左往右的格式化，否则相反）
> 3. **box垂直方向的距离由margin决定，属于同一个bfc的两个相邻box的margin会发生重叠**
> 4. **bfc的区域不会与浮动区域的box重叠**
> 5. bfc是一个页面上的独立的容器，外面的元素不会影响bfc里的元素，反过来，里面的也不会影响外面的
> 6. 计算bfc高度的时候，浮动元素也会参与计算

### *如何创建bfc*

> 1. float属性不为none（脱离文档流）
> 2. position为absolute或fixed
> 3. display为inline-block,table-cell,table-caption,flex,inine-flex
> 4. overflow不为visible
> 5. 根元素 html

### *应用场景*

> 1. 自适应两栏布局
> 2. 清除内部浮动 
> 3. 防止垂直margin重叠

## *position的几个属性*

> + absolute  绝对定位
> + relative    相对定位 仍然占位置
> + static  默认值 设置left right等不起作用
> + Inherit 规定应该从父元素继承 position 属性的值
> + fixed  相对浏览器窗口进行定位
> + sticky 在屏幕范围（viewport）时该元素 是postion:relative的定位，当该元素的位置将要移出偏移范围时，定位又会变成fixed，根据设置的left、top等属性成固定位置的效果
>
> #### sticky的特性
>
> > 1.sticky不会触发BFC，
> > 2.z-index无效，
> > 3.当父元素的height：100%时，页面滑动到一定高度之后sticky属性会失效。
> > 4.父元素不能有overflow:hidden或者overflow:auto属性。
> > 5.父元素高度不能低于sticky高度，必须指定top、bottom、left、right4个值之一 

### *延伸 position定位垂直居中的原理*

> ```css
> /*垂直居中*/
> div {
>   position: absolute;
>   left: 0;
>   right: 0;
>   top: 0;
>   bottom: 0;
>   margin: auto;
> }
> ```
>
> margin: auto 是根据left与right 相距的距离来自适应居中的(top bottom类似)

## position和float之间会相互影响吗

1. 如果display为none，这时候不产生盒子，所以会忽略position和float
2. 如果position的值为absolute或fixed，float会被转换为none
3. 如果position不是绝对布局，这时，如果float不为none或者该元素是该元素。
4. 如果position不是绝对布局，并且float为none不浮动，且不是跟元素，display就取设置值