# [JS犀牛书笔记]1. 类型，值和变量

挖个坑吧，JavaScript 犀牛书英文的第七版（JavaScript: The Definitive Guide, 7th Edition）其实在去年（2020）的五月份就发布了，当时就买了一本回来打算尝尝鲜，不过基于是一本厚厚的英文书，看了没多久就直接把我劝退了。不过现在打算趁着假期好好的读一遍，所以打算顺便写点读书笔记记录一下。话不多说直接开始吧。

## 词法结构（Lexical Structure）

第一章中主要是简单介绍了一下 JavaScript 的历史，发展趋势和一些简单的用法，在这里就不展开了，有兴趣的朋友可以自己去搜一下看看。所以这边直接就从第二章的词法结构开始讲起了。

词法结构是什么意思呢？词法结构就是 JavaScript 中最低嗯的基本规则，其中包括了例如：如何定义变量，如何将注释和程序分开，每一句语句又是如何分开的等等。

### 文本和文字（Text and Literals）

JavaScript是一个区分大小写的语言，这意味着所用的关键字，变量名，函数名等等都是要区分大小写的。

```js
let a = 1; // Ok
Let a = 1; // SyntaxError

let b = 1, B = 2; // Ok
```

### 注释（Comments）

注释可以用两种方法来表示，第一种是//，用来表示单行注释，第二种是/* ... */ 用来表示块状注释。

```js
//这是注释
/* 
这也是注释
这还是注释
*/
```

## 类型，值和变量（Types, Values and Variables）

