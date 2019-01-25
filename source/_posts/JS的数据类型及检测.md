---
title: Javascript的数据类型及检测
date: 2019-01-16 19:23:14
tags:
- JavaScript
categories:
- 前端
---

# **JavaScript 有几种类型的值？** 

Javascript 有两种数据类型，分别是**基本数据类型**和**引用数据类型**。

- 基本数据类型包括 Undefined、Null、Boolean、Number、String、Symbol (ES6 新增，表示独一无二的值)

- 引用数据类型统称为 Object 对象，主要包括对象、数组和函数。

## (一)基本数据类型

### 1.**值是不可变的** 

### **2.存放在栈区** 

原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。 

### **3.值的比较** 

- == : 只进行值的比较，会进行数据类型的转换。
- === : 不仅进行值的比较，还要进行数据类型的比较

## (二)**引用数据类型** 

### **1.值是可变的** 

```javascript
var a={age:20}； 
a.age=21； 
console.log(a.age)；//21 
```

上面代码说明引用类型可以拥有属性和方法，并且是可以动态改变的。 

### **2.同时保存在栈内存和堆内存**

引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。 

 ![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5VfdbarrED0jzOaGibKyicE8r68gtHcD19vrYIPbzrQmQpaqV9Dxg5n4KxRFww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) 

### **3.比较是引用的比较** 

一个变量给另一个变量赋引用类型的值时，同样也会将存储在变量中的对象的值复制一份放到为新变量分配的空间中 。

```javascript
var a = {age:20}; 
var b = a; 
b.age = 21; 
console.log(a.age == b.age);//true 
console.log(a.age);//21 
```

变量 a 初始化时，a 指针指向对象{age:20}的地址，a 赋值给 b 后,b 又指向该对象{age:20}的地址，这两个变量指向了同一个对象。因此，改变其中任何一个变量，都会相互影响。

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5VfdbatNianVSvleALACNlWoWmXS1ibe6gDVib9VUwSiaDNsLN3ibBYxcAic9JbhBg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) 

此时，如果取消某一个变量对于原对象的引用，则不会影响到另一个变量。 

```javascript
var a={age:20};
var b=a; 
a = 1; 
b // {age:20} 
```

上面代码中，a 和 b 指向同一个对象，然后 a 的值变为 1，这时不会对 b 产生影响，b 还是指向原来的那个对象。 

# **判断数据类型** 

## **1.typeof** 

typeof 返回一个表示数据类型的字符串，返回结果包括：number、boolean、string、symbol、object、undefined、function 等 7 种数据类型，但**不能判断 null、array（返回的是object）** 等 。

> null 表示一个值被定义了，定义为“空值”
>
> undefined 表示根本不存在定义。 
>

```javascript
typeof Symbol(); // symbol 有效 
typeof ''; // string 有效 
typeof 1; // number 有效 
typeof true; //boolean 有效 
typeof undefined; //undefined 有效 
typeof new Function(); // function 有效 
```

数组和对象返回的都是 object，这时就需要使用 instanceof 来判断 

## **2.instanceof** 

instanceof 是用来判断 A 是否为 B 的实例，表达式为：**A instanceof B**，如果 A 是 B 的实例，则返回 true,否则返回 false。

instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

```javascript
[] instanceof Array; //true 
{} instanceof Object;//true 
new Date() instanceof Date;//true 
new RegExp() instanceof RegExp//true 
```
*注：数据类型大写*
- 关于数组的类型判断，还可以用 ES6 新增**Array.isArray()** 

```javascript
Array.isArray([]); // true 
```

### instanceof 三大弊端

1. 对于基本数据类型来说，字面量方式创建出来的结果和实例方式创建的是有一定的区别的 

   ```javascript
   console.log(1 instanceof Number)//false 
   console.log(new Number(1) instanceof Number)//true 
   ```

   从严格意义上来讲，只有实例创建出来的结果才是标准的对象数据类型值，也是标准的 Number 类的一个实例；对于字面量方式创建出来的结果是基本的数据类型值，不是严谨的实例。但是由于 JS 的松散特点，导致了可以使用 Number.prototype 上提供的方法。 

