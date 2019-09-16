## *栅格系统布局和less和css属性选择器*

##*栅格系统布局的原理*

### 媒体查询

> 写法   @media screen and (max-width:xxxpx) and (min-width:xxxpx)
>
> 简写   @media(max-width:xxx px)
>
> 分类
>
> > > 大于1200px   大屏(width设为1192px 居中)
> > >
> > > 小于1200px大于992px  中屏(width 设为970px，居中)
> > >
> > > 大于768px小于992px    小屏(width 设为750px 居中)
> > >
> > > 小于768px    超小屏  (width设为100%)
>

## *less+媒体查询*

```less
.adapterMinxins(@index) when (@index>0){
  @media(min-width: extract(@adapterList,@index)){
  html {
  font-size = @baseFont / @psDwidth * extract(@adaterList,@index);
  }}
  .adapterMinxin(@index-1) 
}

/*调用*/
 @adapterMinxins(@len)
 @len:length(@adapterList)
```

## *css属性选择器(w3c)*

| [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | 用于选取带有指定属性的元素。                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | 用于选取带有指定属性和值的元素。                             |
| [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | 用于选取属性值中包含指定词汇的元素。                         |
| [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | 用于选取带有以指定值开头的属性值的元素，该值必须是整个单词。 |
| [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | 匹配属性值以指定值开头的每个元素。                           |
| [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | 匹配属性值以指定值结尾的每个元素。                           |
| [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | 匹配属性值中包含指定值的每个元素。                           |