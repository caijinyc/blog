---
title: JS 中的 autoprefixer
date: 2018-04-20
tags:
- JavaScript
---
在编写 CSS 的时候，常常需要考虑游览器的兼容，需要给样式加上前缀。在 webpack 中直接写 CSS 的时候有 autoprefixer 插件帮我们自动添加，很方便。但是在 JS 中添加 CSS 的时候，插件就不起作用了，这个时候，就需要自己来添加了。所以可以实现一个函数，来为我们自动添加相应的游览器兼容前缀。<!-- more -->

直接上代码：

```js
// elementStyle 是一个对象，包含了游览器支持的 style
let elementStyle = document.createElement('div').style

let vendor = (() => {
  // 定义游览器前缀
  let transformNames = {
    webkit: 'webkitTransform',
    Moz: 'MozTransform',
    O: 'OTransform',
    ms: 'msTransform',
    standard: 'transform'
  }

  // 遍历前缀，如果游览器支持的话，就返回对应 key
  for (let key in transformNames) {
    if (elementStyle[transformNames[key]] !== undefined) {
      return key
    }
  }

  // 如果都不支持，那肯定是有问题的，返回 false
  return false
})()

// 把函数导出，然后直接使用 import 引用就行了
export function prefixStyle (style) {
  if (vendor === false) {
    return false
  }
  // 如果 vendor 为标准，就不改变 style
  if (vendor === 'standard') {
    return style
  }

  // 否则返回 vender(也就是 webkit Moz O ms 中的一个) + 样式首字母大写
  // 例如：webkit + transform ---> webkitTransform
  return vendor + style.charAt(0).toUpperCase() + style.substr(1)
}

```

首先，使用 createElement() 方法得到 Element 对象，然后得到游览器支持的 style 样式。

然后使用一个立即执行函数，来获取到样式前缀。

然后导出一个 prefixStyle 函数，需要使用的时候直接 import 导入就可以了。

使用的例子：

```js
// 首先引入 prefixStyle 函数
import {prefixStyle} from 'common/js/dom'

// 然后定义我们的样式
// 得到的 transfrom 就是加上前缀之后的样式
const transform = prefixStyle('transform')

// 使用
let m = document.querySelector('.box')
m.style[transform] = 'scale(2)'

// 如果我们的游览器是 webkit 内核，那么上面的代码就等于
m.style['webkitTransform'] = 'scale(2)'
```

例如，游览器用的是 webkit 内核，那么 prefixStyle('transform') 就可以得到 webkitTransform。

这样就很方便的给样式加上了前缀，因为使用了模块化的思想，所以可以很方便的重复使用。