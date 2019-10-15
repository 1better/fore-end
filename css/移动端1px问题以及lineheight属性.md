## *移动端1px的问题以及lineheight以及em rem问题*

### 为什么会有这个问题

> ```html
> 	<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
> 	这句话定义了本页面的 viewport 的宽度为设备宽度,初始缩放值和最大缩放值都为1,并禁止了用户缩放. viewport 通俗的讲是浏览器上可用来显示页面的区域, 这个区域是可能比屏幕大的。
> ```
>
> #### devicePixelRatio的概念
>
> ​		在window对象中有一个devicePixelRatio属性，他可以反应css中的像素与设备的像素比。然而1px在不同的移动设备上都等于这个移动设备的1px，这是因为不同的移动设备有不同的像素密度。有关这个属性，它的官方的定义为：设备物理像素和设备独立像素的比例，也就是
>
> 		devicePixelRatio = 物理像素 / 独立像素 1px变粗的原因：
> ​		viewport的设置和屏幕物理分辨率是按比例而不是相同的. 移动端window对象有个devicePixelRatio属性,它表示设备物理像素和css像素的比例, 在retina屏的iphone手机上, 这个值为2或3,css里写的1px长度映射到物理像素上就有2px或3px那么长

### 如何解决

> 1. 把 viewport 宽度设置为实际的设备物理宽度 (flexible.js)
>
> 2. transform + 伪类
>
>    ```html
>    <!DOCTYPE html>
>    <html>
>      <head>
>        <meta charset="UTF-8" />
>        <title>test</title>
>        <style>
>          .box-shadow-1px {
>            height: 200px;
>            width: 200px;
>            text-align: center;
>          }
>          .scale {
>            position: relative;
>            margin-bottom: 20px;
>            border: none;
>          }
>          .scale:after {
>            content: "";
>            position: absolute;
>            top: 0;
>            left: 0;
>            border: 1px solid #000;
>            -webkit-box-sizing: border-box;
>            box-sizing: border-box;
>            width: 200%;
>            height: 200%;
>            -webkit-transform: scale(0.5);
>            transform: scale(0.5);
>            -webkit-transform-origin: left top;
>            transform-origin: left top;
>          }
>       	</style>
>      </head>
>      <body>
>        <div class="box-shadow-1px scale">box-shadow-1px</div>
>        <script></script>
>      </body>
>    </html>
>    
>    ```
>
>     

## *line-height1.5和150%区别*

- 父元素设置line-height:1.5会直接继承给子元素，子元素根据自己的font-size再去计算子元素自己的line-height。
- 父元素设置line-height:150%是计算好了line-height值，然后把这个计算值给子元素继承，子元素继承拿到的就是最终的值了。此时子元素设置font-size就对其line-height无影响了。 

## *em和rem的区别*

+ em是相对于父元素
+ rem是相对于根元素