---
layout: post
title: typescript编码规范
categories: typescript
description: typescript编码规范
keywords: typescript, ES6
---
## 1. 基本格式
### 1.1 缩进
> `BAD`: (indent)
```typescript
const badIndent = {
    a: 'string',
}
```

> `GOOD`: 统一使用两个空格进行缩进
```typescript
const goodIndent = {
  a: 'string',
}
```

> `Reason`: 让代码更加紧凑，增加横向代码展示空间。让代码在非编辑器环境下展示可读性更高

### 1.2 缩进
> `BAD`: (align)
```typescript
declare function foo(
  a: number,
  b: string,
c: boolean,
  d: { a: number; b: string }
)

interface Foo {
  a: number,
    b: string,
}
```

> `GOOD`: 当单行无法容下参数声明时, 在多行显示, 且参数必须对齐. 同理, 类成员, 数组成员接口成员也需要对齐
```typescript
declare function fooo(
  a: number,
  b: string,
  c: boolean,
  d: { a: number; b: string }
)

interface Fooo {
  a: number,
  b: string,
}
```

> `Reason`: 可读性 

### 1.3 引号
> `BAD`: (quotemark)

```typescript
const str = "string"
```

> `GOOD`: 使用单引号

```typescript
const str = 'string'
```

> `Reason`: 统一单引号增强代码可读性; 单引号更容易输入

---
<br/>

> `BAD`: (quotemark)
```typescript
function jsx() {
  return <div str='string' />
}
```

> `GOOD`: 对于JSX, 字符串是使用双引号, 这是遵循XML的规范
```typescript
function jsx() {
  return <div str="string" />
}
```

### 1.3 引号
> `BAD`: (semicolon)
```typescript
const withSemicolon = 'strig';
```


> `GOOD`: 不使用分号
```typescript
const withSemicolono = 'strig'
```

> `Reason`: 这个不作硬性要求，不使用分号在大多数情况下可以让代码变得简洁一些。
> <br>显式给定分号可以帮助语言格式化(如uglify)工具输出一致的结果，另外可以避免一些坑， 如 `foo() \n (function(){})` 将会变成单语句. 
> 不过使用`prettier`这类格式化工具，可以自动处理这些问题。保证代码格式的统一

### 1.4 行宽
> `BAD`: (max-line-length)
```typescript
const veryverylongArray = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'night']
```

> `GOOD`: 最大行宽不超过80
```typescript
const veryverylongArrayo = [
  'one',
  'two',
  'three',
  'four',
  'five',
  'six',
  'seven',
  'eight',
]
```

> `Reason`: 可读性，为了更好的显示代码和在非编辑器环境预览代码.<br>
> 大多数编辑器都支持显示标尺， 比如`VSCode`的`editor.rules`设置选项。 另外`prettier`也是根据代码的行宽来格式化代码的。<br>
> 最佳实践的行宽是80

### 1.5 最大行数
> `GOOD`: (max-file-line-count) 为了更好地阅读和定位代码, 当你的文件超过*500*行时,就需要考虑将文件分割为多个文件了

### 1.6 逗号
> `BAD`: (trailing-comma)
```typescript
const notrailingWhitespace = {
  foo: 'string',
  bar: 1
}

const story = [
    once
  , upon
  , aTime
]

import {
  Foo,
  Bar,
  Baz
} from './Foo'

function foo(
  bar: number,
  baz: string
) {/*...*/}
```

> `GOOD`: 对于跨越多行的*数组字面量*, *对象字面量*, *import*, *export*, *type字面量*, *函数形参列表*, *函数调用实参*等, 都应该尾随逗号,
```typescript
const trailingWhitespace = {
  foo: 'string',
  bar: 1,
}

const story = [
  once,
  upon,
  aTime,
]

import {
  Foo,
  Bar,
  Baz,
} from './Foo'

function foo(
  bar: number,
  baz: string,
) {/*...*/}
```

> `Reason`: 这可以让`git diffs`信息更加简洁. typescript会在编译阶段自动移除多余的逗号，所以不需要担心兼容性问题

## 2. 命名规则
> `BAD`
```typescript
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}
```

> `GOOD`：使用驼峰式命名对象、函数和实例。
```typescript
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

---
<br/>

> `BAD`: (class-name)
```typescript
declare class className {}
```

> `GOOD`: 始终使用PascalCased形式命名*class*, *type alias*, *interface*, *namespace*等
```typescript
declare class ClassName {}
```

---
<br/>

> `BAD`: (interface-name)
```typescript
interface IInterfaceName {}
```

> `GOOD`: typescript 官方不建议为接口添加I前缀和类一样, 接口使用PascalCased格式
```typescript
interface InterfaceName {}
```

> `Reason`: 尽管对Java/C#程序员来说是很正常的事情. JavaScript标准库接口的惯例是没有I开始的,比如Window, Document. 
> <br>另外和Java或C#不同的是，Typescript中的类也可以被implement 或被接口extends, 即类和接口的界限不是十分突出

---
<br/>

> `BAD`
```typescript
class Foo {
  _bar: number
  // ... other
}
```

> `GOOD`: 不要使用下划线`_`开头或者结尾来命名属性。在某些情况，Javascript程序员习惯使用`_` 开头来命名私有属性， 但是Typescript中有完善的访问描述符，所以应该避免使用下划线
```typescript
class Foo {
  private bar: number
  // ... other
}
```

---
<br/>

> `BAD`
```typescript
function q() {
  // ...stuff...
}
```

> `GOOD`: 避免单字母命名。命名应该具备描述性。
```typescript
function query() {
  // ..stuff..
}
```

---
<br/>

假设文件内容为
```typescript
/**
 * Select
 */
