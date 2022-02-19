---
title: 实现居中的几种方法
date: 2018-03-25
tags:
- CSS
---
在开发的过程中，很多时候我们需要实现居中，所以记录一下几种居中的方法。<!--more-->


## 水平居中
### text-align: center;
`text-align: center` 居中是**只针对行内元素**的，例如 span、a、img、input 、text 等行内元素。 

我们有这样的 HTML 结构：
```html
<div class="parent">
  <span>inline element</span>
</div>
```

行内居中只需要给父元素设置 `text-align: center` 就可以实现。

```css
.parent {
  text-align: center;
  width: 200px;
  height: 50px;
  border: 1px solid red;
}
```
注意：对于元素中的块级元素它是不起作用的。但是可以把块级元素的 `display` 设置为 `inline-block 或者 inline` 之后，也是可以生效的，但是需要注意设置 `inline` 之后是会丢失一些功能的，比如设置元素的宽高。
###  margin: 0 auto;
通过给自身设置 `margin: 0 auto` 可以实现对**块级元素**的居中。例如：
```html
<div class="parent">
  <div class="chilren"></div>
</div>
```

```css
.parent {
  width: 100%;
  border: 1px solid red;
}
.chilren {
  margin: 0 auto;
  width: 20px;
  height: 20px;
  background: red;
}
```
 > 和 `text-align: center` 一样，它也可以改变行内元素的 `display` 为 `inline-block` 来实现行内元素的居中。
### positon: absolute;
使用 `absolute` 也是可以实现居中的。**但是使用它之后，子元素就不能把父元素撑起来了。需要自己确定父元素的大小。**
```html
<div class="parent">
  <div class="chilren"></div>
</div>
```
```css
.parent {
  position: relative;
  width: 200px;
  height: 50px;
  border: 1px solid red;
}
.chilren {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 50px;
  height: 50px;
  background: red;
}
```
首先给父元素添加 `position: relative ` 这样子元素就可以通过父元素来实现绝对定位。然后让元素向右偏移 50%，需要注意，**这个时候居中的是子元素的左边线**。所以这时候就需要使用 `transilateX(-50%)` 让元素再相对自己向左移 50%（自身的 50%），这样就能完美实现居中啦。（这个方法在垂直居中的时候同样适用）

### display: flex;
使用弹性布局也可以很方便的实现水平居中（和垂直居中）。

看下例子：
```html
<div class="parent">
  <div class="chilren"></div>
</div>
```
```css
.parent {
  display: flex;
  justify-content: center;
  width: 200px;
  height: 50px;
  border: 1px solid red;
}

.chilren {
  left: 50%;
  width: 50px;
  height: 50px;
  background: red;
}
```
用 flex 来实现居中真的很方便，也是网页布局的一大利器~好用的不得了。对于 flex 不了解的，可以参考阮一峰老师的这篇文章：[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)。

## 垂直居中
### positon: absolute; 和 display: flex;
这两个方法用来实现垂直居中也是非常方便的。

#### positon: absolute;

通过决定定位实现垂直居中的方法和实现水平居中基本没什么两样，只需要向下偏移 50%，然后相对自身向上偏移 50% 就可以了。看代码：

```html
<div class="parent">
  <div class="chilren"></div>
</div>
```
```css
.parent {
  position: relative;
  width: 200px;
  height: 200px;
  border: 1px solid red;
}
.chilren {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate3d(-50%, -50%, 0);
  width: 50px;
  height: 50px;
  background: red;
}
```
这边使用了 `translate3d` 这个属性，三个值分别对应了 x、y、z 轴方向的位移。这样我们就完美的实现元素的居中啦~

#### margin-top: 50%;
根据绝对定位实现居中，也可以联想到使用 `margin-top: 50%` 和 `transform: translateY(-50%)` 来实现垂直居中，道理是差不多的。代码就不放了，相信会使用绝对定位实现居中的就会这个啦。

#### display: flex;
这个很简单，直接看代码吧。看过阮一峰老师文章就懂了，写的很详细，我就不多赘述了。
```html
<div class="parent">
  <div class="chilren"></div>
</div>
```
```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 50px;
  border: 1px solid red;
}

.chilren {
  left: 50%;
  width: 50px;
  height: 50px;
  background: red;
}
```
### line-height 实现文字的垂直居中

因为行高的垂直居中性，所以在行内的元素都是会居中的。那么使用 `line-height` 等于父元素高度也可以实现元素的居中。

```html
<div class="parent">
  <div class="chilren">啦啦啦，line-height</div>
</div>
```
```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  border: 1px solid red;
}
.chilren {
  line-height: 200px;
}
```
这个方法在实现文字垂直居中的时候是非常方便的。

暂时就先记录这么多常用的居中方法，以后有其他好用的方法的话会进行补充。








