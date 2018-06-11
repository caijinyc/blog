---
title: JS 正则表达式
date: 2018-04-15
tags:
- JavaScript
---
因为自己的正则学的真的不是很好，所以重新学习一边，顺便记录一下，以备复习。<!--more-->

### 正则工具
推荐这款正则的工具，Github：[Regexper](https://github.com/javallone/regexper-static)，用图片解释的正则，很方便很实用。因为是国外的网站，有时候会上不去，所以推荐下载下来本地运行，这样会比较方便。

### 初始化
* 字面量: `var reg = /\bis\b/g;`
* 构造函数：`var reg = new RegExp('\\d\\w\\d', 'g')`

使用构造函数的时候需要注意 `\` 反斜杠是需要转义的。

### 修饰符
* g：global 全文匹配
* i： ignoreCase 或略大小写
* m: multiline 换行符当成新的一行

```js
let reg = /\d/gim
reg.global
//true

reg.global = false
reg.global
// true
```

修饰符默认都是 false，而且都是只读的，只能在创建的时候确认，创建之后是无法修改的。

## 字符类
**[]**：[abc] 代表 a、b、c 中的一个匹配就可以。

### 字符类取反向
**^**：取反向类。

### 范围类
**[a-z]**：从 a 到 z 的任意字符，这是个**闭区间**，然后 [a-zA-Z] 可以连写。

### 预定义类
* `.`：等价于 [^\r\n] 除了回车符和换行符之外的所有字符
* `\d`：等价于 [0-9] 数字字符
* `\D`：非数字字符
* `\s`：[\t\n\x0B\f\r] 空白符
* `\S`：非空白符
* `\w`：等价于 [a-zA-Z_0-9] 单词字符（字母、数字、下划线）
* `\W`：非单词字符

### 边界
* `^`：以xxx开头 /^@./
* `$`：以xxx结尾 /.@$/
* `\b`：单词边界

### 量词
* `?`：出现0次或1次（最多出现一次）
* `+`：出现1次或多次（至少出现一次）
* `*`：出现0次或多次
* `{n}`：出现n次
* `{n,m}`：出现n到m次
* `{n,}`：至少出现n次

### 贪婪模式
正则表达式会匹配尽量多的值，直到匹配失败。例如：

```js
'12345678'.replace(/\d{3,6}/, 'M')
// "M78"
```

### 非贪婪模式
如果要取消贪婪模式，只需要在量词后面加一个问号 `/\d{4,6}?/`。例如：

```js
'12345678'.replace(/\d{3,6}?/, 'M')
// "M45678"
```

### 分组
使用  () 可以达到分组功能
`abcd{4} ----> (abcd){4}`

#### 分组捕获
```js
'2011-01-12'.replace(/(\d{4})-(\d{2})-(\d{2})/, '$2--$3--$1')
// '01--12--2011'
```

#### 忽略分组
不希望捕获某些分组，只需要在分组内加上 `? : `，就可以了。

```js
'2011-01-12'.replace(/(?:\d{4})-(\d{2})-(\d{2})/, '$2--$3--$1')
// '12--$3--01'
```

### 或
可以用 | 达到效果，`Byron | Casper`，使用分组来不干涉其他表达式。  
`BBry(on|CA)sper`

### 前瞻
正则表达式从文本头部向尾部开始解析，所以文本的尾部方向，称之为“前”（这个理解起来有点难，我想了好久）。所谓**前瞻**就是正则在匹配到规则的时候，向前检查是否符合断言。（其实还有后顾，但是 JS 暂时好像没有支持）。

#### 正向前瞻 exp(?=assert)
正向前瞻指的就是，表达式匹配以后还需要向前检查是否符合（正向）断言（assert）。例子：
```js
'a2*34v8'.replace(/\w(?=\d)/g, 'X')
// 'X2*X4X8'
```
#### 反向前瞻 exp(?!assert)
反向前瞻指的就是，表达式匹配以后还需要向前检查是否不符合（反向）断言（assert）。例子：
```js
'a2*34v8'.replace(/\w(?!\d)/g, 'X')
// 'aX*3XvX'
```

## 对象属性
### RegExp.prototype.test(str)
用于测试字符串参数中是否存在匹配正则表达式模式的字符串，存在返回 true，否则返回 false。  

**非全局 （没g）**  
当不使用全局的时候，test() 方法会直接返回 true，或者 false。

**全局 （有g）**  
这个时候就会被 lastIndex（当前匹配结果的，最后一个字符的，下一个字符） 影响了，因为每次匹配之后，它的 lastIndex 都会改变，这时候就会影响到 test() 方法的结果。看例子：
```js
let reg = /\w/g
while(reg.test('ab')) {
  console.log(reg2.lastIndex)
}
// 1
// 2
```
当但三次循环的时候，因为 lastIndex 为 2 所以就直接返回了 false，循环结束。

### RegExp.prototype.exec(str)
使用正则对字符串执行搜索，并将更新全局 RegExp 对象的属性以反映匹配结果。
* 如果没有匹配返回 null，否则返回一个结果数组
   * index 声明匹配文本的第一个字符的位置
   * input 存放被检索的字符串 string

**非全局 （没g）**  

- 第一个元素是相匹配的文本
- 第二个元素是与 正则 的第一个字表达式相匹配的文本（如果有的话）
- 第三个就是与 正则 的第二个字表达式相匹配的文本（如果有有），以此类推
例子：
```js
/\d\w\d/.exec('a1b2c3d4e5')
// ["1b2", index: 1, input: "a1b2c3d4e5", groups: undefined]

/\d(\w)\d/.exec('a1b2c3d4e5')
// ["1b2", "b", index: 1, input: "a1b2c3d4e5", groups: undefined]

/\d(\w)(\d)/.exec('a1b2c3d4e5')
// ["1b2", "b", "2", index: 1, input: "a1b2c3d4e5", groups: undefined]
```
**全局（有g）**  

直接看例子吧：
```js
let ts = 'a1b2c3d4e5'
let reg = /\d(\w)(\d)/g
let ret
while (ret = reg.exec(ts)) {
  console.log(ret)
}

// ["1b2", "b", "2", index: 1, input: "a1b2c3d4e5", groups: undefined]
// ["3d4", "d", "4", index: 5, input: "a1b2c3d4e5", groups: undefined]
```

### String.prototype.search(reg)
* search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
* 方法返回第一个匹配结果 index，查找不到返回 -1。
* search() 方法不执行全局匹配，它将忽略标志 g，并且总是从字符串的开始进行检索。

### String.prototype.match(reg)
* match() 方法将检索字符串，以找到一个或多个与 regexp 匹配的文本。
* regexp 是否具有标志 g 对结果影响很大！

**非全局 （没g）**

* 返回数组的第一个元素存放的匹配文本，而其余的元素存放的是与正则表达式的字表达式匹配的文本。
* 除了常规的数组元素之外，返回的数组还含有 2 个对象属性。
  * index 声明位置
  * inpu 声明对 stringObject 的引用
```js
'a1a2b3c4d5e'.match(/\d(\w)\d/)
 // ["1a2", "a", index: 1, input: "a1a2b3c4d5e", groups: undefined]
```

**全局（有g）**
* 全局检索，找到所有字符串中的所有匹配字符串
  * 没有找到任何匹配，返回 null
  * 如果找到了一个或多个匹配，返回一个数组
* 数组元素中存放的是字符串中所有的匹配子字符串，不包含 index 和 input

```js
'a1a2b3c4d5e'.match(/\d(\w)\d/g)
// ["1a2", "3c4"]
```

### String.prototype.split(reg)
一般的使用：`'a,b,c,d'.split(','); ---> ["a", "b", "c", "d"]'`

**使用正则**
```js
'a1a2b3c4d5e'.split(/\d/)
// ["a", "a", "b", "c", "d", "e"]
```
### String.prototype.replace(reg)
replace 的第二个值可以传入 function 来处理复杂的替换。

**function参数含义**：function 会在每次匹配替换的时候调用，有四个参数。返回值就是替换的内容。
1. 匹配字符串
2. 正则表达式分组内容，没有分组则没有该参数
3. 匹配项在字符串中的 index
4. 原字符串

例子：

```js
// 例子1：没有分组
'a1a2b3c4d5e'.replace(/\d/g, (match, index, o) => {
  return parseInt(match) + 5
})
// "a6a7b8c9d10e"

// 例子2：有分组
'a1a2b3c4d5e'.replace(/(\d)\w(\d)/g, (match, g1, g2, index, o) => {
  return g1 + g2
})
// "a12b34d5e"
```

### 练习
使用正则来实现一个时间的函数。

```js
// 传入一个实例化后的时间，以及需要的时间格式，如 yyyy-mm-dd
// new Date()，会返回一个Date对象的实例。如果不加参数，实例代表的就是当前时间。
function formaDate (date, fmt) {
  // 找到 y 字符，然后替换成年
  if (/(y+)/i.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  let o = {
    'M+': date.getMonth() + 1,
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds()
  }
  for (let k in o) {
    // 找到相应字符，然后进行替换
    if (new RegExp(`(${k})`, 'i').test(fmt)) {
      let str = o[k] + ''
      fmt = fmt.replace(RegExp.$1, (m) => {
        // 是否需要补全0，例如 8:5 --> 08:05
        // 如果需要，则使用 substr 方法操作
        if (m.length === 2) {          
          str = ('00' + str).substr(str.length)
        }
        return str
      })
    }
  }
  return fmt
}

// 然后我们只需要传入实例化的时间，以及时间的格式，就可以返回符合格式的时间，非常的方便
formaDate(new Date(), 'yyyy-mm-dd hh:mm')
// "2018-04-15 20:02"
formaDate(new Date(), 'mm-dd-yyyy')
// "04-15-2018"
```
