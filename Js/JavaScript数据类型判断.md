# js中的数据类型和类型转换

## 1、概述

JavaScript有8种数据类型：

number、string、boolean、undefined、null、Symbol(es6+)、bigint（es11+）、Object

原始类型：number、string、boolean

复杂类型(引用类型)：Object、Array、Function



## 2、类型判断

### 2.1 typeof 运算符

```js
typeof 111 // 'number'
typeof '123' // 'string'
typeof false // 'boolean'
typeof undefined // "undefined"
```

typeof常用来判断基础类型，但是对于所有复杂类型均会返回Object

```js
typeof function fn(){} // "object"
typeof [1,3,4] // "object"
typeof {name:'123'} // "object"
typeof null // "object"
```

### 2.2 instanceof 运算符

instanceof可以被很好的用来区分数组和对象

```js
[1,3,5] instanceof Array // true
{name:'kevin'} instanceof Arrary // false
```

### 2.3 Object.prototype.toString.call() 方法

此方法通过原型方法返回对象的类型

```js
let obj = {}
Object.prototype.toString.call(obj) // '[object Object]'

let arr = []
Object.prototype.toString.call(arr) // '[object Array]'
```

### 2.4 constructor方法

```js
let obj = {}
obj.constructor === Object // true

let arr = []
arr.constructor === Array // true

```

### 2.5 Array.isArray()

```js
let arr = []
Arrary.isArray([]) // true
```



## 3、特殊值null和undefined

访问undefined变量时，往往会报错。它的设计初衷也就是为了更好的发现错误。

undefined转化为数值是会变成NaN、而null转换为数值时会场变成 0



## 4、JavaScript中的隐式转换

```js 
/*
			Number
		Sting	Boolean
	Object
	在隐式转换中，数据会向中间类型进行靠拢
*/

1 === true // true
true === 3 // false
// 对象类型转String会先尝试调用toString()方法 再调用 valueOf()
// 对象类型转Number会先尝试调用valueOf()方法，再调用 toString()
```



## 5、JavaScript值布尔真假值

### 5.1 操作符

下列运算符会返回布尔值：

- 前置逻辑运算符： `!` (Not)
- 相等运算符：`===`，`!==`，`==`，`!=`
- 比较运算符：`>`，`>=`，`<`，`<=`

注意：==双等号会对数据进行隐式转换，当比较的数据类型不同时，默认调用toString方法进行比较。===三等号则会对对数据的值和数据类型都强制比较(强推)。

注意：===号下任意类型等于本身，但是NaN不等。

### 5.2 JavaScript中的真假值

| 真值                   | 假值      |
| ---------------------- | --------- |
| true                   | false     |
| >0的number类型数值     | 0，-1     |
| 非空对象、数组、字符串 | undefined |
|                        | null      |
|                        | NaN       |
|                        | 空字符串  |