在 JavaScript 中的种类一共有两种，其中第一种为基本类型（primitive types），第二种为引用类型（object type）。基本类型包括了数字（Number），字符串(String），布尔值（Boolean），空值（null），未定义值（undefined），于ES6新加入的 Symbol 和于ES2020新加入的 BigInt。除此之外，剩余所有的类型均为引用类型，或者说均为对象（Object）。

```js
//primitive types
let a = 1; // typeof a = number
let b = 'hi'; // typeof b = string
let c = true; // typeof c = boolean
let d = null; //typeof d = object * 为object的原因会在等下说到
let e; // typeof e = undefined
let f = Symbol(); // typeof f = symbol
let g = 1n; //typeof g = bigint
```

### 数字（Numbers）

在 JavaScript 中，数字使用由 IEEE754 standard 定义的64位浮点形式来表示的。这使得我们我可以精确的表示从-2<sup>53</sup>一直到2<sup>53</sup>之间的所有整数。如果使用更大的整数的话，可能会因为尾数被截去而失去一部分的精确度。

在JavaScript中，十进制的数字可以直接被一连串的 digits 来表示，整数和浮点数都可以被表示，比如：

```js
1
13
1300000
1.3
133.3333
```

除此之外，我们也可以在数字前加上0x来表示16进制数，加上0b或者0o来表示八进制数，比如:

```js
0xff // 255
0x8f3e // 36670

0b77 // 63
0o11771 // 5113
```

#### 运算方法（Arithmetic）

最基本的运算方法包括了加减乘除：+，-，*，/；也包含了 modulo（求余/求莫）：%；还有求平方：**

```js
3+1 // 4
2.5-1.1 // 1.4
2*3 // 6
8/2 // 4
7%2 // 1
2**3 // 8
```

除了基本的运算方法之外，在 JavaScript 中可以通过 Math Object 进行更多数学方面的操作，以下为一些常用的例子：

```js
Math.pow(2,8) // 256 求平方，等同于 ** 
Math.round(0.67) // 1 四舍五入到最近整数
Math.floor(0.67) // 0 向下取整到最近整数
Math.ceil(0.67） // 1 向上取整到最近整数
Math.abs(-3) // 3 求绝对值
Math.max(1,2,3) // 3 求最大值
Math.min(1,2,3) // 1 求最小值
Math.random() // 0 <= x <1 随机0到1中的数
Math.PI // π 圆周率的值，约为3.141592653589793，PI需大写
Math.E // e 自然对数的底的值，约为2.718281828459045，E需大写
Math.sqrt(25) // 5 求平方根
Math.cbrt(125) // 5 求立方根
Math.hypot(3,4) // 5 求每一个输入值的平方之和的平方根
Math.log(Math.E**100) // 100 求自然对数
Math.log10(100) // 2 求底数为10的对数
Math.fround(1.2）// 1.2000000476837158 精确到最近的32位浮点数
```

JavaScript中的运算也不会因为溢出，或者将一个数除以0而报错。如果以上情况发生了，则会返回 Infinity (无限大）。对 Infinity 进行运算操作依然会返回 Infinity。不过也有一个特例，在以下的特定情况会返回NaN（Not a Number）：

- 在0除以0时
- 在 Infinity 除以 Infinity 时
- 求负数的平方根时
- 在对非数字类型进行数学运算后的，无法转换为数字时

```js
0/0 // NaN
Infinity/Infinity // NaN
Math.sqrt(-3) // NaN
'a' - 'b' // NaN
```

除了在全局对象上的 Infinity 和 NaN 之外，Number Object 也拥有一下常用 properties：

```js
Number.isNaN(x) // 判断 x 是否为 NaN
Number.isFinite(x) // 判断 x 是否有限
Number.isInteger(x) // 判断 x 是否为整数
Number.isSafeInteger(x) // 判断 x 是否为安全整数 (safe integer）
Number.MIN_SAFE_INTEGER // 返回最小的安全整数 -2**53
Number.MAX_SAFE_INTEGER // 返回最大的安全整数 2**53 - 1
Number.EPSILON // 返回两个数的最小差值 2**-52
Number.MAX_VALUE // JavaScript 中可以被表达的最大值，大于它会被转换成 Infinity
Number.MIN_VALUE // JavaScript 中可以被表达的最小值，小于它会被转换成 0
```

NaN 海域哦一个奇特的性质，它并不等于任何值，包括他自己。想要判断一个值是否为 NaN，只能使用 isNaN() 函数来判断：

```js
let a = NaN;
a === NaN // false
a == NaN // false
isNaN(a) // true
```

#### BigInt
BigInt是在ES2020中新加入的数字类型，其可以表示64位的 Integer，BigInt可以通过在整数后加n来，或者使用BigInt（）来声明：

```js
let a = 123n // typeof a = bigint
let b = BigInt(123) // typeof b = bigint
a === b // true
```

普通的数学运算方法也可被用于 BigInt：

```js
1000n + 1000n // 2000n
1000n - 1000n // 0n
1000n * 1000n // 1000000n
1000n / 95n // 10n
1000n % 95n // 50n
2n ** 10n // 1024n
```

但是不可以将 BigInt 和其他类型进行运算，其原因是 Number 和 BigInt 并没有说哪一个会更加 general（笼统），Number 包含了小数，而 BigInt 包含了更多的整数，所以对于返回值无法给出肯定的回答，所以 JavaScript 决定不能将 BigInt 与其他类型混合运算。

```js
100n + 100 // Uncaught TypeError: Cannot mix BigInt and other types
```

但是相反的，比较算法依然可以被正确的执行：

```js
1 > 2n // false
1 < 2n // true
0 == 0n // true
0 === 0n // false
```

### 文本（Text）
在 JavaScript 中，文本用字符串（String）的方式来表示，字符串是无法变更的（immutable）有序的16位值（16-bit value）的序列，每一个16位值通常表示一个 Unicode 字符，字符串的长度就是其中包含的16位值的数量。JavaScript中并不存在类似于 Char 之类表示一个16位值的类型，我们可以通过维护一个长度为1的字符串来表示。

#### 字符串（Stirng）

在 JavaScript 程序中，可以通过单引号，双引号或者反引号来包含住字符来表示。在单引号中，双引号和反引号可以被表达在字符串中，对于双引号和反引号亦是如此：

```js
let a = 'a'; // Ok
let b = "b"; // Ok
let c = `c`; // Ok
let d = 'this is "cool"!'; // Ok
let e = "it is 10 o'clock"; // Ok
let f = `"something", 'something else'`; // Ok
```

与单双引号不同，在反引号之内还可以使用 JavaScript 表达式:

```js
let a = 1 // 1
let b = 2 // 2
let c = `${a} + ${b} = ${a+b}` // 1 + 2 = 3
```

#### 字符串的用法（Working with Strings)

最常用的字符串内建特性之一就是连接字符串。如果我们对两个数字进行 + 运算，它会返回他们的和，而如果对字符串进行 + 运算，则会连接这两个字符串，如果是数字加上字符串的话则会通过字符串的方式连接并返回字符串（因为字符串更加笼统，其包裹了所有的 Number 的字符表达方式）：

```js
1 + 1 // 2
'1' + '1' // '11'
'1' + 1 // '11'
```

字符串也可以通过 === 或者 !== 进行比较， < 或者 > 也可以用于比较字符串，其比较原理是逐一比较字符串中每一个16位值的大小：

```js
'a' === "a" // true
'a' !== "b" // true
'a' > 'b' // false
'aaa' > 'b' // false
```

在 JavaScript 的字符串拥有许多 API，以下是一些常用的：

```js
let s = 'Hello, world';
s.length // 12 字符串长度
s.substring(0,4) // 'Hell' 求子串，第一个参数为子串开始的 index，第二个参数为从此 index 后的不包含在子串中的 
s.slice(0,4) // 同上
s.slice(-3) // 'rld' 取最后三个16位值
s.split(',') // ["Hello", "world"] 以','分割字符串

s.indexOf('l') // 2 寻找第一次出现'l'的位置
s.lastIndexOf('l',2) // 10 寻找最后一次出现'l'的位置
s.startsWith('He') // true 判断字符串是否以'He'开头
s.endsWith('ld') // true 判断字符串是否以'ld'结尾
s.includes('or') // true 判读字符串是否包含'or'

s.replace('hello','hi') // 'hi, world' 替换第一个出现的子串
s.replaceAll('l', 'L') // 'HeLLo, worLd' 替换所有符合的子串
s.toLowerCase() // 'hello, world' 变成小写
s.toUpperCase() // 'HELLO,WORLD' 变成大写

s.charAt(1) // 'e' 根据 index 寻找16位值
s.charCodeAt(1) // '101' 根据 index 寻找16位值的数值
s.codePointAt(1) // '101' 根据 index 寻找16位值的点序列

's'.padStart(3,'!') // '!!s' 在左侧填充！直到长度为3
's'.padStart(3,'!') // 's!!' 在右侧填充！直到长度为3

' s '.trim() // 's' 裁剪两边空格
' s '.trimStart() 's ' 裁剪左侧空格
' s '.trimEnd() ' s' 裁剪右侧空格
```

因为 JavaScript 中的字符串的值是无法更改的（immutable），所以像replace之类修改字符串的函数并不会改变原来的字符串本身，而仅仅只是创建一个新字符串。

我们也可以像对待一个 read-only 数组一样来对待字符串：

```js
let s = 'Hello, world';
s.charAt(1) // 'e'
s[1] // 'e' 可以这种方法代替charAt()
```

### 布尔值（Boolean Values）

布尔值可以用来表示 truth（真理） 或者 falsehood (谬论)， 开或关，是或否。在 JavaScript 中的布尔值一共有两个值，分别是 true 和 false。布尔值通常是在 JavaScript 中比较的结果。布尔值有 toString() 函数，可以用来转换成 'true' 或者 'false'。 

### Null 和 Undefined（Null and Undefined）

null 是一个通常用来表示值的缺失的关键字。对 null 使用 typeof 会返回 'object'， 这表示 null 可以被想象成一个特别的对象用来表示 no object。但通常情况下，null被视为其类型中的唯一成员，可以用来表示没有值的字符串或者数字，大多是语言中都有类似于 JavaScript 中 null，比如 NULL，nil，Node。

undefined 用来表示更深一层的值的缺失，其表示某一变量的值在被查询时还未被初始化，或者是某一个不存在的数组元素、对象属性；在某一个函数不拥有显式的返回值时，undefined 会作为返回值；在函数的参数未被定义时，值也为 undefined。Undefined 是一个全局常量对象，它是其类型中的唯一成员。

undefined 通常表示系统层面，error-like 的值的缺失；null 则用来表示编程层面寻常的值的缺失。

```js
null == undefined // true
null === undefined // false
```

### Symbols

Symbol 是在 ES6 引入的，它用来表示非字符串表示的属性的标识符。JavaScript 中最基本的对象是 unordered collection of properties（未排序的属性组合），每一个属性通常有一个标识符（key）和一个值（value)，而标识符通常为字符串（现在 symbol 也可以作为标识符）。

Symbols 可以用 Symbol() 来构造，这个函数绝对不会返回两个相同的值。所以说用这个方法取得的 Symbol 值是绝对不同的。在构造 Symbol 时，可以传入一个字符串，其会在 toString() 的返回值中出现。

```js
let a = Symbol() 
let b = Symbol()
a == b // false
a.toString() // "Symbol()"

let c = Symbol('notShared') 
let d = Symbol('notShared')
c == d // false
c.toString() // "Symbol(notShared)"
```

在想要定义一个会被广泛使用的 Symbol 的值的时候，可以使用 Symbol.for()。它接受一个字符串作为参数，把 Symbol 绑定在这个字符串上。如果这个字符串未被绑定，这回返回一个新的 Symbol；但如果这个字符串已经被绑定了，它便会返回那个被绑定的 Symbol。也可以通过 Symbol.keyFor() 来返回某一个 Symbol 所绑定的字符串。

```js
let a = Symbol.for('shared')
let b = Symbol.for('shared')
a === b // true

a.toString() // "Symbol(shared)"
Symbol.keyFor(a) // "shared"

let c = Symbol('shared') // 未用 Symbol.for() 构造
c === a // false
```

### 全局对象（The Global Object）

全局对象的主要目的是为了持有一些全局中可用的属性。每当 JavaScript 开始编译，或者说每当一个新的浏览器窗口被打开时，一个新的全局对象就会被创建，它包含了：

- 全局常量，例如 undefined，Infinity，NaN
- 全局函数，例如 isNaN(), parseInt()
- 构造函数，例如 Object(), String(), Date()
- 全局对象，例如 Math 和 JSON

全局对象中定义的属性并不是保留字，他们可以被覆盖，但是并不推荐这么做。在 ES2020 中新增的 globalThis 可以让我们指向全局对象并且访问它的内容。

### 不可变的基本类型和可变的对象引用（Immutable Primitive Values and Mutable Object References）

在 Javascript 中，基本类型（Number，String）和引用类型（Object）有本质上的不同。基本类型无法更改（Immutable），这意味着我们没有办法去改变一个基本类型的值；相反的，引用类型（对象）则是可以被改变的（mutable）。

```js
let s = 'string';
s.slice(0,3); // 返回"str"，但不会改变s的值
s // 依然是"string"
s = s.slice(0,3) // s = "str" 赋值行为会给他一个新值，而不是改变他

let a = []; 
a.push(1); // a新增加了一个元素，他被改变了
a // [1]
```

除此之外，基本类型是通过值来进行对比的：只有两个值是一样的情况下，我们才可以说这是两个一样的值；相反的，引用类型则是通过引用来进行对比的：只有当两个值引用了同一个底层的对象时，他们才是相同的。

```js
let a = 'str'
let b = 'str'
a === b // true

let c = [1,2,3]
let d = [1,2,3]
c === d // false c 和 d 虽然看似相同，但引用着两个不同的数组，所以并不相同

let e = Object()
let f = e 
e === f // e 和 f 引用着同一个对像，所以他们相同
e.name = 'jake' // e = {name: "jake"}
f // f = {name: "jake"} 对于 e 的改动会映射到 f 上
```

在上面 e 和 f 的例子中，我们可以看出来在对对象进行赋值操作时，仅仅只是赋值了其引用值（浅拷贝），他并不会创建一个新的对象。如果你想复制一个全新的对象的话，必须显式的复制对象的每一个属性，或数组的每一个元素（深拷贝）。这样的话，对于原本数组的改动就不会影响新的数组了：

```js
let a = [1,2,3,4];
let b = [];
for (let i = 0; i < a.length; i++) {
	b[i] = a[i];
}
a[0] = 4; // a = [4,2,3,4]
b // b = [1,2,3,4] 对于 a 的改动不会影响 b
```

同理，如果我们想对比两个独立的数组或者对象，我们需要对于其所有的元素或者属性进行对比，下面的例子可以用于对比两个数组：

```js
function areArraysEqual(a,b) {
    if (a === b) { // 如果引用着同一个数组，返回 true
    	return true;
    }
    if (a.length !== b.length) { // 如果长度不同，返回 false
    	return false;
    }
    for (let i = 0; i< a.length; i++) {
    	if (a[i] !== b[i]) {
        	return false; // 如果有元素不同，返回 false
        }
    }
    return true; // 所有元素都相同，返回 true
```

### 类型转换 (Type Conversions）

JavaScript 对于值的类型的需求是十分灵活的，在期待一个布尔值的时候，我们可以填入一个非布尔值，JavaScript 会自动帮我们将其转换为布尔值；相同的，在需要一个字符串时，它会将收到的值，不论是什么，都转换陈字符串；对于数字也是如此：

```js
10 + 'hello' // '10hello' 将数字转换成字符串
'7' * '4' // 28 将字符串转换成数字
1 - 'x' // NaN 将字符串转成陈数字，在无法进行有意义的转换时，返回 NaN
```

下面的表格列出了 JavaScript 是如何进行类型转换的：

|Value			|to String		|to Number		|to Boolean |
|-				|-				|-				|-		    |
|**undefined**	|"undefined"	|**NaN**		|false		|
|**null**		|"null"			|**0**			|false		|
|**true**		|"true"			|1				|-			|
|**false**		|"false"		|0				|-			|
|**""**			|-				|**0**			|**false**	|
|**"1.2"**		|-				|1.2			|true		|
|**"str"**		|-				|NaN			|true		|
|**0**			|"0"			|-				|**false**	|
|**1.2**		|"1.2"			|-				|true		|
|**Infinity**	|"Infinity" 	|-				|true		|
|**NaN**		|"NaN"			|-				|**false**	|

#### 转换和相等 （Conversions and Equality）

JavaScript 中有两种操作符来对比两个值是否相等。首先是严格相等比较（strict equality operator: "==="。这个比较在两个值的类型不同时会直接返回 false 而并不是转换他们。在编程中，这个也应该是绝大部分情况下所用的比较操作。其次，因为 JavaScript 拥有灵活的类型转换，这也存在抽象（非严格）相等比较："=="。这会在比较前先将两个值转换成相同的类型（转换规律如下图），并且在转换后进行全等操作符比较（===）。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64dadbdf5add44ccacf45d59a2b608d7~tplv-k3u1fbpfcp-watermark.image)（来自MDN）

