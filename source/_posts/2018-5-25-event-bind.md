---
title: JS 事件委托
date: 2018-05-25
tags:
- JavaScript
---
面试被问到事件委托了，虽然很早之前就使用过事件委托，但是面试的时候居然没有能够讲清楚，再总结一下吧。
<!--more-->

> 第一次经历面试，真的很紧张，很多自己明明会的东西也说不清，实在是丢脸。面试估计已经凉凉 了，吸取一下教训吧，学习的时候一定要学习扎实了。

## 什么是事件委托
事件委托就是利用冒泡的原理，把事件加到父元素或祖先元素上，触发执行效果。

引用个的例子：
> 有三个同事预计会在周一收到快递。为签收快递，有两种办法：一是三个人在公司门口等快递；二是委托给前台MM代为签收。现实当中，我们大都采用委托的方案（公司也不会容忍那么多员工站在门口就为了等快递）。前台MM收到快递后，她会判断收件人是谁，然后按照收件人的要求签收，甚至代为付款。这种方案还有一个优势，那就是即使公司里来了新员工（不管多少），前台MM也会在收到寄给新员工的快递后核实并代为签收。

**那么为什么需要事件委托：**
- 优点1: 提高 JavaScript 性能。事件委托可以显著的提高事件的处理速度，减少内存的占用。
- 优点2: 动态的添加DOM元素，不需要因为元素的改动而修改事件绑定。

## 实现
**传统写法**
```js
<ul id="list">
  <li id="item1">item1</li>
  <li id="item2">item2</li>
  <li id="item3">item3</li>
</ul>

<script>
  let item1 = document.getElementById("item1");
  let item2 = document.getElementById("item2");
  let item3 = document.getElementById("item3");

  item1.onclick = () => {
    alert("hello item1");
  }
  item2.onclick = () => {
    alert("hello item2");
  }
  item3.onclick = () => {
    alert("hello item3");
  }
</script>
```

- 试想，如果需要绑定 100 个 `li` 元素，那么岂不是需要绑定 100 个事件（浪费内存）。
- 当添加一个 DOM 元素的时候，传统的写法还需要再绑定新添加的 DOM。

**事件委托**
```js
<ul id="list">
  <li id="item1">item1</li>
  <li id="item2">item2</li>
  <li id="item3">item3</li>
</ul>

<script>
  let ul = document.querySelector('#list');
  ul.addEventListener('click', (event) => {
    console.log('hello ' + event.target.innerText)
  })
</script>
```

利用事件委托，只需要给父元素添加了事件，就可以给所有 `li` 绑定事件，而不必要给每个 `li` 分别添加事件。这样就非常节省内存了，也很方便。

同时实现了动态添加 DOM 的时候不需要再添加事件。例子：
```js
<ul id="list">
  <li id="item1">item1</li>
  <li id="item2">item2</li>
  <li id="item3">item3</li>
</ul>

<script>
  let ul = document.querySelector('#list');
  ul.addEventListener('click', (event) => {
    console.log('hello ' + event.target.innerText)
  })
  let newVal = `<li id="item4">item4</li>`
  ul.innerHTML = ul.innerHTML + newVal
</script>
```

这个时候点击新添加的 item4 会发现打印了 `hello item4` 说明 item4 绑定了事件，即事件委托可以为新添加的 DOM 元素动态的添加事件。

