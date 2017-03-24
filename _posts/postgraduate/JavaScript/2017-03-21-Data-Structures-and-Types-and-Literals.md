---
layout: post
title: "JavaScript的数据结构和数据类型"
date: 2017-03-21 09:00:00 +0800 
categories: 研究生涯
tag: JavaScript
---
* content
{:toc}


关于数据类型和数据结构的概念，可以参考[Wiki百科](https://en.wikipedia.org/wiki/Data_type)。

在`JavaScript`中，对象是语言的中心，连`functions`也被视为对象。

`JavaScript`有两种基本的数据类型： `primitive type` and `reference type`。两者都是通过对象进行访问的。*原始类型*保存为简单的数据值，*引用类型*则保存为对象(`objects`)，其本质是指向内存位置的引用。

其它的编程语言用栈(`stack`)来储存原始类型，用堆(`heap`)来储存引用对象。`JavaScript`则完全不同：**它使用一个`variable object`来追踪变量的生存周期**，原始类型的值被直接保存在变量对象中，而引用类型的值作为一个指针保存在变量对象中。

<!-- more -->

## Data types ##

> The lastest ECMAScript standard defines **seven(七种)** data types.

### Six data types that are primitives ###

原始数据类型指的是按照原样保存一些简单的数据，比如`true`、`25`。所以的原始类型都可以使用字面量(literal)来代表它们的值，而*字面量*表示不被存放在变量中的值。

> A **primitive**(基元) is data that is not an object and has no methods. 

- `Boolean`. `true` and `false`.
- `null`. A special keyword denoting a null value. And `null` is not the same as Null, NULL ,or any other variant for JavaScript is case-sensitive.
- `undefined`. A top-level property whose value is undefined.
- `Number`. 42 or 3.14159.
- `String`.
- `Symbol` (new in ECMAScript 2015). A data type whose instances are unique and immutable.

#### 鉴别原始类型

鉴别原始类型的最佳方法是使用`typeof`操作符。但以下代码可能需要注意：

``` javascript
console.log(typeof null);//'object'
```
> 这被委员会TC39认定为一个错误。

#### 原始方法(原始变量，原始值)

原始类型(`string`、`number`、`bool`有方法，`null`和`undefined`没有)也拥有方法，但它们并不是对象，`JavaScript`只是使它们看上去像对象一样而已。

``` javascript
var name = 'Nicholas';
var lowercaseName = name.toLowerCase(); 
```

### Reference type ###

*引用类型*指`JavaScipt`中的对象(objects)，*引用值*是引用类型的实例(instances)，和对象是同义词。

*对象*是属性(properties)的无序列表，*属性*包含键/值。如果一个属性的值是*函数(function)*，就被成为方法(method)。

我们可以通过使用`new`操作符和构造函数来创建对象。

``` javascript
var object1 = new Object();
var object2 = object1;
```
>更多有关创建对象的操作在这里：[`Object`]({{ '/2017/03/14/Objects' | prepend: site.baseurl }})

上述代码实例化了一个通用对象，并把它的引用保存在`object1`中。**因为引用类型不在变量中直接保存对象，所以`object1`变量实际上并不包含对象的实例，而是一个指向内存中实际对象所在位置的指针(或者说引用)**，所以将`object1`的值赋值给`object2`变量，使得两个变量各获得一份指针的拷贝，指向内存中同一对象。这是对象与原始值之间的一个基本差别。

#### Dereferencing Objects

虽然`JavaScript`有`garbage-colletced language`，但是最好在不使用对象时将引用解除。

``` javascript
var object1 = new Object();
//do something
object1 = null;
```

#### 内建类型的实例化

+ `Array` - 数组类型
+ `Dage` - 日期和时间类型
+ `Error` - 运行期错误类型
+ `Function` - 函数类型
+ `Object` - 通用对象类型
+ `RegExp` - 正则表达式类型

#### 引用类型也有字面量形式

``` javascript
//Object literal
var book = {"name": "The Kate Runner",
			"year": 2014};

//Array literal
var colors = ["red","bule","green"];

//Function literal
function reflect(value){
	return value;
}

//Regular Expression literal
var number = /\d+/g;
```

> 我们基本上都是使用字面量来定义函数

#### 鉴别引用类型

函数最容易鉴别的引用类型，因为对函数使用`typeof`操作符时，返回值是`function`。

对于其他的引用类型，使用`typeof`操作符都返回`object`。我们选择使用`instanceof`操作符。

``` javascript
var items = [];
var object = {};
function reflect(value){
	return value;
}
console.log(items instanceof Array); //true
console.log(object instanceof Object); //true
cosole.log(reflect instanceof Function); //true
```

> `instanceof`操作符同样可以鉴别继承类型。然而，**所有的引用类型都继承自`Object`**，所以`console.log(reflect instanceof Object)`返回`true`。

`ECMAScript 5`引入了`Array.isArray()`来鉴别一个值是否为`Array`的实例，当然，这是最佳的方法。

``` javascript
var items = [];
console.log(Array.isArray(items)); //true
```

### Primitive Wrapper Types -- 原始封装类型

原始封装类型有三种(`String`、`Number`、`Boolean`)，使得原始类型用起来像对象一样方便。原始封装类型时引用类型，**当读取`String`、`Number`、`Boolean`时，原始封装类型会自动创建**。

``` javascript
var name = "Nicholas";
var firstChar = name.charAt(0); 
console.log(firstChar);	//"N"

//在背后实际是这样的
var name = "Nicholas";
var temp = new String(name);
var fistChar = temp.charAt(0);
temp = null;
console.log(fistChar);	//"N"
```

> 为了能让字符串在`charAt(0)`工作，创建了一个临时对象，这个对象仅仅用于该语句，并在随后被销毁(这个过程也称之为`autoboxing`)。

看以下例子:

``` javascript
var name = "Nicholas";
name.last = "Zakas";
console.log(name.last);	//undefined

//在操作的背后实际上是这样的
var name = "Nicholas";
var temp = new String(name);
temp.last = "Zakas";
temp = null;

var temp = new String(name);
console.log(temp.last);	//undefined
temp = null;
```

> 所以说，添加的新的属性是挂载在临时对象上的，试图访问该属性时，另一个不同的临时对象被创建，而新属性并不存在。

### 详解 

#### Number -- 数字

根据语言规范，`JavaScript`采用`IEEE 754 标准定义的双精度64位格式`来表示数字，所以`JavaScript`不区分整数值和浮点数值。

小心`NaN`(Not a Number)。

```javascript
parseInt ("hello", 10); // 函数返回NaN
NaN + 5；//返回NaN
```

> 使用内置函数`isNaN()`可以判断一个变量是否为`NaN`。`isNaN(NaN);//true`

`JavaScript`还有两个特殊值：`Infinity`(正无穷) 和 `-Infinity`(负无穷)

> 可以使用内置函数`isFinite()`来判断一个变量是否是一个**有穷数**，如果类型是`Infinity`、`-Infinity`、`NaN`则返回`false`。

#### String -- 字符串

`JavaScript`中的字符串是一串`Unicode`字符序列。`JavaScript`字符串是不可更改的。这意味着字符串一旦被创建，就不能被修改。但是，可以基于对原始字符串的操作来创建新的字符串。

#### 布尔类型

False values -- 假值

- `false`
- `0`
- `""` 空字符串
- `NaN`
- `null`
- `undefined`


#### 其他类型

`JavaScript`中`null`和`undefined`是不同的，前者表示一个**空值**`non-value`，必须使用`null`关键字才能访问，后者是`undefined`（未定义）类型的对象，**表示一个未初始化的值，也就是还没有被分配的值。**

`JavaScript`允许声明变量但不对其赋值，一个未被赋值的变量就是`undefined`类型。还有一点需要说明的是，`undefined`实际上是一个不允许修改的常量。

### Data type conversion ###

**JavaScript is a dynamically typed language**. That means you don't have to specify the data type of a variable when you declare it.

> In expressions involving numeric and string values with the `+` operator, JavaScript converts numeric values to strings.

```javascript
x = "The answer is" + 42; //"The answer is 42"
```

**In statements involving other operators, JavaScript does not convert numeric values to strings.**

```javascript
"37" - 7 //30
"37" + 7 //377
```

Functions [`parseInt()`][parseInt] and [`parseFloat()`][parseFloat] is used for converting strings to numbers.


## Literals ##

> Literals are fixed values, not variables, that you literally provide in your script.

- [Array literals](#array-literals)
- [Boolean literals](#boolean-literals)
- [Floating-point literals](#floating-pointing)
- [Integers](#intergers)
- [Object literals](#object-literals)
- [RegExp literals](#regexp-literals)
- [String literals](#string-literals)

### <a name='array-literals'> Array literals </a> ###

An array literal is a list of zero or more expressions, each of which represents an array element, enclosed in square brackets (`[]`).

> Note: [An array literal is a type of object initializer.][object-initializer]

**Extra commas in array literals**

```javascript
	var fish = ["Lion", , "Angel"]; //fish[1] is undefined
```

is the same as:

```javascript
	var fish = ["Lion", undefined,"Angel"];
```

If you include a trailling(尾随) comma at the end of elements, the comma is ignored.

```javascript
	var myList = ['home', , 'school',];
```

> The length of the array is three. And trailing commas can create errors in older browser versions.


### <a name='boolean-literals'> Boolean literals </a> ###

The Boolean type has two literal values: `true` and `false`.

### <a name='intergers'> Intergers </a> ###

- Decimal integer literal consists of a sequence of digits without a leading 0 (zero).
- Leading 0 (zero) on an integer literal, or leading 0o (or 0O) indicates it is in octal. Octal integers can include only the digits 0-7.
- Leading 0x (or 0X) indicates hexadecimal. Hexadecimal integers can include digits (0-9) and the letters a-f and A-F.
- Leading 0b (or 0B) indicates binary. Binary integers can include digits only 0 and 1.

### <a name='floating-pointing'> Floating-point literals </a> ###

A floating-point literal can have the following parts:

- A decimal integer which can be signed (preceded by `+` or `-`),
- A decimal point (`.`),
- A fraction (another decimal number),
- An exponent.

### <a name='object-literals'> Object literals </a> ###
An object literal is a list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`.

> You should not use an object literal at the beginning of a statement. This will lead to an error or not behave as you expect.

```
	var sales = "Toyota";
	function carTypes(name) { ... }
	
	var car = {myCar: "Saturn", getCar: carType("Honda"), special: sales};
```

> The first element of the car object defines a property `myCar`, and assigns to ti a new string value `"Saturn"`. It's called pairs of (property/value). the second element, the `getCar` property, is immediately assigned the result of invoking the function `carTypes("Honda")`; the third element, the `special` property, uses an existing variable `sales`.

Additionally, you can use a numeric or string literal for the name of a property or nest an object inside another.

```javascript
	var car = { manyCars: {a: "Saab", "b": "Jeep"}, 7: "Mazda" };
	console.log(car.manyCars.b); //Jeep
```

Object **property names** can be any string, including the empty string.

> If the property name would not be a valid JavaScript identifier or number, it must be enclosed in quotes. Property names that are not valid identifiers also cannot be accessed as a dot `.` property, but can be accessed and set with the array-like notation `[]`.

```javascript
	var unusualPropertyNames = {
	"!": "Bang!"
	}
	console.log(unusualPropertyNames.!); // SyntaxError: Unexpected token !
	console.log(unusualPropertyNames["!"]); // Bang!
```

### <a name='regexp-literals'> RegExp literals </a> ###

A regex literal is a pattern enclosed between slashes.

```
	var re = /ab+c/;
```

### <a name='string-literals'> String literals </a> ###

A string literal is zero or more characters enclosed in double `"` or single `'` quotation marks.

You can call any of the methods of the String object on a string literal value : JavaScript automatically converts the string literal to a temporary String object, calls the method, then discards the temporary String object.

	console.log("John's cat".length);

#### Using special characters in strings ####

- `\n`
- `\t`
- `\b`
- ...

#### Escaping characters ####

`\` is used as a Escaping characters(转义字符).


[grammar-and-types]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types
[parseInt]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt 
[parseFloat]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat
[object-initializer]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#Using_object_initializers


编辑备注：

+ 2017-01-18第一次编辑
+ 2017-03-21第二次编辑