需要注意的是，虽然在我们可以在需要布尔值的时候输入 undefined，然后让其转换成 false，但这并不意味着 undefined == false：

```js
if (!undefined) { // !undefined 被转换成 !false 也就是 true
	console.log("hello")；
} // "hello" 会被输出到控制台

undefined == false // false，不相等
```

#### 显式转换（Explicit Conversions）

虽然 JavaScript 会进行许多自动的类型转换，但某些时候我们仍然会需要显式的转换它们：

```js
Number('3'); // string to number
String(false); // boolean to string
Boolean([]); // objet to boolean
```

除去 null 和 undefined 之外，所有的值都拥有 toString() 方法，其结果在绝大多数情况下等同于 String() 函数。

除此之外，有些时候我们也可以写显式的用 new Boolean()，Number() 或者 String()来创建一个新的对于原始数据的对象包赚，其表现等同于原始值，不过这些已经被遗忘在了 JavaScript 历史的长河中，如今已经没有什么理由去使用他们了。

某些运算符会自动进行隐式的类型转换：

```js
x + '' // 等同于 String(x)
+x // 等同于 Number(x)
x-0 //等同于 Number(x)
!!x // 等同于 Boolean(x)
```

定义在数字上的 toStirng() 方法可以接收一个格外的参数用来表示数的底数（进制），支持从 2 到 36 的数字，忽略小数点后的数：

