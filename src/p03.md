## 语句 (Statements)

在上一章中，我们将表达式（expressions）称为 JavaScript 中的短语。用这种说法，我们可以将 JavaScript 中的语句（statements）称为句子。在 JavaScript 中，所有的句子由分号（;）进行结尾。表达式会被用来生产出一个值，而语句的执行才会让各种各样的事件发生。拥有副作用（side effect）的表达式可以单独称为语句。

JavaScript 的程序就是由一句句的语句所组成的，默认情况下，执行顺序是根据程序被编写的顺序。

### 表达式语句（Expression Statements）

表达式语句是 JavaScript 中最简单的语句了，它们就是拥有副作用（side effect）的表达式，比如：

- 赋值表达式
- increment / decrement
- delete
- 调用函数

```js
let a, b，c;
a = 1, b = {x:1}; // 赋值表达式
a--; // decrement
delete b.x; // ture, b 中的 x 属性被删除
console.log(a) // 1 调用 console.log() 函数
Math.max(1,2) // 无意义的函数调用，因为这个函数没有任何副作用
c = Math.max(1,2) // 有意义的函数调用，赋值为其副作用
```

### 复合语句和空语句（Compound and Empty Statements）

在 JavaScript 中，可以通过一对中括号来组合多行语句成为一个复合语句，又称语句块。语句块中的语句由分号结尾，而语句块本身不用。通常在语句块中的内容会被缩进来使得其更容易阅读和理解。空语句正如其名，JavaScript 的编译器不会对其进行任何动作。

```js
{
  let a = 1;
  console.log(a);
} // 语句块

; // 空语句
```

### 条件句法（Conditionals）

条件语句会根据表达式的值而执行或跳过其语句。它们也可以被称为程序的分支（branches）。

#### if 句法

if 句法是 JavaScript 中最基本的用来控制流程的句法。它有两种使用方法，第一种是只使用 if，它会在表达式返回真值时执行；第二种是 if else 其中的 else 会在表达式返回假值时执行。

```js
if (true) {
  console.log('true');
} // "true"

if (false) {
  console.log('true');
}
else {
  console.log('false');
} // "false"

```

#### else if 语句

对于 if else 语句来说，它一共只有两种执行方案，因为布尔值一共只有两种。如果在需要不只两种的方案是该怎么办呢？解决方案之一就是使用 else if 语句。其实 else if 就是将 else 和 if 结合了的编程惯用手法罢了：

```js
if (n === 1) {
  console.log(1);
} 
else if (n === 2) {
  console.log(2);
}
else if (n === 3) {
  console.log(3);
}
else {
  console.log(4);
}
// 等同于
if (n === 1) {
  console.log(1);
} 
else {
  if (n === 2) {
    console.log(2);
  }
  else {
    if (n === 3) {
      console.log(3);
    }
    else {
      console.log(4);
    }
  }
}
```

#### switch 语句

在有许多分支运行时，else if 并不一定会是一个很好的选项，因为很有可能所有的执行都是根据某一个值而定的，这时候就可以使用 switch 句法了。

```js
// 等同于 if else if 中的例子
switch (n) {
  case 1: // 进行 n === 1 比较
    console.log(1);
    break; // 每个 case 后要加 break
  case 2:
    console.log(2);
    break;
  case 3:
    console.log(3);
    break;
  default: // 如果所有的 case 都不符合，执行 default
    console.log(4);
    break;
}
```

### 循环语句（loops）

循环语句，正如其名，会循环执行其中的语句，在 JavaScript 中有五种基本的循环句法，他们将被一一介绍。

#### while 循环

如同 if 会在表达式为真值时执行一样，while 也是在表达式为真值时执行，不同的是，每次 while 中的句法执行完毕时会返回循环的顶部再次对 while 的表达式进行判断。使用 while (true) 可以产生无限循环，但是通常在 JavaScript 中，每次循环都会使某些值发生改变从而使循环不会返回一样的结果:

```js
let count = 0;
while (count < 10) {
  console.log(count);
  count++;
} // 0 ~ 9， 在 count 等于10的时候，while 的表达式判断为假值，结束循环
```

#### do while 循环

do while 循环和 while 循环十分类似，这不过表达式是在句法执行后才进行判断的，而非执行前。这说明循环体会至少被执行一次。

```js
let count = 0;
do {
  console.log(count);
  count++;
} while (count < 10); // 注意要加分号，他不是以句法块结尾的
// 0 ~ 9， 在 count 等于10的时候，while 的表达式判断为假值，结束循环
```

#### for 循环

相较于 while 循环，for 循环更加的简单，他会遵从一个给定的逻辑，大多数为一个计数器之类的值。他会在循环前初始化；循环中的每一遍进行检测；循环后对计数器的值进行增加或减少，他的句法是：