// ...
export default class Select extends React.Component {
  // ...
}

```

> `BAD`
```typescript
import CheckBox from './checkBox'
// or
import CheckBox from './check_box'
```

> `GOOD`: 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致. 换句话说就是文件命名和默认导出保持一致
```typescript
import CheckBox from './CheckBox'
```

---
<br/>

> `BAD`
```typescript
const platform = 'android'
```

> `GOOD`: 使用`UPPER_SNAKE_CASE`来命名常量
```typescript
const PLATFORM = 'android'
```

## 3. 注释和文档

> `BAD`: (comment-format)
```typescript
//bad
```
> GOOD: 使用`//`作为单行注释, 注释内容前面必须有空格
```typescript
// good
```

---
<br/>

> `BAD`: (file-header)
```typescript
export default function Title() {}
```

> `GOOD`: 每个文件开头必须提供关于这个文件的注释.说明文件的内容, TODO, 更新日期, 权限等信息
```typescript
/**
 * Title bala bala bala
 * @example
 *  <Title icon="apple">content</Title>
 */

export default function Title() {}
```

---
<br/>

> `BAD`
```typescript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {

  // ...stuff...

  return element
}
```


> `GOOD`:  使用`/** ...*/`作为多行注释, 并使用JSDoc格式对代码进行文档化,
```typescript
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param {String} tag
 * @return {Element} element
 */
function make(tag) {

  // ...stuff...

  return element
}
```

---
<br/>

> `GOOD`:  给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。<br/>
> <br> `Tips`: 可以使用`Atom`编辑器的`todo-show`插件，或者`VSCode`的`TODO Parser`插件来解析、查找和处理的你项目中的TODO和FIXME注释
```typescript
// 使用 // FIXME: 标注问题
class Calculator {
  constructor() {
    // FIXME: shouldn't use a global here
    total = 0;
  }
}

// 使用 // TODO: 标注问题的解决方式
class Calculator {
  constructor() {
    // TODO: total should be configurable by an options param
    this.total = 0;
  }
}
```

## 4. 函数
### 4.1 箭头函数
> `BAD`: (arrow-return-shorthand)
```typescript
const arrowFunction = (i) => i 
```

> `GOOD`: 只有一个参数时,可以省略括号
```typescript
const arrowFunctiono = i => i 
```

---
<br/>

> `BAD`: (arrow-return-shorthand)
```typescript
const shorthandReturn = () => { return true }
```

> `GOOD`: 如果可以尽量使用简写的返回
```typescript
const shorthandReturno = () => true
```

---
<br/>

> `BAD`
```typescript
function foo() {
  const self = this
  return function() {
    console.log(self)
  }
}

// or
function foo() {
  const that = this
  return function() {
    console.log(that)
  }
}
```

> `GOOD`: 别保存 this 的引用。优先使用箭头函数
```typescript
function foo() {
  return () => {
    console.log(this)
  }
}
```


### 4.2　函数
> `BAD`: (new-parens)
```typescript
let date = new Date
```

> `GOOD`: new 操作始终加上()
```typescript
date = new Date()
```

---
> `BAD`
```typescript
const foo = function () {
}
```

> `GOOD`: 使用函数声明代替函数表达式
```typescript
function foo() {
}
```

> `Reason`: 因为函数声明是可命名的，所以他们在调用栈中更容易被识别。此外，函数声明会把整个函数提升（hoisted），而函数表达式只会把函数的引用变量名提升

---
> `BAD`
```typescript
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

function proxy() {
  foo.apply(null, arguments)
}
```

> `GOOD`: 不要使用 arguments。可以选择 rest 语法 ... 替代。
```typescript
function concatenateAll(...args) {
  return args.join('');
}

function proxy(...args) {
  foo(...args)
}
```

> `Reason`: 使用 ... 能明确你要传入的参数, 可以被类型检查。另外 rest 参数是一个真正的数组，而 arguments 是一个类数组。


## 5. 对象

> `BAD`: (object-literal-shorthand)
```typescript
const obj = {
  'foo': 'string',
  'foo bar': 1,
}
```

> `GOOD`: 只有在必要时, 才添加引号
```typescript
const objo = {
  foo: 'string',
  'foo bar': 1,
}
```

---

> `BAD`: (object-literal-shorthand)
```typescript
const shorthandObj = {
  obj: obj,
  foo: 'string',
}
```

> `GOOD`: 优先使用对象简写模式
```typescript
const shorthandObjo = {
  obj,
  foo: 'string',
}
```

---