```js
let n = 17;
n.toString(2); // "10001"
n.toString(8); // "21"
n.toString(16); // "11"
```

有些时候为了作为统计数据，可以使用以下方法：

```js
let n = 12345.6789
n.toFixed() // "12346"
n.toFixed(3) // "12345.679" 只保留小数点后三位
n.toExponential() // "1.23456789e+4"
n.toExponential(4) // "1.2346e+4" 使用指数表示，保留小数点后四位
n.toPrecision(4) // "1.235e+4"
n.toPrecision(11) // "12345.678900" 给定结果的 digits 数量，若不足用指数表示
```

Number(), parseInt(), parseFloat() 方法的异同：

```js
Number('  123.45asd') // NaN 在输入的值为字符串时，只要包含非数字或非空格，就会返回 NaN
parseInt('  123.45asd') // 123 更为灵活，返回整数
parseInt('  123.45asd') // 123.45 更为灵活，返回浮点数

parseInt('111', 2) // 7 可以传入第二个参数来指定底数
```

#### 引用值到基本值的转换 （Object to Primitive Conversions）

对于引用值到基本值的转换不像基本值之间的转换一样简单，其会更加复杂和晦涩。其原因之一是因为 JavaScript 对象有时会包含不止一个基本值。JavaScript 中最基础的对象到基本值的转换算法如下：

- **prefer-string**：这个算法返回原始值，在可以被转换成字符串时，更倾向于转换成字符串
- **prefer-number**：这个算法返回原始值，在可以被转换成数字时，更倾向于转换成数字
- **no-preference**：这个算法无更倾向的原始值，而且 classes 可以定义它们自己的转换方法。对于 JavaScript 内嵌的对象类型而言，Date 使用了 prefer-number，除了 Date 外都使用了 prefer-string。

##### 对象到字符串转换

在由对象转换到字符串的场景下，JavaScript 会在将其通过 prefer-string 算法进行转换，再将转换完的值转换成字符串。

这种转换会在发生在，比如，传入一个对象给内嵌函数作为参数时，这个函数需求的参数字符串时；或者对 String() 函数传入对象时；或者在反引号中插入对象时。

