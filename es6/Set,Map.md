# Set,Map

标签（空格分隔）： es6

---
###**Set**
Set本身是一个构造函数，用来生成 Set 数据结构。
```
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
for (let i of s) {
  console.log(i);
}
```
Set函数可以接受一个数组（或者具有 iterable接口的其他数据结构）作为参数，用来初始化。
```
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

const set = new Set(document.querySelectorAll('div'));
set.size // 56
```
数组去重
```
[...new Set(array)];
```
Set 方法：

1.操作：
size();
add(value)：添加某个值，返回 Set 结构本身。
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
has(value)：返回一个布尔值，表示该值是否为Set的成员。
clear()：清除所有成员，没有返回值。

2.遍历：
keys()：返回键名的遍历器
values()：返回键值的遍历器
entries()：返回键值对的遍历器
forEach()：使用回调函数遍历每个成员
```
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
for (let item of set.values()) {
  console.log(item);
}
for (let item of set.entries()) {
  console.log(item);
}
set.forEach((value, key) => console.log(key + ' : ' + value));
遍历：
let unique = [...new Set(arr)];
```


----------
###**Map**
1.操作：
size();
set(key,value)：添加某个值，返回 Set 结构本身。
get(key):get方法读取key对应的键值，如果找不到key，返回undefined。
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
has(value)：返回一个布尔值，表示该值是否为Set的成员。
clear()：清除所有成员，没有返回值。

2.遍历：
keys()：返回键名的遍历器
values()：返回键值的遍历器
entries()：返回键值对的遍历器
forEach()：使用回调函数遍历每个成员