> `BAD`: (prefer-object-spread)
```typescript
const obj1 = {
  a: 1,
  b: 2,
}
let objectAssign = Object.assign({}, obj1)
```

> `GOOD`: 优先使用object spread 语法
```typescript
let objectSpread = { ...obj1 }
```


## 6. 操作符/表达式
> `BAD`: (binary-expression-operand-order)
```typescript
const x = 1
const binaryOperation = 1 + x
```

> `GOOD`: 二元操作符中的字面量应该始终在变量的右边
```typescript
const binaryOperationo = x + 1
```

---

> `BAD`: (no-boolean-literal-compare)
```typescript
const ok: boolean = true
let baz = ok === true ? 'foo' : 'baz'
```

> `GOOD`: 不要比较boolean 字面量
```typescript
baz = ok ? 'foo' : 'baz'
```

---

> `BAD`: (prefer-switch)
```typescript
const type: string = 'foo'
if (type === 'foo') {
  console.log('foo')
} else if (type === 'bar') {
  console.log('bar')
} else if (type === 'baz') {
  console.log('baz')
} else {
  console.log('default')
}
```

> `GOOD`: 当使用if语句比较超过三个时, 考虑使用switch
```typescript
switch (type) {
  case 'foo':
    console.log('foo')
    break
  case 'bar':
    console.log('bar')
    break
  case 'baz':
    console.log('baz')
    break
  default:
    console.log('default')
}
```

---

> `BAD`: (prefer-template)
```typescript
const hello = 'hello'
const world = 'world'
let temp = 'print' + hello + ' ' + world
```

> `GOOD`: 优先使用字符串模板
```typescript
temp = `print${hello} ${world}`
```


## 7. 模块
> `BAD`
```typescript
import IReact from 'react'
```

> `GOOD`: 导入默认导出, 最好匹配库默认的导出名
```typescript
import React from 'react'
```

---
<br/>

> `BAD`: (import-blacklist)
```typescript
import _ from 'lodash'
```

> `GOOD`: 为了减少打包体积, 避免引入整个模块, 而是按需引入需要的子模块. 典型的应用就是lodash
```typescript
import findIndex from 'lodash/findIndex'
```

---
<br/>

> `BAD`
```typescript
import myIcon from 'assets/icons/github.svg'
```

> `GOOD`: 为了区分静态资源和JavaScript模块, 我们使用require 来加载静态资源, 使用import来
```typescript
// 导入Javascript模块
import Foo from './path/to/foo'

// 导入静态资源
<Icon src={require('assets/icons/myicon.svg')} />
<img src={require('assets/images/bg.png')} />
```

---
<br/>

> `BAD`
```typescript
// container/Foo/bar.ts
import Baz from '../../components/Baz'
```

> `GOOD`: 优先使用非相对路径来导入模块, 从而避免'路径地狱'. 使用相对路径还不利于模块的移动. 通过设置`tsconfig.json`的`paths`选项和`webpack`的`resolve.modules`或`resolve.alias` 来实现模块查找, 比如将src的根目录设置为node模块的查找目录
```typescript
import Baz from 'components/Baz'
```

---
<br/>

> `BAD`
```typescript
require.ensure(['mymodule'], require => {
  const myModule = require('mymodule')
  // ...
})
```

> `GOOD`: 避免使用webpack提供的非标准的模块加载语法, 尽量使用ES6模块
```typescript
import('mymodule').then(module => {/*...*/})
```

## 8. 接口
> `BAD`: (interface-over-type-literal)
```typescript
type t = {
  foo: number,
  bar: string,
}
```

> `GOOD`: 优先使用接口来代替type, 因为接口可以被实现, 扩展和合并
```typescript
interface To {
  foo: number,
  bar: string,
}
```



## 9. 类
> `BAD`: (member-access)
```typescript
class Member {
  foo() {
    // do somthing
  }
}
```

> `GOOD`: 始终显式设置成员的可见性, 这样可以提醒你慎重考虑属性的可见性, 避免将因为省略描述符而将私有成员暴露出去
```typescript
class Membero {
  public foo() {
    // do somthing
  }
}
```

---
<br/>

> `BAD`
```typescript
class Foo extends React.Component {
  constructor() {
    this.bar = this.bar.bind(this)
  }
  bar () {
    console.log('baz')
  }
  render() {
    return <button onClick={this.bar}>
  }
}

// or

class Foo extends React.Component {
  bar () {
    console.log('baz')
  }
  render() {
    return <button onClick={this.bar.bind(this)}>
  }
}
```

> `GOOD`: 使用`class properties`初始化类变量和实例变量, 配合`箭头函数`可以绑定方法的`this`上下文
```typescript
class Foo extends React.Component {
  // 静态属性
  static defaultProps = {
    prop1: 1
  }
  // 实例属性
  baz: number = 0
  // 配合箭头函数绑定上下文
  bar = () => {
    console.log('baz')
  }
  render() {
    return <button onClick={this.bar}>
  }
}
```

> `Reason`: 可读性


## 参考文献
1. [Typescript deep dive](https://www.gitbook.com/book/basarat/typescript/details)
2. [Airbnb Javascript Style Guide](https://github.com/airbnb/javascript)