##### 对象到数字转换
在由对象转换到字符串的场景下，JavaScript 会在将其通过 prefer-number 算法进行转换，再将转换完的值转换成数字。

这种转换会在发生在传入一个对象给内嵌函数作为参数时，这个函数需求的参数是数字时；对于绝大多数需要数字来执行的操作符，也会执行这个转换。

##### toString() 和 valueOf() 方法

所有对象都会继承 toString() 和 valueOf() 这两种转换方法。

首先是 toString()，它会返回由字符串表示的对象，但是其默认值却是十分的特别：

```js
let a = {
	name: 'jake',
    age: 22
}
a.toString() // "[object Object]" 
String(a) // "[object Object]" 
```

许多定义类会拥有更加精确的 toString() 方法，比如数组的 toString() 会返回由','链接所有元素的字符串；Date 的 toString() 会返回一个可读的日期和时间；对于函数会返回其 JavaScript 源代码：

```js
let a = [1,2,3,4];
a.toString(); // "1,2,3,4"

let b = () => {
    console.log(1);
}
b.toString();
// "() => {
//    console.log(1);
// }"

let c = new Date(2021,1,12)
c.toString() // "Fri Feb 12 2021 00:00:00 GMT+1100 (Australian Eastern Daylight Time)"
```

其次是 valueOf() 方法，其目的是返回一个用基本值表示的此对象，如果有这么一个基本值存在。不过大多时，对象为许多基本值的组合，所以并没有办法直接通过一个基本值来表示，所以 valueOf() 的默认值会返回其对象本身:

```js
let a = {
    name: 'jake',
    age: '22'
}
a.valueOf() // {name: "jake", age: "22"}
```

### 变量声明和赋值（Variable Declaration and Assignment）

在 JavaScript 中可以通过 let 和 const 来声明变量，在 ES6 之前使用的是 var，不过 var 比较特殊。

#### 使用 let 和 const 声明

在如今主流的 JavaScript中，我们使用 let 和 const 来声明变量，同时尽可能在声明的时候为其赋一个初始值。 如果不为变量赋初始值，变量的值就会是 undefined 直到其被赋值。如果要声明常量，就是用 const 来代替 let。常量的值是无法被改变的，任何尝试去改变的举动会带来 TypeError。通常我们会使用全部大写来声明一个常量。

```js
let num1 = 1;
num1 = 2 // Ok

const NUM2 = 1;
NUM2 = 2; // TypeError
```

#### 变量和常量的作用域（VARIABLE AND CONSTANT SCOPE）

某个变量的作用域是他在被代码中被定义和生效的区域。使用 let 或者 const 声明的变量和常量的定义域是块级的（block scoped）。这意味着它们只在其定义的块内生效。简单地说，如果变量被定义在了一对大括号中{...}，这个变量只能在这对大括号中被访问。当然在声明前对其进行访问也是无效的。

如果变量被定义在了 toplevel，则其是一个全局变量并且拥有全局作用域。

```js
let a = 1;
{ let b = 2; }
a // Ok
b // ReferenceError
```

#### 重复声明

在同一个作用域中重复声明同一个变量/常量名会带来 SyntaxError 。但是在不同的作用域中是可行的（但不推荐）：

```js
let a = 1;
let a = 2; // SyntaxError

let b = 1;
{ let b = 2; } // Ok 但是不推荐
```

#### 声明和类型

在 JavaScript 中，变量或者常量的声明是不予类型挂钩的。一个变量可以持有任何类型的值：

```js
let a = 10;
typeof a // "number"
a = 'ten'; 
typeof a // "string"
```

### 使用 var 声明变量 （Variable Declarations with var）

在 ES6 之前，定义变量的唯一方法就是 var，并且无法定义常量。尽管 var 和 let 很类似，它们有者如下区别：
- var 声明的变量的作用域是函数作用域，let 则是块状作用域。
- 如果在函数外声明 var，会声明一个全局变量。不过其和 let 声明的变量还是有许多差异的。var 声明的变量会被定义为全局对象的属性（var a = 1 等同于 globalThis.a = 1），let则不是全局对象的属性。并且使用 var 声明的变量无法被 delete 操作符删除。
- 用 var 重复声明同一个变量名是合法的。
- var 生命的变量还有一个特性是 hoisting（变量提升）。在 var 声明变量后，它会被提升到函数作用域的最顶部（但是赋值不会被提升）。所以在函数中使用 var 定义的变量是不会报错的，但如果其并未被赋值，值会是 undefined。

在非严格模式，如果不是用 let，var，const 而直接赋值，变量会被定义为全局对象的属性（类似用 var 声明）。不同的是，这样声明的变量可以使用 delete 操作符来删除，但 var 声明的不行。

### 解构赋值（Destructuring Assignment）

解构赋值是 ES6 中新增加的声明和赋值句法。解构赋值的句法中，等号右边为一个数组或对象（有结构的值），等号左边为一个或更多变量名，并且模仿了数组的句法。解构赋值被执行时，右边的结构值会被解构，然后被分配到左边的变量中。

```js
let [a,b] = [1,2]; // 等同于 let a = 1, b = 2
[a,b] = [a+1, b+1]; // 等同于 a = a+1, b = b+1
[a,b] = [b,a]; // 交换 a 和 b 的值
[a,b] // [3,2]
```

解构赋值时返回数组的函数可以更容易地被赋值。解构赋值也并不需要左右的变量数量完全相同，左边多余的变量会被赋值为 undefined，右边多余的值则会被无视：

```js
let [a,b] = [1]; // a = 1, b = undefined
let [a] = [1,2]; // a = 1
```

