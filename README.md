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