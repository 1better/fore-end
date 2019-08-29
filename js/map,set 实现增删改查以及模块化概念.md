## *map,set*

```js
// map set object array 实现增删改查
let item={t:1};
let map=new Map();
let set=new Set();
let obj={}; 
let array = [];

// 增
map.set('t',1);
set.add(item);
obj['t']=1;
array.push(item)
console.info('map-set-obj-array-add',obj,map,set,array); 
  
// 查 
console.info({
  map_exist:map.has('t'),
  set_exist:set.has(item),
  obj_exist:'t' in obj,
  arr_exist: array.find(item => item.t)
}) 

// 改
map.set('t',2);
item.t=2;
obj['t']=2;
console.info('map-set-obj-array-modify',obj,map,set,array); 

// 删除
map.delete('t');
set.delete(item); 
delete obj['t'];
let index = array.findIndex(item => item.t);
array.splice(index, 1);
console.info('map-set-obj-array-delete',obj,map,set,array);
```