如果不想获取结构值的每一个值，也是可行的；也可以通过在变量前加三个点(...)来使最后一个变量获得所有剩余值；解构赋值也可被用于嵌套数组（nested arrays）：

```js
let [a, , , ,b] = [1,2,3,4,5]; // a = 1, b = 5
let [a, ...b] = [1,2,3,4,5]; // a = 1, b = [2,3,4,5]
let [a,[b,c],[d,e]] = [1,[2,3],[4,5]] // a=1, b=2, c=3, d=4, e=5
```

其实结构值并非一定要是数组，可遍历对象也是可以的，所以任何可以被 for/of loop 使用的对象都可以作为结构值。

```js
let [first,...rest] = "Hello"; // first = "H"; rest = ["e","l","l","o"]
let {name, age} = {name: "jake", age: 22}; // name = "jake", age = 22
```

对于多层嵌套对象/数组也可以使用解构赋值，不过会变得难以阅读和维护，所以并不推荐。

### 小 结

这一章的要点：

- 如何在 JavaScript 中对字符/数字进行操作和改动。
- JavaScript 中其他基本类型的工作方式：booleans，Symbols，numm，undefined。
- 不可变基本类型和可变的引用类型的区别。
- 显式的和隐式的类型转换。
- 变量/常量的声明和赋值以及其作用域。

## 表达式和操作符（Expressions andOperators）

在 JavaScript 中，表达式是一个可以产出一个值的短语。比如变量名就是一个简单表达式，可以产出它持有的值。复杂的表达式可以通过组合简单的表达式来产出。搭建一个复杂的表达式最简单的方法就是使用操作符来连接几个简单的表达式。

### 主要表达式（Primary Expression）

最简单的表达式被称为主要表达式，它们是独立存在的，不再包含更简单的表达式。在 JavaScript 中，它们通常是一个常量、一个字面量、某些关键字或变量。（字面量 Literal，是直接在程序中出现的数据值）。

字面量：

```js
1.23 // 数字字面量
"hello" // 字符串字面量
```

关键字：

```js
true 
null 
this // 指向当前对象
```

变量和常量：

```js
let num = 1;
const STR = 'a';

num // 可以转换成 num 持有的值的表达式
STR // 可以转换成 STR 持有的值的表达式
num2 // 未被定义，ReferenceError
```

### 对象和数组的初始化（Object and Array Initializers）

对象和数组的初始化的值是一个新建的对象或数组，它们有时也被称为对象文本值或者数组文本值。它们并不是主要表达式，因为它们包含了更多的子表达式用来表达他们的属性或者元素。

首先是数组初始化：

```js
[] // 空数组，无元素
[1,2,3] // 长度为3的数组，三个元素
[1,[2,3]] // 长度为2的数组，第二个元素是一个长度为二的数组
[1, , , ,5] // 长度为5的数组，三个元素的值为 undefined
```

接下来是对象初始化：

```js
{} //空对象
{x:1, y:2} // 有两个属性的对象
{x:{a:1}, y:{b:2}} // 有两个属性的对象，其属性也都是对象
```

### 函数定义表达式（Function Definition Expressions）

```js
let a = function(b) {
  console.log(b);
}
```

### 属性访问表达式（Property Access Expressions）

属性访问表达式可以用来访问一个对象的属性或者数组的元素，有以下两种句法：

```js
expression.identifier
expression[expression]
```

第一种的 expression 是用来指定对象，identifier 是用来指定属性名的。第二种左边的 expression 是用来指定对象或者数组的，右边中括号中的则是一个新的表达式。这个表达式会用来指定想要的属性名或是索引值：

```js
let a = {x:1, y:2};
a.x // 1
a['y'] // 2

let b = [1,2,3];
b.1 // SyntaxError 无法对数组使用第一种方法，原因在下面有介绍
b[1] // 2
```

不论哪一种访问表达式，如果左边的表达式返回了 nukk 或者 undefined，会传出 TypeError。对于第一种方法（expression.identifier），其虽然拥有更简单的句法，但是它有些许的限制：

- 属性值必须是合理的且在程序被编写时就已经知道的。
- 属性值不能包含空格、标点符号
- 属性值不能用数字开头（所以不可以填入数组的 index，对数组只能使用中括号法）

#### 条件属性访问表达式（Conditional Property Access）

在 ES2020 中新加入了两种属性访问表达式，被称为条件属性访问表达式：

```js
expression?.identifier
expression?.[expression]
```

在 JavaScript 中，只有 null 和 undefined 这两个值没有任何属性。如果对它们进行普通属性访问的话，则会抛出 TypeError；如果对它们使用条件属性访问，则不会抛出错误，而是返回 undefined。

比如对于表达式 a?.b，如果 a 为 null 或者 undefined，表达式会跳过任何对 b 的访问的尝试而直接返回 undefined。但是如果 a 为其他值，表达式 a?.b 等同于 a.b（如果 b 不存在，会返回 undefined）。

这种属性访问表达式又是也会被称为 optional chaining，这是因为它对更长的 chained 的属性一样生效。

```js
let a = {b:undefined};
a.b?.c.d // undefined
```

在上面的代码中， a.b? 是 undefined，a.b?.c 也是 undefined，理论上 a.b?.c.d 是访问 undefined 的 c 的属性，应该返回 TypeError；但事实却并非如此，optional chain 会生成不会返回 TypeError 的 undefined。其原因是在执行到 a.b?.c 时，会直接返回 undefined，然后形成了短路，后面的属性就全部会被忽略。

对于 expression?.[expression] 这种条件属性访问表达式，使用方法和上面差不多。

### 调用表达式（Invocation Expressions）

在 JavaScript 中，调用表达式是用来调用（执行）一个函数的语句。