2. 只要在当前实例的原型链上，我们用其检测出来的结果都是 true。在类的原型继承中，我们最后检测出来的结果未必准确。 

   ```javascript
   var arr = [1, 2, 3]; 
   console.log(arr instanceof Array) // true 
   console.log(arr instanceof Object);  // true 
   ```

3. 不能检测 null 和 undefined 

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5Vfdba4kvaibmKPQZvZhbWYbIZTJaLiaQIN1uvG5SESfqj1MSaAUCxlWTZwZQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) 

## **3.===** 

只能用于判断 null 和 undefined，因为这两种类型的值都是唯一的。 

```javascript
var a = null;
typeof a; // "object" 
a === null; // true 
```

undefined 还可以用 typeof 来判断 

## **4.constructor** 

constructor 作用和 instanceof 非常相似。但 constructor 检测 Object 与 instanceof 不一样，还可以处理基本数据类型的检测。 

```javascript
var aa=[1,2]; 
console.log(aa.constructor===Array);//true 
console.log(aa.constructor===RegExp);//false 

console.log((1).constructor===Number);//true
```

### constructor 两大弊端

1. null 和 undefined 是无效的对象， constructor不存在，这两种类型的数据不能通过constructor来判断。
2. 函数的 constructor 是不稳定的，这个主要体现在把类的原型进行重写，在重写的过程中很有可能出现把之前的 constructor 给覆盖了，这样检测出来的结果就是不准确的

```javascript
function Fn(){}; 
Fn.prototype = new Array();
var f = new Fn;
console.log(f.constructor);//Array 
```

## 5.Object.prototype.toString.call() 

Object.prototype.toString.call() 最准确最常用的方式。首先获取 Object 原型上的 toString 方法，让方法执行，让 toString 方法中的 this 指向第一个参数的值。 

```javascript
Object.prototype.toString.call('');  //[object String]
Object.prototype.toString.call(1);  //[object Number]
Object.prototype.toString.call(true); // [object Boolean]
Object.prototype.toString.call(undefined); // [object Undefined]
Object.prototype.toString.call(null); // [object Null]
Object.prototype.toString.call(new Function()); // [object Function]
Object.prototype.toString.call(new Date()); // [object Date]
Object.prototype.toString.call([]); // [object Array]
Object.prototype.toString.call(new RegExp()); // [object RegExp]
Object.prototype.toString.call(new Error()); // [object Error]
Object.prototype.toString.call(document); // [object HTMLDocument]
Object.prototype.toString.call(window); //[object global] window是全局对象global的引用 
```

补充：

> - 对于 Number、String，Boolean，Array，RegExp、Date、Function 原型上的 toString 方法都是把当前的数据类型转换为字符串的类型（它们的作用仅仅是用来转换为字符串的）
>
> - Object 上的 toString 并不是用来转换为字符串的。
>
>   Object 上的 toString 它的作用是返回当前方法执行的主体（方法中的 this）所属类的详细信息即"[object Object]"，其中第一个 object 代表当前实例是对象数据类型的(这个是固定死的)，第二个 Object 代表的是 this 所属的类是 Object。 

# 总结

|           | typeof | instanceof | ===  | constructor |
| --------- | ------ | ---------- | ---- | ----------- |
| Null      |        |            | √    |             |
| Undefined | √      |            | √    |             |
| Boolean   | √      | √          |      | √           |
| String    | √      | √          |      | √           |
| Number    | √      | √          |      | √           |
| Symbol    | √      | √          |      | √           |
| Object    | √      | √          |      | √           |
| Array     |        | √          |      | √           |
| Function  | √      | √          |      | √           |
