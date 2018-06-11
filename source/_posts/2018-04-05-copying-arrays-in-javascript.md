---
title: 关于 JavaScript 中的复制数组
date: 2018-04-05
tags:
- JavaScript
---

之前在写扫雷的时候，因为需要用到二维数组，当时就在复制数组这里出现了问题，所以记录一下。<!--more-->

当我们在需要复制数组的时候一定需要注意，**数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。**我们来看例子：

```js
var arr1 = [1, 2, 3]
var arr2 = arr1
arr1[0] = 5

console.log(arr2) // [5, 2, 3]
```
 
上面代码中，`arr2` 并不是 `arr1` 的克隆，而是指向同一份数据的另一个指针。修改 `arr2`，会直接导致 `arr1` 的变化。

那么如果正确的复制数组呢？可以使用 [concat()](http://www.w3school.com.cn/jsref/jsref_concat_array.asp) 用于连接两个或多个数组。该方法不会改变现有的数组，而仅仅会返回被连接数组的一个**副本**。看例子：

```js
var arr1 = [1, 2, 3]
var arr2 = arr1.concat()
arr1[0] = 5

console.log(arr2) // [1, 2, 3]
```

因为 `concat()` 返回的是一个副本，所以这个时候改变 `arr1` 就不会导致 `arr2` 改变了。



**还可以利用 ES6 中的扩展运算符来复制数组**

```js
var  arr1 = [1, 2];
// 写法一
var  arr2 = [...arr1];
// 写法二
var [...arr2] = arr1;
```

参考资料：[阮一峰ES6入门：扩展运算符的应用](http://es6.ruanyifeng.com/#docs/array#%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6%E7%9A%84%E5%BA%94%E7%94%A8)