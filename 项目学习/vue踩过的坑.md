## 坑坑更健康

1. transition包裹的元素必须包含width:100%;

2. vue操作样式复习

   #### vue中的样式添加Class

   > 1. 直接添加 class=“active thin”
   > 2. 添加属性方式（v-bind ）:class="['active','thin']"(注意数组里边也要都加冒号)
   > 3. 三元表达式 :class="['active','thin'，flag？'active’:' ']", flag是data里边的key，可以不加冒号
   > 4. 对象的方式  :class="['active',{'active':flag}]"

   #### vue中的样式添加 style

   > 1. h1 :style="styleObj1"
   > 2. h1 :style="[ styleObj1, styleObj2 ]"

3. jquery操作(jquery控件引入到vue里)

   ```js
   // jquery可以这样操作
   let s = `<div class="up"><input type="file"  id="up${id}"/></div>`
   // 这一步已经在页面上添加了这个dom了
   $("#main").append(s)
   // 这样就可以操作s中的样式了
   $('.up).css('color','red')
   // jquery添加修改的样式 vue不能使用update生命周期或者watch或者computed监听到。不在vue的框架中，是jquery生成的，无法对其进行监听
   ```

4. 