```js
for (initialize; test; increment) {
  statements
}
// 比如
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

#### for of 循环

for of 循环可以对可遍历对象使用，例如数组、字符串、set、map，它们都表示了一列可被循环的元素。for of 是在 ES6 新增的。

```js
// for of 数组每个元素
let data = [1 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9], sum = 0;
for (let element of data) { 
  sum += element;
}
sum // 45

// for of 字符串每个十六位值
let str = 'hello', rev = '';
for (let char of str) { 
  rev = char.concat(rev);
}
rev // "olleh"

// for of set 时会遍历其所有元素

// for of map 时会遍历其所有 key-value pair
let map = new Map([['a',1],['b',2],['c',3]]), keys = '', value = '';
for (let [key, value] of map) { // map 中每个元素都是长度为2的数组，第一个值为 key，第二个值为 value
  keys += key;
  values += value;
}
keys // 'abc'
values // 6
```

但是对象在默认情况下是无法用 for of 遍历的，对其使用 for of 会抛出 TypeError。但是其 keys 却是可遍历的。

```js
let o = {a:1, b:2, c:3};
for(let element of o) { // TypeError 会在这行被抛出，因为 o 无法被遍历
  console.log(element);
}

let keys = '';
for(let key of Object.keys(o)) { // Object.keys(o) 会返回包含所有 keys 的数组，所以可以被遍历
  keys += key;
}
keys // 'abc'

let values = 0;
for(let value of Object.values(o)) { // Object.values(o) 会返回包含所有 values 的数组，所以可以被遍历
  values += value;
}
values // 6
```

#### for in 循环

for in 循环的句法和 for of 循环很像，他只是用 in 代替了 of。不过它们还是有区别的，for of 循环需要一个可遍历的对象，而 for in 可以对任何对象使用，他会遍历对象的属性名。

```js
let o = {a:1, b:2, c:3};
for(let element in o) {
  console.log(element); // 'a', 'b', 'c'
  console.log(o[element]); // 1, 2, 3
}
```

在执行 for in 循环时，JavaScript 编译器会先判断其对象是否为 null 或者 undefined，如果是的话，他会直接跳过整个循环。若不是 null 或者 undefined，则会在每一次循环开始时求出可遍历属性名的值并进行赋值。

在 JavaScript 中，数组其实是一种特别的对象，数组的 index 就是它的属性名，但是在面向数组时，大多是情况下我们都想去使用 for of 而非 for in，因为返回其索引并没有什么实际意义。

```js
let a = ['a', 'b', 'c', 'd'];
for(let i in a) {
  console.log(i); // 0, 1, 2, 3
}
```

for in 循环时也有特例，在对象的某个属性为 symbol 时，他不会被 for in 遍历到；在某个属性 enumerable 为 false 时，也不会被遍历。例如对象内嵌的 toString() 方法就不会被遍历，因为他的 enumerable 属性为 false。默认情况下，程序中新增的属性为可遍历的。

### 跳转语句（Jumps）

跳转语句会使 JavaScript 编译器跳到代码中指定的新地点。

#### 标记语句

任何语句都可以通过在其前面加上一个标签的形式来标记它。

```js
identifier: statement
```

被标记的语句可以被其他跳转语句引用。标签（identifier）不可以使用关键字/保留字。标签的命名空间（namespace）和其他变量、函数不同，所以标签可以和其他变量重复。

#### break

break 语句在单独使用时，可以退出最里层的循环或者 switch 语句，它的句法很简单：

```js
break;
```

正因为它是用来退出循环/switch 的，他只能在这两者内部出现，否则会抛出 SyntaxError。

break 后也可以跟一个标签，如果这么做，它会跳到这个标签标记的语句块之后而不接着执行它。若找不到标签，则会抛出 SyntaxError。

```js
break identifier;
```

#### continue

continue 和 break 十分类似，它也会跳过这一循环，不过 continue 会进行剩下的遍历。同 break 一样，他以可以加上一个标签：

```js
continue;
continue identifier;
```

#### return

在 JavaScript 中，调用函数是一个表达式，而表达式都会返回一个值，所以 return 语句在函数中就是用来表达那个返回的值的，若函数中无 return 句法，返回值为 undefined：

```js
function square(x) {
  return x*x; // 有返回值
}
let a = square(2);
a // 4