```js
f(0) // f 为函数表达式， 0 为 argument 表达式
```

当调用表达式执行时，函数表达式会先被执行，接着 arguments 表达式会生成一列 argument 的值。如果函数表达式的类型不是函数，会抛出 TypeError。接下来，arguments 的值会被传入参数名，接下来函数本体会被执行。函数的 return 值会成为这个调用表达式返回的值，若无 return 值，返回 undefined。

#### 条件调用表达式（Conditional Invocation）

使用条件调用表达式 ?.() 时，若函数表达式的类型是 null、undefined 或者不是函数，他不会抛出 TypeError，而是直接返回 undefined，后面的函数也不会被执行。

```js
o.m() // 属性访问，o 必须是对象，m 的值必须为函数，否则抛出 TypeError
o?.() // 条件属性访问，调用函数，若 o 为 undefined，返回 undefined，不会抛出 TypeError
o.m?.() // 属性访问，条件调用函数，o 必须是对象，若 m 为 undefined，返回 undefined
```

### 对象构建表达式（Object Creation Expressions）

构建表达式会调用构建函数来生成对象。

```js
new Object()
new Object({a:1})

new Object // 不传参数时可如此简写
```

### 操作符概览（Operator Overview）

操作符会在以下场景被使用：

- 算数表达式
- 对比表达式
- 逻辑表达式
- 赋值表达式等

大多数操作符是由标点符号表示的，比如 + 和 =，不过也有用关键字表示的 delete 和 instanceof。他们和标点操作符类似，只不过句法稍微复杂一些。

完整操作符列表：
其中 A 表示关联性的顺序，L 为从左到右， R 为从右到左；N 为字符数, lvalue 为变量、数组中的一个元素或对象的一个属性。

```js
w = x - y - z; // 减法为 L 从左到右，所以等同于 w = (x - y) -z
y = a ** b ** c; // 求幂为 R 从右到左，所以等同于 y = a ** (b ** c);
```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/67d89250728141399640fcf05cd846a4~tplv-k3u1fbpfcp-watermark.image)
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93d88db8537a468baadfc60767d10973~tplv-k3u1fbpfcp-watermark.image)
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/859faf8f024142e8858e7a10d858aa79~tplv-k3u1fbpfcp-watermark.image)

### 运算表达式 （Arithmetic Expressions）

基础运算表达式包括加减乘除（+-*/），求幂(**)和求余(%)。求幂等同于使用了 Math.pow() 函数。

```js
2**2**3 // 等同于 2**8
-3**2 // SyntaxError，必须要加括号

5%2 // 1
-5%2 // -1 会保留正负性
6.5%2.1 // 0.2 也可对小数使用
```

#### 加法表达式（+）

需要格外解释的是加法：

- 如果有操作数（operand）是对象，会对其先使用对象转基本类型算法
- 接下来，如果有任意一个操作数是字符串，则会将剩下一个转换成字符串然后进行链接
- 除此以上情况外，将会把操作数转换成数字并进行数学运算


```js
1 + 2 // 3
'1' + '2' // "12"
1 + {} // "1[object Object]"
true + true // 2
2 + null // 2 
1 + 2 + '3' // => "33"
```

#### 一元表达式（Unary Arithmetic Operators）

在 JavaScript 中，一元表达式具有更高的优先级并且都是从右到左关联的。而且它们会在必要时将它们的单个操作数转换成数字。

- **Unary plus（+）**

	将操作数转换成数字并且返回，如果操作数已经是数字，他不会做任何事，无法对 BigInt 使用。
    
- **Unary minus（-）**

	在 Unary plus 的基础上改变符号。

- **增加（++）**

	对操作数的值加一。操作数必须为 Ivalue（变量、数组中的一个元素或对象的一个属性），它会被先转换成数字在进行增加。返回值的大小取决于 ++ 操作符位于操作数的左边（返回增加后）还是右边（返回增加前）。
    
- **减少（--）**

	对操作数的值加一，具体方法同上。
    
```js
+'123' // 123
-'123' // -123

let a = 1;
let b = a++; // b = 1, a = 2

let c = 1;
let d = --c; // d = 0, c = 0
```
    
#### 位运算（Bitwise Operators）

位运算会 perform 更底层的，对于位（bits）的改动。虽然不同于传统数学运算，它们依然需要一个数字操作数，并返回一个数字。大部分情况下，位运算并不常被使用。

位运算会期待一个整数操作数，并基于操作数为32位整数（而非64位浮点数）来进行运算。操作数会在必要时被转换成数字，并且强制删除其小数部分，然后转换成32位整数。值得一提的是，NaN，Infinity，-Infinity 都会被转换成0。除了 >>> 操作符之外，其他均可被数字和 BigInt 操作数使用。

- **Bitwise AND (&)**

	按位对每一个 bit 进行 and 运算，只有两个操作数的同一个 bit 都为1时才会设定返回数的同位 bit 为1，否则为0。
    
- **Bitwise OR (|)**
	按位对每一个 bit 进行 or 运算，只有两个操作数的同一个 bit 中有至少一个为1时才会设定返回数的同位 bit 为1，否则为0。
    
- **Bitwise XOR (^)**
	按位对每一个 bit 进行 exclusive or 运算，只有两个操作数的同一个 bit 不同时时才会设定返回数的同位 bit 为1，否则为0。
    
- **Bitwise NOT (~)**
	一元操作符，对操作数的每一个 bit 进行转换。等同于转换其符号后减去1。

  ```js
  0x1234 & 0x00FF // 0x0034
  0x1234 | 0x00FF // 0x12FF
  0xFF00 ^ 0xF0F0 // 0x0FF0
  ~0xF // 等同于~0x0000000F，结果是0xFFFFFFF0，或者说-16
  ```

