##*flex* 

## 容器的六个属性

> - flex-direction
> - flex-wrap
> - flex-flow
> - justify-content
> - align-items
> - align-content

### flex-direction

> 决定主轴的方向
>
> ```css
> .box {
>   flex-direction: row | row-reverse | column | column-reverse;
> }
> ```
>
> - row（默认值）：水平方向左端。
> - row-reverse ：水平方向右端。
> - column：垂直方向上沿。
> - column-reverse：垂直方向下沿。

### flex-wrap

> 是否换行
>
> ```css
> .box {
>   flex-wrap: nowrap | wrap | wrap-reverse;
> }
> ```
>
> + nowrap (默认值) 不换行
> + wrap 换行
> + wrap-reverse 换行反向(换行的在上边)

### flex-flow

> flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap
>
> ```css
> .box {
>   flex-flow: <flex-direction> || <flex-wrap>;
> }
> ```
>
>  

### justify-content

> 定义了项目在主轴上的对齐方式
>
> ```css
> .box {
>   justify-content: flex-start | flex-end | center | space-between | space-around;
> }
> ```
>
> - `flex-start`（默认值）：左对齐
> - `flex-end`：右对齐
> - `center`： 居中
> - `space-between`：两端对齐，项目之间的间隔都相等。
> - `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

### align-items

> 定义项目在交叉轴上如何对齐
>
> ```css
> .box {
>   align-items: flex-start | flex-end | center | baseline | stretch;
> }
> ```
>
> - `flex-start`：交叉轴的起点对齐。
> - `flex-end`：交叉轴的终点对齐。
> - `center`：交叉轴的中点对齐。
> - `baseline`: 项目的第一行文字的基线对齐。
> - `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

### align-content

> 定义了多根轴线的对齐方式
>
> ```css
> .box {
>   align-content: flex-start | flex-end | center | space-between | space-around | stretch;
> }
> ```
>
> - `flex-start`：与交叉轴的起点对齐。
> - `flex-end`：与交叉轴的终点对齐。
> - `center`：与交叉轴的中点对齐。
> - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
> - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
> - `stretch`（默认值)：轴线占满整个交叉轴。

## 项目的六个属性

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

### order

> 定义项目的排列顺序
>
> ```css
> .item {
>   order: <integer>;
> }
> ```

###  flex-grow

> 定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大
>
> ```css
> .item {
>   flex-grow: <number>; /* default 0 */
> }
> ```

### flex-shrink

> 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
>
> ```css
> .item {
>   flex-shrink: <number>; /* default 1 */
> }
> ```

### flex-basis

> 定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小
>
> ```css
> .item {
>   flex-basis: <length> | auto; /* default auto */
> }
> ```

### flex属性

> 是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选
>
> ```css
> .item {
>   flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
> }
> ```

### align-self

> 允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`
>
> ```css
> .item {
>   align-self: auto | flex-start | flex-end | center | baseline | stretch;
> }
> ```
>
> 

## ([以上摘抄自阮一峰老师](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html))