function square(x) {
  let a = x*x; // 无返回值
}
let a = square(2);
a // undefined
```

在非函数体内出现的 return 语句会抛出 SyntaxError。

return 通常出现在函数最末尾，因为 return 后的句法都不会被执行。return 也可以不返回任何值，等同于返回 undefined 并且跳过后面的句法。

#### throw 

在 JavaScript 中，异常（exception）会在 runtime error 发生或者编写的 throw error 发生时抛出。
异常可以使用 try/catch/finally 来捕获。throw 的句法如下：

```js
throw expression; //表达式可以为错误代码，或者一段文字描述。
```

Error 对象会在 JavaScript 编译器抛出错误时被使用，它拥有一个 name 属性用来表示错误的类型，还有一个 message 属性用来存放在构造函数中存放的字符串。

当异常被抛出时，JavaScript 编译器会立刻停止程序的运行然后跳到最近的异常捕获器（exception handlers）, exception handler 使用了 try/catch/finally 中的 catch。若 exception handler 未被找到，则会抛出错误并报告给 user。

#### try/catch/finally

try/catch/finally 就是 JavaScript 中的异常处理机制。try 中的语法块会定义其中抛出的异常可以处理，catch 中的语法块则会在 try 中抛出异常时捕获它并且执行，finally 中的语法块则是不论 try 中发生了什么都会被执行的。catch 和 finally 块都是可选的，但是 try 后必须跟随至少其中一个。

```js
try {
  //若无异常，便正常执行
  //若抛出异常，跳到 catch 部分
}
catch (e) {
  // 只会在 try 部分抛出异常时被执行
  // 参数中的 e 可以用来引用 Error 对象挥着抛出的异常值
}
finally {
  // 不论是否有异常，它都会在最后被执行
}
```

### 其他句法

#### debugger

debugger 句法通常情况下不会做任何事，但如果有 debugger 程序正在运行，它便会执行某些 debugging action。在实践中，它和 breakpoint 十分类似：停止程序的执行，检查 call stack，显示变量值等等。

####  “use strict”

"use strict" 是在 ES5 引入的指示（directive）。使用这个指示会使得程序进入严格模式。它可以被用在全局，也可以被用在某一个函数内部等等。即使不手写"use strict"，在 class body 和 模块(module）中也会在严格模式下执行。

严格模式会带来更严格的错误检查和安全性升级。严格模式大体会带来如下变化：

- with 句法禁用
- 变量必须被声明，否则会在对未声明的变量名赋值时抛出 ReferenceError（非严格模式则是在全局对象上隐式的创建这个变量）
- 函数中的 this 指向 undefined 而非全局对戏
- 对无法写入的属性赋值，或对 non-extensible 的对象新增属性时会抛出 TypeError
- eval() 会为其中声明的变量等创建新的作用域
- Arguments 对象只持有静态的参数复制，而非对参数的引用
- 在非严格模式下，delete 返回 false 的情况下，在严格模式中会抛出 SyntaxError
- 单纯使用0开头的八进制数时不允许的，会跑吹 SyntaxError
- eval 和 arguments 也会成为关键字
- 对 call stack 的检查被禁用，所以无法使用 arguments.caller 和 arguments.callee，它们会返回 TypeError

### 声明（Declarations）
如同 const、let、var、function、class、import 之类的关键字虽然它们看上去很像语句（statements），但严格来说它们并非是语句，更确切的说法称他们为声明（declarations）。

声明的作用是为了创建一个新的值并且为这个值添加一个变量名用来引用它。所以声明在运行时和句法不同，它们不会像句法一样在执行时运行，而是定义了程序的结构。粗略的说，我们可以理解为声明是在程序运行前先执行的部分。

#### const，let 和 var

const 用来声明常量，let 和 var 用来声明变量。因为它们作用于的不同，基本上现在所有情况都可以使用 let 而非 var。

```js
const TAU = 2 * Math.PI;
let a = 2;
var b = a * TAU;
```

#### function

function 声明是用定义一个函数的。function 声明会被提前到其语法块的最顶层执行。这就是我们曾说的变量提升（hoisting）。

```js
console.log(double(2)); // 4
function double(x) {
  return x * 2;
}
```

#### class

在 ES6 之后可以使用 class 来声明一个新的 class 并给予他一个名字用来被引用。class 声明并不会进行变量提升。所以不能再声明前引用。

```js
class Square {
  constructor(length) {
    this.length = length;
  }
  area() {
    return this.length ** 2;
  }
  circumference() {
    return this.length * 4;
  }
}

let a = new Square(9);
a.area() // 81
```

#### import 和 export

import 和 export 可以使不同模组间进行值的传递。一个模组是一个拥有其自己的全局命名空间（namespace）的 JavaScript 文件。下面是它们简单的用法，更深入的用法会在之后介绍。

```js
// import
import Square from './Square.js';
import { PI } from './constants.js';
// export
export constTAU = 2 * Math.PI;
export default function a() {...} //
```

### 小结

下面的列表概括了这章出现的所有语句的作用：

- **语句：作用**
- break：从循环或 switch 中退出
- case：switch 中语句的标记
- class：声明一个 class
- const：声明常量
- continue：直接开始下一个循环
- debugger：在 debug 中加一个 breakpoint
- default： switch 中的默认语句
- do while：基础循环结构
- export：声明可被其他模组 import 的值
- for：基础循环结构
- for in：循环对象的属性名
- for of：循环可遍历对象的值
- function：声明一个函数
- if else：根据条件执行语句
- import：声明从其他模组 export 的文件
- label：给语句添加标签
- let：变量声明
- return：从函数返回值
- switch：多分支条件语句
- throw：抛出异常
- try catch finally：处理异常
- "use strict"：进入严格模式
- var：声明变量
- while：基础循环结构