- Shift left (<<)
	
    '<<' 操作符将左侧操作数中所有 bits 向左移动。移动的次数等同于右边的操作数。 (右边的操作数只支持0到31，所以它会被先转换成32位整数，然后截取其最后五位）。向左移动一次等同于乘2，移动两次等同于乘4，以此类推。
    
 - Shift right with sign (>>)
 	
    '>>' 操作符则会向右移动，并且保留符号（正数最高位是0，负数最高位是1），在移动是根据最高位填入新的最高位的值。右移正数等同于除以二然后向下取整，但负数不是。
    
- Shift right with zero fill (>>>)
	
    '>>>' 操作符类似于 '>>' 只不过在最高位只会填入0，而不关心操作数的正负性。
    
  ```js
  7 << 2 // 28
  -7 << 2 // -28

  7 >> 1 // 3
  -7 >> 1 // -4

  7 >> 1 // 3
  -7 >> 1 // 2147483644
  ```
  
### 关系表达式（Relational Expressions）

关系表达式一定会返回布尔值。

#### 等于操作符

=== 为全等操作符，操作数必须要严格相等才返回 ture， == 为相等操作符，会先对操作数进行类型转换然后对比。!== 和 != 会返回相应的 opposite value。多数情况应该使用 === 而非 ==。

#### 对比操作符

会先进行类型转换再对比，只有字符串和数字可以被对比。>= 或者 <= 并不会使用 == 或者 === 来对比，而是简单的 < 和 > 的 opposite value。

#### in 操作符

如果右边操作数（对象)包含左边操作数（属性名），返回 true。

```js
let a = {x:1, y:2}
'x' in a // true
'z' in a // false
```

#### instanceof 操作符

如果左边的操作数（对象）是右边的操作数（class of object）的实例，返回 true。

```js
let a = nwe Date();
a instanceof Date // true
a instanceof Object // true
a instanceof String // false
```

如果右边的操作数不是 classs of object，会抛出 TypeError。

### 逻辑表达式（Logical Expressions）

逻辑表达式包含了&&、||、!，它们分别表示逻辑和、逻辑或、逻辑非。

- Logical AND (&&)，一共有三层理解：
  - 第一层为简单的布尔值，会返回布尔值结果
  - 第二层为非布尔值，会将它们转换成真值（truthy）或假值（falsy）并返回
  - 第三层是真值（truthy）或假值（falsy）实际上的值：若左边的操作数为假值，会直接返回这个值；若左边是真值，则会返回右边的真假值。
  
    ```js
    let o1 = {a:1};
    let o2 = null;
    o1 && o1.a // 1，因为 o1 返回真值，他被无视了，返回的是 o1.a 的真值
    o1.a // 1
    o2 && o2.a // 因为 o2 返回假值，后面便不会执行了， o2.a 被无视了
    o2.a // TypeError
    ```
- Logical OR (||)，也是一共有三层理解：
  - 第一层为简单的布尔值，会返回布尔值结果
  - 第二层为非布尔值，会将它们转换成真值（truthy）或假值（falsy）并返回
  - 第三层是真值（truthy）或假值（falsy）实际上的值：若左边的操作数为真值，会直接返回这个值；若左边是假值，则会返回右边的真假值。
  
    ```js
    let a = b || 100; // 优先使用 b，若 b 的值为假值（比如0），再去使用 100
    ```
    
- Logical NOT (!)，一元操作符，位于操作数的前面，一定会返回布尔值。它会先将操作数转换成布尔值，再取其 opposite value。

### 赋值表达式（Assignment with Operation）

赋值表达式左边的操作数应为一个 lvalue（变量、数组元素、对象属性），右边的操作数为一个任意属性下的任意值。

除了简单的 = 赋值表达式外，这里还有许多其他的赋值表达式:
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f800753f4dca44d58091c99dc12c6530~tplv-k3u1fbpfcp-watermark.image)

### 其他的表达式

#### 条件表达式（?:）

这是 JavaScript 中唯一的一个三元表达式，所以条件表达式也被称为三元表达式。

```js
x > 0 ? x : -x // 求绝对值

if (x > 0) {
  return x;
} 
else {
  return -x;
} // 等同于上面的条件表达式
```

####  First-Defined（??）

?? 类似于逻辑或 ||，只不过 || 返回第一个真值，而 ?? 返回第一个 defined 的值。

```js
0 || 1 // 因为0是假值，返回1
0 ?? 1 // 因为0是个被 defined 的值，返回0

'' || 'hi' // 因为""是假值，返回"hi"
'' ?? 'hi' // 因为""是个被 defined 的值，返回""

false || true // 因为第一个值是 false，返回第二个值 true
false ?? true // 因为 false 是个被 defined 的值，返回 false
```

#### typeof 操作符

typeof 是个一元操作符，放在单个操作数之前，然会返回它的类型（用字符串表示）。

注意：typeof null 会返回 "object"；typeof function 会返回 "function"。

#### delete 操作符

delete 是一个用来删除对象属性或者数组元素的一元操作符。操作数不为 lvalue 时不进行任何操作而直接返回 true，删除失败时返回 false，

### 小结

这一章的要点：

- 表达式是 JavaScript 程序中的短语
- 任何表达式都可被 evaluated 成为一个值
- 表达式也可以拥有除产生一个值之外的其他作用（如变量赋值）
- 基础表达式可以被结合用来生产复杂的表达式
- JavaScript 中有许多操作符用于运算
- '+' 操作符可以用来进行数字相加或字符串链接
- 逻辑 && 和 || 表达式有"短路”效应，有时候之有一个 argument 会被 evaluate