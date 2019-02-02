---
title: 《实战ES2015》读书笔记
date: 2017/03/03 12:46:25
categories:
- Notes
tags:
- ES2105
- ES6
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs83ifsetj20jg0d015w.jpg
---

最近这几天抽空看了一下脸谱书系列中的《实战ES2015》，但是还没看完（其实是懒，逃）。其中的第三章对ES2015中的一些新语法进行了详细的讲解，在这里顺便就参照着书里的知识点总结一下，写个笔记记录一下学习的内容，权当加深一下印象吧！

顺便推荐一下阮一峰老师的ES6教程[ECMAScript6入门](http://es6.ruanyifeng.com/)

这本书更加详细的阐述了ES6的知识点，相较于《实战ES2015》来说更加偏向于理论，也更加详细，是一本值得学习的好书籍，推荐大家去看！喜欢看实体书的童鞋也可以去各大平台购买实体书，支持一下阮老师！！！

## 本文章主要讲解的新语法有：

- let、const和块级作用域
- 箭头函数（Arrow Function）
- 模板字符串（Template String）

## let、const和块级作用域

### 块级作用域

**作用域（Scope）**是`ECMAScript`编程中非常重要的一个概念，虽然它在通报`ECMAScript`编程中并不能充分引起初学者的注意，但在异步编程中，良好的作用域控制技巧则成为了`ECMAScript`开发者的必备技能。

在`ES2015`之前的`ECMAScript`标准中，原本只有**全局作用域**和**函数作用域**。

### let定义变量

在`ES2015`中，`let`可以说是`var`的进行版本，`var`在绝大部分情况下可以被`let`替代。`let`与`var`的异同点大致如下表：

|                    | let  | var  |
| ------------------ | ---- | ---- |
| 定义变量           | ✅    | ✅    |
| 可被释放           | ✅    |   ❌   |
| 可被提示（Hoist）  |   ❌   | ✅    |
| 重复定义检查       | ✅    |  ❌    |
| 可被用于块级作用域 | ✅    |   ❌    |

**重复定义检查**可以用下面这段代码来说明：

```javascript
// var
var foo = 'bar';
var foo = 'abc';
console.log(foo); //=> abc

// let
let bar = 'foo';
let bar = 'def'; //=> Uncaught SyntaxError: Identifier 'bar' has already been declared
```

`var`可以让同一个变量名在同一个作用域里被定义多次，而这种“特性”很可能会导致一些问题。而`let`则加入了代码审查（Code Review）制度，当同一个变量名在同一个作用域内被定义第二次时，便会抛出错误，以警示开发者修改代码。

此外，`let`可被用于**块级作用域（Block Scope）**。

使用`let`形成的块级作用域可以在大部分具有`{ ... }`的语句中使用，例如

- `for () {}`
- `do {} while()`
- `while {}`
- `switch() {}...`

```js
switch(true) {
	default:
		let bar = 'foo';
		break;
}
console.log(bar); //=> undifined
```

### const定义常量

`const`的引入是`ECMAScript`获得了真正的定义常量的能力。

```javascript
// 定义一个常量
const PI = 3.1415926;
// 尝试对该常量进行重新赋值
PI = 3.14; //=> Uncaught TypeError: Assignment to constant variable.
```

变量与内存之间的关系由三个部分组成：

- 变量名
- 内存绑定
- 内存（内存地址）

`ECMAScript`读取变量时，会从变量当前的内存地址所指想的内存空间中读取内容。当用户改变变量的值时，引擎会重新从内存中分配一个新的内存空间以存储新的值，并将新的内存地址与变量进行绑定。const的原理就是在变量名与内存地址之间建立**不可变**的绑定，当后面的程序尝试申请新的内存空间时，引擎便会抛出错误。

除了`let`会产生块级作用域以外，`const`同样可以产生块级作用域，其定义的常量也同样遵循变量在作用域中的生命周期。

### 变量的生命周期

在`ECMAScript`中，一个变量（或常量）的生命周期（Life Cycle）模式是固定的，由两种因素决定：

- 作用域
- 对其的引用

绝大部分`ECMAScript`运行引擎对垃圾数据的收集方式都是基于对变量（或常量）的**引用**进行统计，当一个变量的引用被全部解除时，引擎便会将其认定为应该被清除的。简单表示便是：**一个事物当为人所需要时，便永生不朽；但若被抛弃时，便悄离去。**

如果想延长的变量的生命周期，最为常用的方法便是闭包（Closure）。因为变量的生命周期由对其的引用所决定，二闭包的原理便是利用高阶函数来产生能够穿透作用域的引用。

在`ECMAScript`中，变量（或常量）的生命周期是从程序进入定义语句所在的作用域开始的，即便是在定义语句之前。例如下面这行代码：

```javascript
var foo = 1;
```

我们不妨将这一句拆分成两句，以便更好理解后面的内容。

```javascript
var foo; // Declaration
foo = 1; // Assignment
```

1. `ECMAScript`引擎在进入一个作用域时，会先扫描这个作用域内的变量（或常量）定义语句（`var`，`let`或`const`），然后在这个作用域内为扫描得到的变量名做准备，在当前作用域内被扫描到的变量名都会进入未声明**（Undeclared）阶段**。
2. 进入声明语句时，`var foo;`，即前半句是**声明部分（Declaration）**，用于在`ECMAScript`引擎中产生一个变量名，当此时此刻变量名没有对应的绑定和内存空间，所以“值”为null。
3. `=`的左右是作为变量的赋值语句，引擎执行至此处即为该变量的**赋值部分（Assignment）**，计算将要赋予变量名的值的物理长度（内存空间占用大小），向系统申请相应大小的内存空间，然后将数据存储到里面去，并在变量名和内存空间之间**建立绑定（Binding）关系**，此时变量（或常量）才得到了相应的值。
4. 到当前作用域中的语句被执行完毕时，引擎便会检查该作用域中被定义的变量（或常量）的被引用情况，如果引用已被全部解除，引擎便会认为其应该被清除。
5. 运行引擎会不断检查存在于运行时（Runtime）中的变量（或常量）的被引用情况，并重复第四步，直至程序结束。
   ES2015标准中（一般情况下为严格模式）不允许变量（或常量）在被定义之前被其他语句所读取，以免出现逻辑性错误。

### 更佳体验

从工程化角度上看，我们应该在`ES2015`中遵循以下三条原则：

- 一般情况下，使用`const`来定义值的存储容器（常量）。
- 只有在值容器明确地被确定将会被改变时才使用`let`来定义（变量）。
- 不再使用`var`。

## 箭头函数（Arrow Function）

除了`let`和`const`外，箭头函数是使用频率最高的新特性了。

### 使用语法

1. 单一参数的单行箭头函数

```javascript
// Systax: arg => statement
const fn = foo => `${foo} world` // means return `foo + 'world'`

// 这是箭头函数最简洁的形式，常见于用作简单的处理函数，如过滤。
let array = ['a', 'bc', 'def', 'ghij'];
array = array.filter(item => item.length >= 2); //=> bc, def, ghij
```

2. 多参数的单行箭头函数

```javascript
// Systax: (arg1, arg2) => statement
const fn = (foo, bar) => `${foo} world` // means return `foo + 'world'`
```

在大多数情况下，函数都不会只有一个参数传入，在箭头函数中，多参数的语法跟普通函数一样，以括号来包裹参数列。这种形式常见于数组的处理，如排序。

```javascript
let array = ['a', 'bc', 'def', 'ghij'];
array = array.sort((a, b) => a.length < b.length); //=> ghij, def, bc, a
```

3. 多行箭头函数

```javascript
// Systax: arg => { ... }
// 单一参数
foo => {
	return `${foo} world`;
}
// Systax: (arg1, arg2) => { ... }
// 多参数
(foo, bar) => {
	return foo + bar;
}
```

4. 无参数箭头函数

如果一个箭头函数无参数传入，则需要用一对空的括号来表示空的参数列表

```javascript
// Systax: () => statement
const greet = () => 'Hello World';
```

### this穿透

箭头函数就如同它在`CoffeeScript`中的定义一般，是用于将函数内部的`this`延伸至上一层作用域中，即上一层的上下文会穿透到内层的箭头函数中。

### 程序逻辑注意事项

- 箭头函数对上下文的绑定是强制性的，无法通过`apply`或`call`方法改变。
- 因为箭头函数绑定上下文的特性，故不能随意在顶层作用域使用箭头函数。
- 同样地，在箭头函数中也没有`arguments`、`callee`甚至`caller等对象。
- 如果哟使用`arguments`的需求，可以使用后续参数`...set`来获得参数列表。

### 编写语法注意事项

`ES2015`提供了多行的箭头函数语法，所有在使用单行箭头函数时，请不要对单行的函数体做任何换行，以免出现语法错误。

```javascript
const fn = x 
=> x * 2; // SystaxError
const fn = x => x * 2; // OK
```

参数列表的右括弧、箭头需要保持在同一行内。

```javascript
const fn = (x, y) // SystaxError
=> {
	return x * y;
}
const fn = (x, y) => { // OK
	return x * y;
}
const fn = (x,
			y) => { // OK 
	return x * y;
}
```

单行箭头函数只能包含一条语句。但如果是错误抛出语句（throw）等非表达式的语句，则需要使用花括号包裹。

```javascript
const fn1 = x => x * 2; // OK
const fn2 = x => x = x * 2;  return x + 2; // SystaxError
const fn3 = x => { // OK
	x = x * 2;  
	return x + 2; 
}
const fn4 = x => { throw new Error('some error message'); } // OK
```

若要使用单行箭头函数直接返回一个对象字面量，请使用一个括号包裹该对象字面量，而不是直接使用大括号，否则ECMAScript解析引擎会将其解析为一个多行箭头函数。

```javascript
const ids = [ 1, 2, 3 ];
const users = ids.map(id => { id: id});
//=> Wrong: [ undefined, undefined, undefined ]
const ids = [ 1, 2, 3 ];
const users = ids.map(id => ({ id: id}));
//=> Correct: [ { id: 1 }, { id: 2 }, { id: 3 } ]
```

## 模板字符串

### 使用语法

我们使用普通字符串时会用单引号或双引号阿里包裹字符串的内容。而`ES2015`的模板字符串则需要使用**反勾号（backtick，`）**。

```javascript
// Systax: `string`
const str = `something`
```

支持字符串元素注入

```javascript
// Systax: `before-${injectVariable}-after`
const str = 'str'
const num = 1;
// ...
const str1 = `String: ${str}` //=> String: str
const str2 = `Number: ${num}` //=> Number: 1
// ...
```

支持换行

```javascript
/**
 * Systax: `
 *  content
 *
 */
 
 const sql = `
 	SELECT * FROM Users
 	WHERE FirstName = 'Mike'
 	LIMIT 5;
 `
```

有以下一些字符串字面量，通常用于非打印或特殊用途的字符，如下表所示：

| 字面量   | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| `\n`     | 换行                                                         |
| `\r`     | 回车                                                         |
| `\t`     | 制表符                                                       |
| `\b`     | 空格                                                         |
| `\f`     | 进制                                                         |
| `\\`     | 用于打印`\`                                                  |
| `\'`     | 用于打印``‘`                                                 |
| `\"`     | 用于打印`"`                                                  |
| `\xnn`   | 以十六进制代码`nn` 表示一个字符（其中`n`为`0~F`）。例如`\x41`为`A` |
| `\unnnn` | 以十六进制代码`nnnn` 表示一个`Unicode`字符（其中`n`为`0~F`）。例如`\u03a3`为`∑` |

多行模板字符串会在每一行的最后添加一个`\n`字面量，所以在读取多行字符串的长度时，除最后一行外，每一行的长度都会加1，即增加了`\n`的长度

```javascript
const str = `A
B
C
D` //=> A\nB\nC\nD
console.log(str.length); //=> 7
```

### 注意事项

与普通字符串不一样的是，多行字符串没有两种或两种以上的定义语法，这就意味着它无法向下面第一行代码中普通字符串那样，使用双引号嵌套单引号来表达字符串中的字符串，但可以使用反斜杠来讲需要显示的反勾号转义为普通的字符。

```javascript
const str1 = "Here is the outter string. 'This is a string in another string'";
const str2 = `Here is the outter string. \`This is a string in another string\``;
```

