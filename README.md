# JS犀牛书英文第七版笔记

挖个坑吧，JavaScript 犀牛书英文的第七版（JavaScript: The Definitive Guide, 7th Edition）其实在去年（2020）的五月份就发布了，当时就买了一本回来打算尝尝鲜，不过基于是一本厚厚的英文书，看了没多久就直接把我劝退了。不过现在打算趁着假期好好的读一遍，所以打算顺便写点读书笔记记录一下。话不多说直接开始吧。

同步更新于：

- [掘金](https://juejin.cn/user/1407794523416350)
- [知乎](https://www.zhihu.com/people/zhu-yu-lei-9)

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

## 语句 (Statements)

在上一章节的中，我们将表达式（expressions）称为 JavaScript 中的短语。用这种说法，我们可以将 JavaScript 中的语句（statements）称为句子。在 JavaScript 中，所有的句子由分号（;）进行结尾。表达式会被用来生产出一个值，而语句的执行才会让各种各样的事件发生。拥有副作用（side effect）的表达式可以单独称为语句。

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

## 对象（Object）

对象使 JavaScript 中最基本的数据类型。对象是一个复合值，这意味着它结合了许多值（包括基本值或别的对象），对象也允许我们根据名字去存放或者取回这些值。

对象是一个未排序的属性（properties）组合，每个属性都有一个名字和值。属性名通常为字符串，不过也有 symbol 作为属性名的。在对象中除了 name-value pair 之外还会继承其原型（prototype）的属性。对象的方法就是典型的继承属性。

JavaScript 对象是动态，属性可以经常被删除和增加。我们也可以通过忽略对象中的值的部分来表示一个字符串的 set。

任何非字符串、数字、符号、布尔值、null、undefined的值都是对象。虽然字符串、数字、布尔值并非对象，它们也可以表现成一个无法变更的对象。

同一个对象中不可以拥有一样的属性名，但属性名可以是以一个空字符串。值可以是任何 JavaScript 的值，或者是一个 getter 或 setter 函数。

某些时候，区分在对象直接定义上的属性和在原型中继承的属性是有必要的，所以 JavaScript 中用私有属性（own property）来指向非继承的属性。

除了名字和值以外，每一个属性还有三个属性特性（property attributes）：

- writable attribute（可赋值性） 用来指定这个属性的值是否可以被 set
- enumerable attribute（可枚举性） 用来指定这个属性是否会在 for/in 循环中被返回
- configurable attribute（可改变性） 用来执行这个属性是否可以被删除，和这个属性的特性（attribute）可否被修改

大多数 JavaScript 内嵌对象拥有有 non-writable、non-enumerable、non-configurable 特性的属性。但在默认情况下，手动创建的对象的属性是 writable、enumerable、configurable 的。

### 创建对象

在 JavaScript 中一共有三种方法可以创建对象，它们会在下面被介绍。

#### 对象字面量（object literals）

创建对象最简单的方法就是使用对象字面量了。简单的说，对象字面量就是在一对大括号中，用逗号来分割每一对 name: value pair：

```js
let o1 = {}; //	空对象
let o2 = {a: 1, 'b': 2}; //两个属性的对象
let o3 = {
  '': 1,
  o2: o2
} // 两个属性的对象，第一个属性名为空字符串，第二个属性值为对象
```

#### 使用 new 来创建对象

使用 new 可以初始化一个新的对象，new 之后必须跟随一个函数调用。这种情况下，使用的函式会调用构造函数（constructor）来创建新对象。对于内嵌属性，它们都拥有构造函数：

```js
let o = new Object(); // 等同于 {}
let a = new Array(); // 等同于 []
let d = new Date();
let m = new Map();
```

除了内嵌函数外，我们也可以为我们新建的对象定义构造函数。

#### 原型（prototypes）

在介绍第三中创建对象的方法前，要先介绍一下原型。在 JavaScript 中，几乎所有的对象都和另一个对象有着关联，这另一个对象就是原型。对象会从原型上继承属性。

所有使用第一种方法，对象字面量，创建的对象都拥有一样的原型对象，我们可以通过 Object.prototype 来引用这个原型对象。

而使用 new 和 构造函数调用而生成的对象会使用其构造函数中的原型属性的值作为原型。

> 上面的原文是这样的： Objects created using the new keyword and aconstructor invocation use the value of the prototype property ofthe constructor function as their prototype.

所以使用 new Object() 来创建的对象会继承 Object.prototype 作为其属性，等同于用 {} 创建的。 同样的，使用 new Array() 创建的数组会继承 Array.prototype 作为其属性。

所以在 JavaScript 中，绝大多数对象都会有一个原型，只有极少一部分对象才会拥有原型作为属性。正是这极少一部分拥有原型作为属性的对象是被用来定义其他对象的原型。

Object.prototype 是极少数没有原型的对象之一（引用其原型 Object.prototype.\__proto\__ 会返回 null），他不会从任何对象上继承任何属性。绝大多数内嵌构造函数会拥有 object.prototype 作为其原型，例如 Date.prototype.\__proto\__ === Object.prototype 会返回 true。这种链状的继承被称为原型链（prototype chain）。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/51aeaa6ef9d34143ad675055de427e3c~tplv-k3u1fbpfcp-watermark.image)

### Object.create()

第三种创建对象的方法是使用 Object.create()，他会将其第一个参数作为对象的原型

```js
let o1 = Object.create({x:1, y:2});
o1.__proto__ // {x:1, y:2}
o1.x + o1.y // 3
```

传入参数时也可以传入 null，可以通过这样来创建一个没有原型的对象。这意味着他不会继承任何东西，包括像 toString() 之类的方法。

```js
let o2 = Object.create(null);
o2.__proto__ // undefined
```

如果想要创建一个普通空对象，则需要在参数传入 Object.prototype：

```js
let o3 = Object.create(Object.prototype); // 等同于使用 {} 或 new Object() 创建
o3 // {}
o3.__proto__ === Object.prototype // true
```

### 查询和修改属性（Querying and Setting Properties）

我们可以使用 dot(.) 或者中括号([ ])来查询一个属性的值，在 dot 之后必须是属性名，而 [ ] 中必须是可以转换成字符串或 symbol 的值，这个字符串就是想要的属性名。若想要进行赋值，只需要把它们放在赋值表达式的左手变就行了

```js
let author = book.author; // 查询 book 中 author 的属性
let author = book{"author"]; //	同上

book.author = "jake"; // 对 book 中 author 的值进行修改
book["author"] = "jake"; // 同上
```

#### 将对象联想为数组（Objects As Associative Arrays）

在上面的例子中，使用 dot 和中括号都可以用来查询属性。

第一种句法，使用 dot，更像在 C 或者 Java 中访问一个静态对象。第二种句法，使用中括号，更像是进行了数组的访问，只不过将索引替换成了字符串。所以在 JavaScript 中，对象时关联数组（associative array）。

在 C，C++，Java 等强类型语言中，对象只能拥有指定数量的属性，而且属性名需要被定义。但是 JavaScript 则是弱类型语言，上述的描述并不对 JavaScript 成立，对象可以新增任意数量的属性。

当使用 dot 来访问属性时，其之后需要的是一个标签，而非任何的数据类型，所以它们并不能被程序操作，所以想要访问在程序执行时定义的属性名就会变得困难。

但是使用 [ ] 就不会有这样的问题，因为它期待一个字符串，而字符串时一个数据类型，可以被程序操作。所以在 runtime 产生的属性通常由 [ ] 来访问。 

```js
for (let i = 0; i < 3; i++) {
  console.log(customers[`name${i}`]);
} // 这个例子会返回 customers 对象中的 name1、name2、name3属性
// 我们发现只能通过 [ ] 来操作，因为无法将数据类型转换成标签被 dot 使用
```

#### 继承（Inheritance）

假如我们需要查询某一个对象 o 中的某一个属性 x 时，如果 o 中并没有名字为 x 的私有属性时，查询会继续到 o 的原型，去查看其原型中是否有 x 这个属性。如果还是查不到，但是 o 的原型还拥有原型，则查询会继续下去，以此类推，知道 x 被找到或着达到了拥有 null 作为原型的对象。

```js
let o1 = {x:1}; // 从 Object.prototype 继承
let o2 = Object.create(o1) // 从 o1 继承
o2.y = 2;
let o3 = Object.create(o2) // 从 o2 继承
o3.z = 3;
o3.x + o3.y +o3.z // 6
```

注意，原型链只有在访问时会一级一级向上查询，而在赋值属性时，不论原型链是否有这个属性名，其将被赋值在当前实例中。

#### 属性访问错误（Property Access Errors）

在访问不存在的属性名时，并不会抛出 error，而是返回 undefined。但是访问一个不存在的对象的属性会抛出 ReferenceError；访问 null 或者 undefined 的属性则会抛出 TypeError。

对 null 或者 undefined 的属性进行赋值会抛出 TypeError，除此之外，赋值也有失败的时候：在对 non-writeable 的属性进行赋值时；在对不允许新增属性的对象赋值时。在严格模式下，在 set 属性失败时就会返回 TypeError，而非严格模式下这个错误一般是无声的。

### 删除属性（Deleting Properties）

使用 delete 操作符可以从一个对象上删除一个属性，他不会操作那个属性的值，而是直接删除那个属性本身

```js
delete book.author;
delete book["title"];
```

delete 操作符只能删除私有属性，继承的属性无法被删除。它也不能删除 non-configurable 的属性，例如某些内嵌对象，因为它们是全局对象的属性。

### 检验属性（Testing Properties）

首先，我们可以使用 in 操作符来检验一个属性名是否属于一个对象。他会检验私有属性和继承属性：

```js
let o = {x:1};
'x' in o; // true
'y' in o; // false
'toString' in o // true
```

然后，我们也可以i使用 hasOwnProperty() 方法来检验一个对象是否有某一个私有属性，对于继承属性，他会返回 false：

```js
let o = {x:1};
o.hasOwnProperty('x') // true
o.hasOwnProperty('y') // false
o.hasOwnProperty('toString') // false
```

我们也可以使用 propertyIsEnumerable() 方法来增强 hasOwnProperty() 方法，它会先进行 hasOwnProperty() 判断，再在结果上判断属性是否 enumerable：

```js
leto={x:1};
o.propertyIsEnumerable("x") // true 
o.propertyIsEnumerable("toString") // false，不是私有属性 
Object.prototype.propertyIsEnumerable("toString") // false，这个方法是 non-enumerable 的
```

### 遍历属性（Enumerating Properties）

在前面循环的部分提到过，可以使用 for in 来遍历对象的每一个属性名，但是对于 non-enumerable 的属性则不会被遍历到。除了 for in 我们还可以使用 for of 来遍历和对象属性相关的数组：

- Object.keys() 返回一个包含每一个 enumerable 属性名的数组
- Object.getOwnPropertyNames() 返回一个包含每一个属性名的数组，不论属性是否 enumerable
- Object.values()返 回一个包含每一个 enumerable 属性的值的数组
- Object.getOwnPropertySymbols() 返回一个包含每一个属性名为 symbol 的属性组成的数组，不论属性是否 enumerable
- Reflect.ownKeys() 返回全部私有属性，不论是否 enumerable，为字符串或者 symbol

#### 遍历顺序

在 ES6 中，私有的，enumerable 的，属性的遍历顺序被正式定义了。其顺序遵循以下条件：

- 非负整数的字符串属性会最先被列出，排序是从小到大的数字顺序。这意味着数组或者类数组对象可以顺序的遍历其属性（元素）
- 在所有雷素与数组索引的属性被先列出后，剩下的所有拥有字符串作为属性名的属性将会被列出（包括负整数，浮点数）。它们会根据加入对象时的顺序被排序。
- 最后，用 symbol 作为属性名的属性才会被列出。它们的顺序也是根据加入对象时的顺序。

在使用 for in 遍历时，其除了会遍历私有属性之外还会遍历继承属性。其顺序会先遍历私有属性（根据如上的顺序），接下来会沿着原型链往上，以此方法遍历每一个原型对象。不过已经出现过的属性名将不会被再次返回，即使那个属性名是 non-enumerable 的。

### 为对象增加属性（Extending Objects）

在 ES6 中新增了 Object.assign() 方法，他期待两个或更多对象作为其 arguments。这个方法会修改并返回第一个对象（被成为 target object）。他不会对除第一个之外的对象（被成为 source objects）进行改变。

对于每一个 source objects，其中的每一个可遍历私有属性都会被复制到 target object 中。对 source objects 的顺序是从左到右的，这意味着 source objects 中第一个对象会 override target object 中同名属性， 第二个则会改写第一个，以此类推。 

```js
let o1 = {x:1,y:2};
o2 = Object.assign({x:2},o1); // o2 = {x:1,y:2}, x:2 的属性被 override 了
let o3 = {x:3,z:3};
o4 = Object.assign({},o2,o3) // o4 = {x:3,y:2,z:3} x:1 被 x:3 override，z:3 被添加
```

### 序列化对象（Serializing Objects）

Objects Serializing 是将对象的状态转换成字符串的过程。这个字符串也可以重新被转换成对象。

JSON.stringify() 和 JSON.parse() 分别被用于序列化和恢复一个 JavaScript 对象。这两个函数使用了 JSON 数据交换格式，JSON stand for 'JavaScript Object Notation'，他的句法和 JavaScript 的对象和数组十分接近。

```js
let o = {
  x: 1,
  y: {
    z: [false, null, '']
  }
}; // o 是对象
let s = JSON.stringify(o); // s 是字符串： "{"x":1,"y":{"z":[false,null,""]}}"
let p = JSON.parse(s); // p 是对象
```

JSON 的句法其实是 JavaScript 句法中的子集，它并不能表示所有 JavaScript 的值。对象、数组、字符串、有限数字、布尔值和 null 在 JSON 中支持，它们可以被序列化和解析。

Date 对象会被 serialized 成为 ISO-formatted date 字符串，但是在解析时不会返回 date 对象，而是解析成一个而字符串。

函数、正则表达式、Error 对象、undefined 时不可以被序列化和解析的。

在序列化对象是，只有 enumerable 的私有属性会被序列化。

### 对象上的方法（Object Methods）

正如前面所说，所有 JavaScript 对象都会从其原型继承属性。（除了显式创建的无原型对象）那些被继承的属性被称为主要方法（primarily methods）。这节会介绍一下这些方法

#### toString() 方法

toString() 方法不接受 arguments，它会在被调用时返回表示对象的值的字符串。这个方法会在当一个对象需要被转换成字符串的时候被自动调用。我们也可以为对象定义私用的 toString() 方法。

```js
let a = {}.toString(); // "[object Object]"
let b = 'a' + a; // b = "a[object Object]"
```

如上可见，这个方法的默认返回值并不是很有用，所以许多 class 定义了它们自己私有的 toString() 方法。比如数组就重新定义了私有的 toString() 方法；函数的 toString() 会返回其源代码。

#### toLocaleString() 方法

除了基础的 toString() 之外，所有对象都拥有 toLocaleString() 方法。这个方法的目的是返回一个本地化的（localized）字符串来表示对象。默认的 toLocaleString() 本身不会做任何本地化的措施，它们只是简单的调用 toString() 方法并返回值。Date 和 Number classes 定制了其版本的 toLocaleString()。在尝试根据本地情况格式化 Date、Number和 times 是会调用 toLocaleString() 方法。

```js
let n = 1234567;
n.toString(); // "1234567"
n.toLocaleString(); // "1,234,567"

let d = new Date();
d.toString(); // "Mon Feb 15 2021 12:43:32 GMT+1100 (Australian Eastern Daylight Time)"
d.toLocaleString(); // "2/15/2021, 12:44:26 PM"
```

#### valueOf() 方法

这个方法和 toString() 类似，只不过会在需要将对象转换成非字符串的基本值时被调用，比如转换成数字时。默认的 valueOf() 方法并会做什么特别的事情，但是某些内嵌对象有它们私有的 valueOf() 方法。例如 Data class 会转换成数字，然后就可以进行时间顺序的大小比较。我们也可以自己定义 valueOf() 方法：

```js
let a = {
  x:1,
  y:2,
  valueOf: function() {
    return this.x + this.y;
  }
}
a.valueOf() // 3
```

#### toJSON() 方法

Object.prototype 上其实并没有定义 toJSON() 方法，但是当使用 JSON.stringify() 方法来序列化对象时，他会去寻找 toJSON() 方法。如果这个方法存在，会调用这个方法来取得这个方法返回值的序列化后的字符串。

### 更多的对象字面量句法（Extended Object Literal Syntax）

最近的 ES 更新中带来许多更新、更方便的对象字面量句法：

#### 属性简写（Shorthand Properties）

直接看例子：

```js
let x = 1, y = 2;
let o = {x:x, y:y}; // 在 ES6 之前，为对象添加同变量名的属性需要重复一次
let o = {x,y}; // 在 ES6 之后，则可以使用这样的简写，效果和上面一样
```

#### 计算生成的属性名（Computed Property Names）

在 ES6 之前，如果某些时候想要生成一个对象，但是对象的属性名并不是一个我们可以直接输入文本的编译时常量，而是一个通过变量保存的值，或者通过函数返回的值。所以基础的对象字面量法就不可用了。这种时候我们就必须先创建对象，再手动添加属性：

```js
let prop_name1 = 'p1';
function prop_name2() {
  return 'p' + 2;
}
let o = {};
o[prop_name1] = 1; 
o[prop_name2()]= 2; 
o // {p1: 1, p2: 2}
```

但是在 ES6 之后就简单多了，我们可以使用计算生成的属性名（Computed Property Names）这一特性来简化成对象字面量

```js
let prop_name1 = 'p1';
function prop_name2() {
  return 'p' + 2;
}
let p = {
  [prop_name1]: 1,
  [prop_name2()]: 2, 
};
p // {p1: 1, p2: 2}
```

#### Symbol 作为属性名（Symbols as Property Names）

在 ES6 中，属性名也可以是一个 Symbol 了，基于上面的 Computed Property Names，我们也可以用变量或者常量来储存 Symbol 了。

```js
const extension = Symbol('extensions');
let o = {
  [extension]: {...}
}
o // {Symbol(extensions): {...}}
```

正如前面对 Symbol 的介绍，它们是不透明的值，除了将它设成属性名之外，我们不能对它做任何事。Symbol 存在的意义就是为了定了安全的对象 extension。就比如在从第三方获取对象时，而我们不知道他的属性；如果我们要加入新的属性，使用 Symbol 可以确保我们不会 override 原对象中的任何属性。同样的，第三方的代码也无法获取到你通过 Symbol 作为属性名的属性。

#### 展开操作符（Spread Operator）

在 ES8 中新加入了展开操作符（...），在复制属性时，我们可以通过使用它来展开对象的私有属性：

```js
let o1 = {x:1, y:2};
let o2 = {z:3, w:4};
let o3 = {...o1, ...o2};
o3 // {x: 1, y: 2, z: 3, w: 4}
```

虽然被称为操作符，但是展开操作符并不是一个操作符，他是一个只能在对象字面量中使用的特殊的句法。

#### 方法简写（Shorthand Methods）

当一个函数被定义成了一个对象的属性时，我们称之为方法；在 ES6 之前在对象字面量中定义方法时会使用到函数定义表达式；而在 ES6 之后，则可以使用更简单的写法：

```js
// ES6 之前
let square = {
  area: function () { return this.side ** 2 },
  side : 10
}
square.area() // 100

// ES6 之后
let square = {
  area() { return this.side ** 2 }, // 注意我们省略了冒号和关键字 function，但和上面无差别【
  side : 10
}
square.area() // 100
```

#### 属性的 Getters 和 Setters

目前我们提到的所有的属性都是一个用有属性名和值的数据属性，JavaScript 中其实还支持访问属性（accessor properties）。访问属性不拥有单一值，而是拥有两个访问方法：getter and/or setter。

当程序查询访问属性的值时，getter 方法会被调用（不传参数）。这个方法的返回值就会成为访问结果。

当程序为访问属性赋值时，setter 方法会被调用。这个方法会被用来对属性进行赋值，方法的返回值会被忽略。

如果一个属性同时拥有 setter 和 getter 方法，这是一个可读写的属性；若只有 getter 方法，则为只读属性；若只有 setter 方法，则为只写属性。

访问属性是通过和属性同名的方法来定义的。除去前面的 get 和 set 关键字，它们和普通的方法看上去一样。

下面有个例子：

```js
let o = {
  x: 3,
  y: 4, // x 和 y 都是常规的可读写属性
  get r() {
    return Math.hypot(this.x, this.y); 
  },
  set r(newValue) {
    let oldValue = Math.hypot(thi.x, this.y);
    let ratio = newValue / oldValue;
    this.x *= ratio;
    this.y *= ratio;
  }, // r 是通过 getter 和 setter 来定义的可读写访问属性
  get sum() {
    return this.x + this.y;
  } // suum 是只通过 getter 定义的只读访问属性
}
o.r // 5，调用了 r 的 getter 方法
o.sum // 7 调用了 sum 的 getter 方法

o.r = 10 // Ok 调用了 r 的 setter 方法
o.x, o.y // 6, 8
o.sum // 14
```

访问属性可以被继承，就像普通的数据属性一样：

```js
let o2 = Object.create(o);
o2.x // 6
o2.r // 10
o2.sum // 14
o2.r = 5 // Ok

```

### 小结

这一章的要点：

- 最基础的对象术语，比如 enumerable（可枚举性） 和 own property（私有属性）
- 对象字面量句法和 ES6 后的新特性
- 如何读写，删除，枚举，检验对象的属性
- 对象如何从原型继承属性，使用 Object.create()
- 如何复制属性到另一对象中，使用 Object.assign()

## 数组（Arrays）

数组是在 JavaScript 和其他任何编程语言中最基础的数据类型。数组是有许多值的有序集合，每一个在数组中的值被称为元素（element）。每一个元素都拥有其在数组中的，用数字表示的位置，被称为索引（index）。

在 JavaScript 中的数组是不需要被定义类型的，这意味着一个数组的元素可是是任何类型，同一个数组中不同的元素也可以拥有不同的类型。这使得我们可以用数组构造出复杂的数据结构，例如一数组的对象，或者一数组的数组。

JavaScript 中的数组是从0开始进行索引的（zero-based），最多拥有 2<sup>32</sup>-1 个元素（32-bit indexes）。JavaScript 中的数组是动态的，它的长度会根据需要而增加或减少。而且我们不必手动创建长度固定的数组，也不用在长度变更时进行手动的重新分配。JavaScript 中的数组也可以是稀疏的，元素不一定要有连续的索引，其中可以有空缺。数组有一个名为 length 的属性，对于连续的数组，它会返回元素的数量；对于稀疏的数组，它会返回其元素中最大索引的值加一。

JavaScript 中的数组其实是一种特别的对象，索引其实就是用非负整数表示的属性名。但通常在实现时数组被优化过，所以访问通过数字索引来访问数组元素会比通过属性名来访问对象属性值来的快得多。

数组会从 Array.prototype 继承属性，其中定义了许多用于操作数组的方法。

ES6 引入了一系列新的数组被称为 typed arrays。不像普通的数组，它们的查毒是固定的，且只能拥有固定的（fixed）数字元素类型。它们可以拥有更好的性能，也可以对二进制数据进行 byte-level 访问。

### 创建数组（Creating Arrays）

在 JavaScript 中一共有以下几种方式来创建数组：

- 数组字面量（Array Literals）
- ... 展开操作符
- Array() 构造函数
- Array.of() 方法
- Array.from() 方法

#### 数组字面量（Array Literals）

数组字面量是创建数组最简单的方法了：

```js
let empty = []; // 空数组
let a = [1, 2, 3, 4, 5]; // 五个数字元素的数组
let b = [1, true, 'a', {x:1}, [1,2,3]]; // 五个各种类型的元素

let c = 1;
let d = [c, c+1, c+2, c+3]; // 可以在数组元素的位置使用表达式
let e = [1, , , ,5]; // 长度为 5 的稀疏数组
let f = [,,]; // 长度为 2 的稀疏数组（因为尾部的逗号是可选的，所以长度为 2 而非 3
```

#### 展开操作符（The Spread Operator）

在 ES6 及以后可以使用展开操作符，...，来在数组字面量中加入元素：

```js
let a = [1,2,3];
let b = [0, ...a, 4]; // b = [0,1,2,3,4]
```

使用展开操作符也可以用来创建一个数组的浅拷贝：

```js
let a = [1,2,3];
let b = [...a];
```

展开操作符可以对任何可遍历对象使用。例如字符串，

```js
let a = [...'hello'];
a // ['h','e','l','l','o'];
```

#### 数组构造函数（The Array() Constructor）

另一种创建数组的方法就是使用数组的构造函数了：

```js
let a = new Array();
a // []

let b = new Array(10); // 传入一个 argument 时，指定其长度为10
b.length // 10

let c = new Array(1,2,3,4,5); // 传入多个 arguments，成为其元素
c // [1,2,3,4,5]
```

使用数组字面量在几乎所有情况都会比调用构造函数来的简单。

#### Array.of()

使用数组构造函数时有一个弊端，在 argument 只有一个整数时，它会被当作长度。所有我们无法使用构造函数来生成只有一个数字作为元素的数组。

在 ES6 之后，Array.of() 函数修复了这个问题。这是一个工厂（factory）方法，可以被用来创建和返回一个新的数组。他会直接使用其 arguments 作为元素。

```js
Array,of() // []
Array.of(10) // [10]
Array.of(1,2,3) // [1,2,3]
```

#### Array.from()

Array.from() 是另一个工厂方法，他会期待一个可遍历，或类数组对象作为其第一个 argument 然后返回由其构造的数组。

```js
let a = Array.from([]); // []
let b = Array.from([1,2,3]); // [1,2,3]
let c = Array.from('abc'); // ['a','b','c']
```

### 读写数组元素（Reading and Writing Array Elements）

当使用 [ ] 访问数组元素时，可以进行元素的读写。

```js
let a = [1];
let b = a[0]; // b = 1
a[1] = 2; // a = [1,2]
```
在使用非负整数作为属性名时，数组会自动维护其 length。JavaScript 会自动将数字索引转换成字符串，并将其作为属性名。

在访问不存在的索引时，不会抛出 error 而是返回 undefined

```js
let a = [1,2,3];
a[5] // undefined
a[-1]// undefined
```

### 稀疏数组（Sparse Arrays）

稀疏数组就是那些拥有非连续元素的数组。通常情况下，数组的 length 属性是其元素数量，而对于稀疏数组则其 length 属性会大于元素数量。通过以下方法可以创建一个稀疏数组：

```js
let a = new Array(5);
a // [empty × 5]

let b = [];
b[1000] = 0;
b // [empty × 1000, 0]
```

通常情况下，充分稀疏的数组会被以一种更为缓慢，但节省了空间的方式编译。访问元素的速度和普通数组差不多。

虽然在实际情况中，大多数的数组都不是稀疏的，但想要理解 JavaScript 的 true nature，了解稀疏数组是很有必要的。

### 数组长度（Array Length）

每一个数组都拥有一个 length 属性，这也是区分数组和别的对象的属性。

- 密集数组，length 的值为最大索引加一
- 稀疏数组，length 的值一定大于每一个元素的索引

为了维持 length 这一属性，数组还有两个特别的行为：

1. 在对索引大于 length 的位置赋值时，length 会被设置成索引的值加一
2. 如果将 length 设置成一个小于其当前值的非负整数 n 时，所有索引大于或等于 n 的位置会被删除

```js
let a = [1,2,3];
a.length = 1; // a = [1] 后面的元素被删除了
a.length = 3; // a = [1, empty × 2] 长度被补回了，但元素不会
```

### 增删数组元素（Adding and Deleting Array Elements）

我们已经见过如何增加元素了，只需对新索引进行赋值即可。除此之外，我们还可以使用 push() 方法在数组中增加一个或多个值：

```js
let a = [];
a.push(1); // a = 1
a.push(2,3); // a = [1,2,3]
```

pushing 一个值进入数组等同于为 a[a.length] 进行赋值。接下来还会对 pop(), unshift(), shift() 进行介绍。对于删除数组元素，其方法和删除对象属性类似，只需要使用 delete 操作符即可：

```js
let a = [1,2,3];
2 in a // true
delete a[2];
2 in a // false
a // [1, 2, empty]
```

注意使用 delete 时不会改变数组的长度，因为他并不会让高索引元素自动补上空白处。删除元素会是数组变成稀疏数组

### 遍历数组（Iterating Arrays）

在 ES6 之后，遍历数组元素最简单的方式就是使用 for of 循环，对于不存在的元素，他会返回 undefined：

```js
let a = [1, , , ,5];
for (let element of a) {
  console.log(element); // 1, 3 x undefined, 5
}
```

如果想在返回时同时知道每个元素的索引，可以使用 entries() 方法来进行解构赋值：

```js
let a = [1, , , ,5];
for (let [index, element] of a.entries()) {
  console.log(`${index}: ${element}`);
}
// 0: 1
// 1: undefined
// 2: undefined
// 3: undefined
// 4: 5
```

除此之外还可以使用 forEach()，这是一个数组的方法，用于进行数组遍历，他会传递元素的值和元素的索引作为 arguments，不过注意，forEach() 方法会忽略不存在的元素：

```js
let a = [1, , , ,5];
a.forEach((element,index) => {
  console.log(`${index}: ${element}`);
};
// 0： 1
// 4： 5
```

当然我们也可以使用 old-fashioned for 循环来遍历数组。这里就不列出了。

### 多维数组（Multidimensional Arrays）

JavaScript 其实并不在真正意义上支持多维数组，但是我们可以用 arryas of arrays 来模拟。要访问其中的值时，只需要调用 [ ] 两次即可。例如 matrix[x][y]。

### 数组方法（Array Methods）

数组的方法大致可以分为以下几类：

- 遍历方法
- 模拟栈或队列方法
- subarray 方法
- 搜索或排序方法

注意，某些方法会直接在原数组上进行改动而某些则是返回一个新数组。

#### 数组遍历方法

在谈论每一个方法前，先讲一下他们的共通性。首先，每一个方法都可以接受一个函数作为它们第一个 argument。这个数组的每一个（或某些）元素会对这个函数进行调用，对于不存在的元素则不会进行调用。大多数情况下，这个函数会调用三个 arguments：元素的值、元素的索引、数组本身。但大多数情况下，只有第一个 argument 会被使用到。

大多接下提到的遍历方法还会接受一个可选的第二个 argument，如果有的话，第二个 argument 会作为第一个函数的调用者，所以在第一个函数中的 this 会指向第二个 argument。

遍历方法并不会对原数组进行改变，即使调用函数后的返回值不同。通常情况下，argument 中的函数会进行 inline 定义。

##### forEach()

forEach() 方法会遍历数组，并执行定义的函数：

```js
let a= [1,2,3,4,5], sum = 0;
a.forEach(value => { // 只使用了第一个 argument，元素的值
  sum += value; 
});
sum // 15

a.forEach((v,i,a) => { // 使用了第三个 argument，元素的值，元素的索引，指向的数组
  a[i] = v+1;
})
a // [2,3,4,5,6]
```

不过在 forEach() 遍历中，这里没有终止其遍历的方式（无法使用 break）。所以在遍历完成前将无法停止。

注意，forEach() 会直接跳过不存在的元素。

##### map()

对于传入 map() 方法和 forEach() 方法 argument 的函数，它们被调用的方法是一样的。只不过 map() 中的函数需要有一个返回值。map() 在被调用时会返回一个新的数组，新的数组会根据传入的函数的返回值最为元素而组成：

```js
let a= [1,2,3,4,5], sum = 0;
b = a.map(value => { 
  sum += value; 
  return sum; // 需要一个返回值，会作为返回数组中的元素
});
b // [1,3,6,10,15]
```

注意，map() 会跳过不存在的元素，但是返回数组依然会以同样的顺序包含那些不存在的元素。

##### filter()

filter() 方法会根据传入的函数来过滤掉一些元素，并返回剩下的一个包含剩余元素的新数组。filter() 的调用方式如同 forEach() 和 map() 一样，不过传入的函数需要返回 true 或者 false。只有返回值为 true 时，这个元素才会被加入返回的新数组中：

```js
let a= [1,2,3,4,5];
b = a.filter(value => { 
  return value%2; //需要返回 true 或者 false，只有 true 时，该元素才会被加入返回数组
});
b // [1,3,5]
```

注意，filter() 也会跳过不存在的元素，不过返回数组一定时密集的，即不再包含那些不存在的元素。

##### find() 和 findIndex()

这两个方法也会根据接收的函数来遍历数组，当函数的返回值为真值时，直接返回当前元素，并结束遍历。find() 会返回符合的元素值，findIndex() 则会返回元素索引，若遍历完成还是无法找到，则分别返回 undefined 和 -1：

```js
let a = [1,2,3,4,5];
a.findIndex(x => x===3); // 2 索引2的值为三
a.findIndex(x => x<0); // undefined
a.find(x => x%5); // 元素值5可以整除5
a.find(x => x%6); // -1
```

注意, find() 和 findIndex() 也会跳过不存在的元素。

##### every() 和 some()

这两个方法也是根据接收的函数来遍历数组，他们的运作模式就是他们的方法名：

- every() 会在每一个元素调用接收函数都返回 true 时返回 true，若有一个返回了 false，则直接停止遍历并返回 false
- some() 只要有一个元素返回了 true，它便会停止遍历并返回 true
- 基于数学转换，对空数组 [ ] 使用 every() 会返回 true，对其使用 some() 则会返回 false

```js
let a = [1,2,3,4,5];
a.every(x => x<10); // true
a.every(x => x%2); // false
a.some(x => x%2); // true
a.some(isNaN); // false
```

##### reduce() 和 reduceRight()

reduce() 和 reduceRight() 方法会根据接收的函数来结合每一个数组元素，并返回一个单一值。

reduce() 会接收两个 arguments，第一个是函数，他会接收两个值并将它们结合并返回；第二个可选 argument 则是一开始传入函数的初始值：

```js
let a = [1,2,3,4,5];
a.reduce((x,y) => x+y, 0) // 15
a.reduce((x,y) => x*y, 1) // 120
a.reduce((x,y) => x>y ? x : y) // 5
```
不同于其他遍历方法，它接收的函数的 arguments 的元素值，元素索引和数组本身，是 arguments 的第2，3，4位，而非1，2，3位。他的第一个 argument 是到目前为止累计结果。

在第一次调用函数时，它的第二个 argument 会作为这个累计结果被传入，接下来就是上一次调用函数的返回值。在没有第二个 argument，既没有给定初始值时，数组的第一个值就会被作为这个初始值，然后数组的遍历会从第二个值开始。

对空数组使用 reduce() 会抛出 TypeError，对只有一个元素的数组使用 reduce() 则会直接返回那个值，而不进行函数的调用。

reduceRight() 基本上和 reduce() 一样，只不过他是从右到左的。

因为可选初始值的介入，reduce() 和 reduceRight() 不在可以接收那个用来变成函数调用者的第二个 argument

#### 数组扁平化方法

##### flat()

在 ES2019 中，flat() 方法被引入了，它会创建并返回一个新的数组包含原始元素，只不过数组中的元素会被 flattened 罢了：

```js
[1,[2,3]].flat(); // [1,2,3]
[1,[2,[3]]].flat(); // [1,2,[3]]
```

我们可以传入一个 argument 来指定打平的深度，传入 Infinity 可以将数组完全扁平化：

```js
let a = [1,[2,[3,[4]]]];
a.flat(1); // [1, 2, [3, [4]]]
a.flat(2); // [1, 2, 3, [4]]
a.flat(Infinity); // [1, 2, 3, 4]
```

##### flatMap()

flatMap() 的运作方法等同于 map()，不过返回数组会被自动打平。a.flatMap(f) 等同于(但是效率更高于）a.map(f).flat()：

```js
let str = ["hello world", "the definitive guide"];
let words = str.flatMap(value => value.split(' '));
words // ["hello", "world", "the", "definitive", "guide"]
```

#### 数组联接方法

##### concat()

concat() 方法会创建并返回一个新的数组，包含调用 concat() 的数组的所有元素，并会一一联接传入 concat() 的 arguments 作为元素。若传入的 argument 是一个数组，则它会被先打平一次再联接。concat() 也不会对调用它的函数造成影响：

```js
let a = [1,2,3];
a.concat(4,5); // [1,2,3,4,5]
a.concat([4.5]); // [1,2,3,4,5]
a.concat([4,[5]]); // [1,2,3,4,[5]]
a // [1,2,3] 原数组不变
```
concat() 会在每次调用时创建其原数组的复制，所以这是一个开销比较大的操作。在进行类似于 a = a.concat(b) 之类的操作时，我们会希望不使用 concat() 而是使用直接修改原数组的方法。

#### 数组模拟栈和队列

##### push() 和 pop()

push() 和 pop() 方法允许我们用数组模拟栈并对其进行操作。push() 方法可以在数组后追加元素并返回修改后的数组长度。push() 不同于 concat()，他不会对输入的 arguments 进行扁平化操作。pop() 方法可以用来弹出数组最后一个元素。这两个方法都是直接在原数组上进行的。这是我们可以模拟 FILO 栈（first-in, last-out stack)：

```js
let stack = [];
stack.push(1,2); // stack = [1,2]
stack.pop(); // stack = [1]，返回 2
stack.push([3,4]); // stack = [1,[3,4]]
stack.pop(); // stack = [1]，返回 [3,4]
stack.pop() // stack = []，返回 1
stack.pop() // 已经没有元素了，返回 undefined
```

若想要扁平化添加的数组，可以通过使用延展操作符：

```js
a1.push(...a2);
```

##### unshift() 和 shift()

它们和用法和 push() 和 pop() 如出一辙，只不过是从数组头进行操作而非数组尾。unshift() 会在数组头推入一个元素，将每个后续元素向后推，并返回数组长度；shift() 会弹出第一个元素，并将后续元素向前推。

我们也可以用 unshift() 和 shift() 来模拟栈，不过它们比 push() 和 pop() 跟低效，因为 unshift() 和 shift() 操作时要移动数组每一个元素。不过我们可以用 push() 和 shift() 来模拟一个队列：

```js
let queue = [];
q.push(1,2); // q = [1,2]
q.shift(); // q = [2]，返回 1
```

注意，在 unshift 多个元素时，他们是同时被 unshift 进去的。所以和一个一个 unshift 的顺序不同：

```js
let a = [];
a.unshift(1);
a.unshift(2);
a // [2,1]

let b = [];
b.unshift(1,2);
b // [1,2]
```

##### 子数组方法

##### slice()

slice() 方法，正如其名，会根据原数组返回一个 slice，或者说是 subarray，但不会改变原数组。他接收 2 个 arguments，分别是是界定的返回的 subarray 的起始和结束索引。返回的数组会包含从第一个 argument 作为索引的元素一直到第二个 argument 作为索引的元素（但不包括）；如果只传入一个 argument，则会从那个索引开始截取剩下的整个数组；若传入负数，他便会成为根据 length 的相对长度；若不传入 argument，则直接复制原数组：

```js
let a = [1,2,3,4,5];
a.slice(); // [1，2，3，4，5]
a.slice(0,3); // [1,2,3]
a.slcie(3); // [4,5]
a.slice(1,-1) // [2,3,4]
a.slice(-3-2) // [3]
```

##### splice()

splice() 是一个多目的的方法，他会直接在原数组进行修改。它可以插入或移除或同时执行插入和移除特定的元素。splice() 接收的第一个 argument 会指定插入/移除元素的位置；第二个 argument 会指定想要删除的元素数量（若没有第二个 argument，则会从开始位置删除剩下的所有元素）；接下来的 arguments 会指定想要插入的元素值。splice会返回一个包含被删除元素的数组：

```js
let a = [1,2,3,4,5,6,7,8];
a.splice(5); // a = [1,2,3,4,5]， 返回 [6,7,8]
a.splice(1,2); // a = [1,4,5]，返回 [2,3]
a.splice(2,0,'a','b'); // a = [1,4,'a','b',5]，返回 []
a.splice(2,2,'x'); // a = [1,4,'x',5]，返回 ['a','b']
```

##### fill()

fill() 方法会为一个数组的元素赋值，它会对原数组直接进行修改，并返回修改后的数组。第一个 argument 是想要设定元素的值；第二个可选的 argument 则指定了开始元素的索引，若没有的话，则从索引 0 开始；第三个可选的 argument 则指定了结束元素的索引（但不包括），若没有的话，则会一直到数组结束（如slice 一样，我们可以使用负数）：

```js
let a = new Array(5) // a = [empty × 5]
a.fill(0) // a = [0,0,0,0,0]，同时返回 [0,0,0,0,0]
a.fill(1,1) // a = [0,1,1,1,1]，同时返回 [0,1,1,1,1]
a.fill(2,2,-1) // a = [0,1,2,2,1]，同时返回 [0,1,2,2,1]
```

##### copyWithin()

copyWithin() 方法会复制数组中的一节 subarray 并将其放置到原数组中的其他位置。他会修改原数组，不能返回修改后的数组。他不会改变数组长度。第一个 argument 指定了放置复制完的 subarray 的开始索引；第二个 argument 指定了从哪个索引开始复制 subarray（省略时为0）；第三个 argument 指定了从哪个索引（不包括）结束复制 subarray(省略时为数组的 length），也可以使用负数：

```js
let a = [1,2,3,4,5];
a.copyWithin(1); // [1,1,2,3,4] // 复制了整个原数组，从索引 1 开始放置它，多余的元素 5 被忽略了
a.copyWithin(2,3,5) // [1,1,3,4,4] // 复制了索引 3 到 5：[3,4]，放在了从索引 2 开始的位置
a.copyWithin(0,-2) // [4,4,3,4,4] //复制了索引 3 到 5：[4,4]，放在了从索引 0 开始的位置
```

虽然这个方法看上去很麻烦，但是它是一个 high-performance 的方法，并且在 typed arrays 中会格外实用。他是用过模仿 C 语言中的 memmove() 函数诞生的。

#### 数组搜索方法

##### indexOf()

它会搜索数组，并在找到第一个指定值时返回它的索引。若找不到，则返回 -1。它接收的第一个 argument 是进行对比的值；可选的第二个 argument 是遍历开始位置。
它的检索方法是进行 === 比较，所以对于对象，它会检测是否引用了同一个对象（若只想检测对象的内容是否相同，可以使用 find() 方法）：

```js
let a = [0,1,2,1,0];
a.indexOf(1) // 1
a.indexOf(1,2) // 3
a.indexOf(3) // -1
```

注意，字符串也有 indexOf() 方法，他的用法和数组的相同，只不过接收的负数会被当做 0。

##### lastIndexOf()

lastIndexOf() 方法基本等同于 IndexOf()，只不过是从右到左遍历的。

##### includes()

ES2016 新增的 includes() 方法中，它接收一个 argument 并在数组包含那个 argument 时返回 true，否则返回 false。它也是进行了 === 比较，不过有一个特例，他会对 NaN 和 NaN 的比较返回 true：

```js
let a = [1,2,3,NaN];
a.includes(1); // true
a.includeS(4); // false
a.includes(NaN); // true
a.indexOf(NaN); // -1
```

#### 数组排序方法

##### sort() 

这个方法会对元素进行排序并返回。他会直接改变原数组。在没有传入 arguments 时，他根据字符串的字母顺序进行排序，必要时会进行类型转换，转换成字符串，undefined会被排序在最后：

```js
let a = ["banana","cherry","apple"];
a.sort() // a = ["apple", "banana", "cherry"]
```

如果想用其他方法排序，则需要传入一个 argument 作为比较函数。他会根据这个比较函数来判断如何排序。这个函数接收两个 arguments 并判断哪一个会被排序在前面。如果想要第一个 argument 排序在前，则函数的返回值需要小于 0；若想要第二个 argument 排序在前，则函数的返回值需要大于 0；若他们相等，则返回 0：

```js
let a = [33,4,1111,222];
a.sort(); // a = [1111,222,33,4]; 根据字母顺序进行排序，而非数字大小
a.sort((x,y) => x-y); // a = [4,33,222,1111]，根据数字大小进行排序
```

##### reverse() 

reverse() 方法会返回 reverse 元素顺序后的数组。他会直接改变原数组。

```js
let a = [1,2,3];
a.reverse();
a // [3,2,1]
```

#### 数组转字符串方法

##### join()

join() 方法回将每一个元素转换成字符串并连接他们，形成一个字符串。它可以接收一个字符串 argument 作为元素的链接方法。若不传入 argument，默认使用逗号链接。若数组包含不存在的值，会被转换成空字符串：

```js
let a = [1,2,3];
a.join(); // '1,2,3'
a.join(' '); // '1 2 3'
a.join(''); // '123'
a.join('-'); // '1-2-3'
```

join() 是 String.split() 的 inverse 方法。数组也有 toString() 方法，他等同于调用了无 arguments 的 join() 方法； toLocaleString() 方法则会以本地的链接方法链接数组。

##### 数组静态方法

##### isArray()

Array.isArray() 方法可以用于判断一个未知的值是否为数组：

```js
Array.inArray([]); // true
Array.isArray({}); // false
```

### 类数组对象（Array-Like Objects）

JavaScript 中有许多类数组对象，虽然它们并不是数组，也无法从数组原型上继承很多有用的方法，但是它们依然可以被遍历，就如同我们遍历数组那样。我们可以将对象的 length 作为类数组对象的长度，非负整数属性名作为索引来定义一个类数组对象：

```js
let a = {};
let i = 0;
do {
  a[i] = i**2;
} while( i++ < 5 );
a.length = i;
a // {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, length: 6}，这就是一个类数组对象
let total = 0;
for (let j = 0; j < a.length; j++) {
  total += a[j];
}
total // 55
Array.from(a) // [0, 1, 4, 9, 16, 25]
```

除了对象之外，字符串也拥有类似于只读数组的行为。除了使用 charAt() 来访问单一的16位值，我们也可以用数组的方法：

```js
let a = 'hello';
a.charAt(0); // 'h'
a[0] // 'h'
```

这样的好处不仅是具有更快的编写速度、更高的可读性，也 protentially 更为高效。

#### 调用数组方法

即便类数组对象不拥有数组原型上的方法，我们可以仍可以对类数组对象使用那些方法：

```js
let a = {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, length: 6}; // a 是一个类数组对象
Array.prototype.push.call(a,36);
a // {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, length: 7}

let b = 'hello';
Array.prototype.join.call(b,'-'); // 'h-e-l-l-o'
Array.prototype.push.call(b,'a'); // TypeError，因为字符串被认作只读数组，所以无法进行修改
```

### 小节

这一章的要点：

- 使用数组字面量可以在中括号中用逗号分割每个元素值
- 数组元素可以通过数组加中括号中加入索引的形式来访问（a[i]）
- for of 循环和 ... 展开操作符对遍历数组很有用
- 数组原型上有许多好用方法，熟悉他们很重要

## 函数（Functions）

函数是 JavaScript 中最基础的组件，也是所有编程语言中最普遍的特性之一。函数就是一块只被定义了一次，但是可以被多次调用（执行）的代码块。函数的定义中可以包括一列参数（parameters），它们会在函数体中被使用。在调用函数时，会提供 arguments 对象作为参数传入。 函数通常会根据参数来计算出一个返回值来表示调用函数的表达式的值。除了 arguments 之外，每次调用是还会将调用时的上下文（invocation context）传入函数。在函数中可以通过 this 关键字访问。

如果函数被赋值成为了一个对象的属性，他就成为了那个对象的方法（method）。在调用对象方法时，那个对象就成了他的调用上下文，也就是 this 的值。用于创建新对象的函数被称为构造函数（constructor）。

在 JavaScript 中，函数也是对象，它们也能被程序操作。例如函数可以被复制到一个变量名上，也可以传入其他的函数中。正因为函数是对象，我们也可以为函数定义属性或者调用函数上的方法。

JavaScript 中的函数也可以被定义在函数中，内部函数可以访问任何外部函数中的值只要在在其作用域中。这也意味着 JavaScript 函数是闭包（closures），详细内容会在接下来被介绍。

### 定义函数（Defining Functions）

使用 function 关键字是定义函数最直白的方法了。在 ES6 中，一种新的定义函数的方法出现了，他被称为箭头函数（arrow functions），这允许我们不使用 function 关键字也能定义一个函数。他的格式十分简洁，所以在作为 argument 传递时会很有用。除此之外，我们还可以使用 Function() 构造函数来定义函数。

#### 函数声明（Function Declarations）

函数声明由以下几部分组成：

- 首先是 function 关键字
- 其次是一个用于表示函数名的标签，它会成为一个变量名，并且新构建的函数会被赋值在这个变量名上
- 再其次是一对括号，其中包含了 0 个或更多个标签。这些标签就是函数的参数名（parameter name）
- 最后是一对大括号，里面包含了一句或多句 JavaScript 语句。它们也被称为函数体，会在调用函数时执行

例子：

```js
function sum(x,y) {
  return x + y;
}

function printHi() {
  console.log('Hi!');
}
```

函数名会变成变量名，函数本身会变成变量值。用函数声明定义的函数会进行变量提升（hoisting）到其所在的最近代码块中的最高处。所以在程序中，可以在声明函数前调用它。

函数可以拥有一个返回值（return value），他会被使用 return 语句返回。这个值会被返回到函数的调用者（caller）。return 语句也会使函数停止之后的执行。若 return 语句后没有跟着表达式，则返回值是 undefined。

函数也可以不拥有返回值，这样调用函数就会简单的执行函数体中的语句，然后返回 undefined 作为值。

#### 函数表达式（Function Expressions）

函数表达式和函数声明看上去很像，但它通常在一个长表达式或语句的上下文中（context）出现，并且函数名是可选的：

```js
const square = function(x) { return x**2;} // 函数表达式出现在了赋值表达式中
square(10) // 100
let a = [3,2,1];
a.sort(function(a,b) {return a-b;}); // 函数表达式成为了其他函数的 argument
let tenSquare = (function(x) {return x**2;}(10)); //函数表达式被定义并且同时调用
tenSquare // 100
tenSquare(10) // TypeError，tenSquare 不再是函数，而是调用函数的返回值
```

函数声明会为其声明一个变量并保存函数作为那个变量值；而函数表达式则不会声明变量，将其赋值给常量或变量是可选的。函数表达式的最佳实践是为其赋值到一个 const 常量上，这样可以却把他不会被覆写。不同于函数声明，函数表达式不会被提升（hoist）。

#### 箭头函数（Arrow Function）

在 ES6 中，我们可以通过箭头函数来定义函数。这种句法十分的简洁，并且使用了 => 箭头符号用于区分参数和函数体。function 关键词则不需要使用。箭头函数也是表达式，所以他的名字也是可选的：

```js
const sum = (x,y) => {return x + y;};
```

群殴事件哦箭头函数还有更加简洁的写法。如果函数体中只有单一的 return 语句，我们甚至可以省略 return 关键字和大括号：

```js
const sum = (x,y) => x + y;
```

更近一步，如果只有一个参数，则括号也可以被省略；但若没有参数，则不能省略括号：

```js
const square = x => x**2;
const num = () => 123;
```

如果过函数体只有一句 return 语句，并且返回的值是对象时，需要加一对括号对函数体进行区分：

```js
const a = x => {return {value: x};}; // 返回对象
const b = x => ({value:x}); // 返回对象，和上面一样
const c = x => { value: x}; // 不返回任何值，使用了标签语法
const d = x => {v:x, w:x}; // SyntaxError
```

箭头函数和别的函数还有一个区别，其会根据它被定义的环境而继承 this 的值，而非其调用时再去定义的环境。箭头函数的另一个区别是它不具有原型。

#### 嵌套函数（Nested Functions）

在 JavaScript 中，函数可以被嵌套在其他函数中：

```js
function hypotenuse(a,b) {
  function square(x) {
    return x**2;
  }
  return Math.sqrt(square(a) + square(b));
}
hypotenuse(3,4) // 5
```

嵌套函数需要注意的是其变量作用域：嵌套函数可以访问其存在的函数的参数和变量（比如上面的 square() 中可以访问 a 和 b，也就是 hypotenuse() 的参数）。这一点十分的重要，更详细的会在后边被讨论。


### 调用函数（Invoking Functions）

函数体被定义时，它们并不会被执行。只有在它们被调用时才会被执行。它们可以被以下五种方法调用：

- 作为函数
- 作为方法
- 作为构造函数
- 使用 call() 和 apply() 这种非直接方法
- 隐式的，通过 JavaScript 特性调用

#### 函数调用（Function Invocation）

函数会在调用表达式中被调用。括号中的表达式会被计算，并且结果的值会作为 arguments 传入函数作为其参数。调用表达式的值就是函数的返回值。若没有返回值时为 undefined。

在非严格模式中调用函数时，调用上下文（this 的值）是全局对戏；在严格模式中，调用上下文则是 undefined。注意箭头函数的行为是不同的，它一直会从其定义的位置继承 this 的值。通常情况下，作为函数调用的函数（不是方法）一般情况下根本就不用使用 this 关键字。

我们可以使用 this 来判断现在是否在严格模式中：

```js
const strict = function() {return !this;};
strict()； // 严格模式下返回 true，非严格模式下返回 false
```

#### 方法调用（Method Invocation）

方法其实也是函数，只不过被保存为了对象的属性。假设有一个对象 o 和一个函数 f，我们可以这样定义一个方法：

```js
o.a = f;
```

这样方法 a() 就被定义了，它可以这么被调用：

```js
o.a(x,y);
```

方法调用和普通函数调用最大的区别就是它们的调用上下文（invocation context），属性访问表达式包含两部分：一个对象（o）和一个属性名（a）。当这样调用方法时，对象（o）会成为调用上下文，所以方法可以使用 this 来引用那个对象（o）：

```js
let calculator = {
  a: 1,
  b: 2,
  add() { // 其中的 this 指向了这个对象，calculator
    this.sum = this.a + this.b;
  },
  muti() {
    this.product = this.a * this.b;
  }
}
calculator.add(); // 调用了 add() 方法
calculator.sum = 3
calculator['muti'](); // 也可以这样调用
```

##### this 指向

对于面向对象编程（oop）时，方法和 this 十分的重要。任何作为方法的函数都会隐式的传入包含那个方法的对象作为 argument，在函数内可以通过 this 访问。this 是一个关键字而非变量，所以他无法被赋值。他的作用域和变量不同。嵌套函数（非箭头函数）也不会包含他的函数中继承 this，而是：若这个内嵌函数以方法的形式被调用，this 的值为其对象；如果以函数调用（非箭头函数），this 的值就会是全局对象（非严格模式）或者 undefined （严格模式）。

```js
let o = { 
  m: function() { //对象中的方法
    console.log(this === o); // true，方法的 this 指向了对象 o
    function f() { // 方法的内嵌函数
      console.log(this === o); // false，内嵌函数的 this 不再指向对象 o
      console.log(this === globalThis); // true，而是指向了全局对象（在浏览器中为 window）
    }
    f();
  }
}
o.m(); // true false true
```

在上述的例子中，其实内嵌函数无法指向对象在大多数情况被认为是 JavaScript 的缺陷，所以在 ES6 中引入了箭头函数，用来修补这个缺陷，箭头函数可以正确的继承 this 的值：

```js
const f =() => { // 将上面例子中的函数声明改成箭头函数
  console.log(this === o); // true，this 的值就会指向对象 o
  console.log(this === globalThis); // false，而非全局对象
}
```

#### 构造函数调用（Constructor Invocation）

如果函数或方法以关键字 new 创建，这就是调用了构造函数。构造函数调用和普通的函数调用有以下区别：对于 arguments 不同的处理、不同的调用上下文和返回值。

构造函数调用时会创建一个新的空对象，并且根据其指定的对象继承 prototype 上的属性。这个新建的对象就是其调用上下文，所以可以被 this 指向。构造函数一般不会使用 return 关键字，而是直接返回新创建的对象。

#### 非直接调用（Indirect Invocation）

JavaScript 中的函数是对象，就如同所以其他对象一样，它们也有方法。其中两个为 call() 和 apply()，可以被用于非直接调用。它们也允许我们显式的指定 this 的值。它们会在接下来被介绍。

#### 隐式函数调用（ Implicit Function Invocation）

以下情况会隐式的调用函数：

- 如果某个对象的 getter 或 setter 被定义，在查询或者修改那个属性的值时，上述方法会被调用
- 在对象作用于需要字符串的情况下（比如连接字符串和对象时），它的 toString() 方法辉被调用
- 同上，在需要非字符串值时，valueOf 方法会被调用()
- 对可遍历对象进行遍历时，会有一些方法被使用到
- 代理对象（Proxy objects）的行为完全由函数掌控，对对象上的任何操作都会调用其函数

### 函数的 arguments 和参数（Function Arguments and Parameters）

在 JavaScript 中函数的参数类型是动态的，所以不需要一个固定的类型。JavaScript 甚至不会检查你传入了多少个 arguments。

#### 可选参数和默认值（Optional Parameters and Defaults）

当函数调用时的 arguments 数小于其定义的参数时，多余的参数会被设定为其默认值，通常情况下时 undefined。这一点对于某些 arguments 为可选的时候格外有用。在 ES6 以后，我们可以为参数列表中直接为参数设定默认值：

```js
function a (x,y =2) {
  return x+y;
}
a(1); // 3，y 为默认值 2
a(1,3); // 4
```

为参数设定默认值的表达式会在函数被调用时定义，所以每次它缺少 arguments，默认值就会被传递作为参数。

#### 剩余参数（Rest Parameters）

剩余参数是我们可以超出参数数量的 arguments：

```js
function max(a = 0, ...rest) { // 为第一个 argument 设定默认值 0，获得剩余的所有 arguments 进入 rest 数组
  let maxValue = a;
  for (let n of rest) { // 遍历数组 rest
    if (n > a) {
      maxValue = n;
    }
  }
  return maxValue;
}
max(1,2,5,234,66,44,165); // 165
```

注意，剩余参数必须在 arguments list 中的最后一位。剩余参数是一个数组，即使不穿参也是一个空数组而非 undefined。虽然都是三个点，但是不要把剩余参数和展开操作符搞混了。这种参数可变的函数被叫做 vararg 函数。（从 C 语言中带来）

#### arguments 对象（The Arguments Object）

在函数体中，argument 指向了调用它的 Arguments 对象。Arguments 对象是一个类数组对象， 这使我们可以在函数中使用索引取得 argument。

不过随着剩余参数的加入，使用 Arguments 对象的机会也是越来越少了，不过这是好事。因为它并不高效并且难以优化，特别是在非严格模式下。所以要尽可能避免使用 Arguments 对象。

#### 在调用函数时使用展开操作符（The Spread Operator for Function Calls）

展开操作符被用来展开一个数组，在函数调用时他也可以被这么使用：

```js
let nums [1,2,56,67,435,234,43,5];
Math.min(...nums); // 1
```

虽然他看上去和前面提到的剩余参数相同，不过它们是相反的：

- 展开操作符会在 arguments list 中展开数组
- 剩余参数会在参数中将 arguments 整合成数组

#### 解构 arguments 进入参数（Destructuring Function Arguments intoParameters）

在调用一个拥有一列 argument 值时的函数时，他们会被传入函数作为参数。传入的这一点其实和给变量赋值很像，所以我们也可以使用解构赋值:

```js
function add([x1,y1],[x2,y2]) {
  return [x1+x2, y1+y2];
}
add([1,2],[3,4]); // [4,6]
```

#### argument 类型（Argument Types）

JavaScript 参数不会定义其类型，也不会进行类型检验。不过我们可以手动添加一些类型检验：

```js
function sum(a) {
  let total = 0; 
  for (let element of a) {
    if (typeof element !== 'number') {
      throw new TypeError('elements must be numbers');
    }
    total += element;
  }
  return total;
}
sum([1,2,3]); // 6
sum(1,2,3); // TypeError, 1 不可被遍历
sum([1,2,'3']); // TypeError，元素 '3' 不是数字
```

### 函数作为值（Functions as Values）

在 JavaScript 中，函数不仅可以被定义和调用，他和可以作为值。这意味着函数可以被赋值给变量，存放于对象的属性，成为数组的元素，或者作为 argument 传入另一个函数。这是许多其他静态语言所不具备的能力。

```js
function square(x) {return x**2；}
```
上面的函数被定义之后，赋值给了变量 square。square 就会有了指向这个函数对象的引用。它也可以被赋值于其他变量：

```js
let s2 = square;
square(3); // 9
s2(3); // 9
```

在函数成为于数组元素时，他甚至不需要名字：

```js
le a = [x=>x**2, 3];
a[0](a[1]); // 9
```

##### 定义函数属性

因为函数只不过是特别的对象，它们也可以拥有属性。当函数需要一个”静态的“变量，其值会在整个调用中保持不变的情况下，直接调用函数的属性会十分方便：

```js
function counter() { // 定义函数
  return counter.count++; // 操作并返回函数的属性
}
counter.count = 0; // 为函数定义属性
counter(); // 0
counter(); // 1
counter(); // 2
```

### 函数作为命名空间（Functions as Namespaces）

在函数中定义的变量是无法在函数外访问到的，所以函数可以被称为新的命名空间（namespace）用来定义不会影响全局命名空间的变量。

比如在进行运算时出现了许多中间值，如果它们需要被不同程序使用，而我们又不知道这些变量是否会和别的变量冲突时，我们可以定义一个新的函数，并在其中定义这些变量，并且调用这个函数，这样就不会污染全局命名空间了：

```js
function chunkNamespace() {
  // 出现的中间值会存在函数命名空间中，而非全局命名空间
}
chunkNamespace();

//可以被包装成匿名函数：
(function() {
  // 变量
}()); // 这种形式被称为：immediately invoked function expression，其括号不可被省略
```

### 闭包（Closures）

就如同大多编程语言，JavaScript 使用了词法作用域（lexical scoping）。这意味着这意味着函数会使用其定义时的作用域（scope）执行，而非其调用时的作用域来执行。为了实现这种 lexical scoping，JavaScript 函数对象内部的状态必须保存一个指向函数定义时的作用域的引用。

这种函数对象和作用域的组合被称为了闭包。

技术层面来说，所有的 JavaScript 函数都是闭包，不过大多数情况下函数会在其定义的作用于下被调用，所以其中是否包含闭包就变的不是那么重要了。

闭包只有在函式调用于另一个作用域（不是其定义的作用域）时，才会变得比较有意思。通常这会发生在内嵌函数在其父函数中被返回的情况下。有许多强大的编程技术都会使用到这种内嵌函数的闭包。

```js
let scope = 'global scope'; // 一个全局变量
function checkScope() {
  let scope = 'local scope'; // 一个局部变量
  function f() {
    return scope; // 返回这时的 scope 值
  }
  return f();
}
checkScope() // 'local scope'
```

上面的例子中，checkScope() 函数声明了一个局部变量，然后定义并调用了一个返回那个变量值的函数。所以很明显 checkScope() 会返回 'local scope'。现在我们进行一点改变：

```js
let scope = 'global scope'; // 一个全局变量
function checkScope() {
  let scope = 'local scope'; // 一个局部变量
  function f() {
    return scope; // 返回这时的 scope 值
  }
  return f;
}
checkScope()() // 会返回什么呢？
```
在上面新的例子中，我们将函数的返回值设定成了其内嵌函数，而非直接调用内嵌函数的返回值。所以现在会发生什么改变呢？

其实我们只需要谨记词法作用域的基础逻辑：JavaScript 函数会使用其定义时的作用域来执行。

所以说，f() 函数被定义在了和 scope = 'global scope' 同一作用域下，也就是 checkScope() 内部。这种函数对象和其定义作用域的绑定依然存在，即闭包依然存在。所以不论在哪儿执行，他的作用域都是其绑定好的作用域。在上述的例子中依然会返回 "local scope"。

简而言之，这就是闭包强大之处：它可以捕获函数对象被绑定的作用域下的全部局部变量（和参数）。

```js
function counter() {
  let n = 0;
  return {
    count : function() {return n++;},
    reset : function() { n = 0;}
  };
}
let c1 = counter(), c2 = counter();
c1.count() // 0
c1.count() // 1
c2.count() // 0
c1.reset()
c1.count() // 0
c2.count() // 1 
```

上面的例子中，counter() 函数返回一个 counter 对象，这个对象有两个方法。我们需要理解的是：

- 两个方法是如何访问变量 n 的
- 每一次调用函数时都会创建一个新的对象，和新的作用域，以及其中变量

箭头函数中 this 的从包含其的函数中继承 this 的值，但是 function 关键字的不会。所以如果需要用 this 的值来指向包含它的函数，使用箭头函数。（还可以在外围函数中将 this 赋值进一个变量）

### 函数的属性和方法（Function Properties, Methods, and Constructor）

#### length 属性

length 是一个只读属性，代表了这个函数的 arity：就是他的参数数量。若有剩余参数，则其不会被包含于其中。

```js
function f1(a,b,c) {}
function f2(a,b,...c) {}
f1.length // 3
f2.length // 2 
```

#### name 属性

name 属性表示了函数定义时指定的名字。如果时函数拥有名字，则会返回那个名字；若为匿名函数，会返回他被第一次赋值到的那个变量。

```js
function f1(a,b,c) {}
let f2 = function (a,b,c) {}
f1.name // 'f1'
f2.name // 'f2'
```

#### prototype 属性

除去箭头函数之外的所以函数，都有 prototype 属性。这会指向原型对象。每一个函数都有其独特的原型对象。当一个函数被用于构造函数时，新创建的对象会从这个原型对象上继承属性。

#### call() 和 apply() 方法

这两个方法使我们可以间接的调用函数。这两个方法接受的第一个 argument 是我们要调用的函数的对象，也就是成为 this 的值的调用上下文。若想在对象 o 上调用函数 f，我们可以：

```js
f.call(o);
f.apply(o);
// 两者等同于如下
o.m = f;
o.m();
delete o.m;
```

记得箭头函数会从定义的上下文继承 this 的值，它并不会被 call() 和 apply() 给予的 this 覆盖。所以在箭头函数上调用这两个方法，第一个 argument 就会被忽略。

call() 方法除了第一个之外的 arguments 就是我们希望传入函数的 arguments；apply() 则需要将第二个 argument 定义为数组：

```js
f.call(o, 1, 2);
f.apply(0, [1, 2]);
// 两者等同于如下
o.m = f;
o.m(1, 2);
delete o.m;
```

#### bind() 方法

bind() 方法的主要目的便是将一个函数 bind 到一个对象上。在调用函数 f 的 bind() 方法并传入对象 o 时，这个方法会返回一个新的函数。调用这个新函数会以调用 o 的方法 f 的形式调用。

```js
function f(y) {
  return this.x + y;
}
let o = { x: 1};
let f2 = f.bind(o);
f2(2); // 3
```

箭头函数还是一样从其定义的上下文继承 this 的值，所以 bind() 不会改写它。

bind 并不仅可以绑定对象，也可以绑定 arguments，这个对箭头函数也有效：

```js
let sum = (x,y) => x + y;
let sum2 = sum.bind(null, 2); // 将第一个参数绑定为 2
sum2(3); // 5，只用传入第二个参数
sum2(3,4); // 5，更多的参数会被忽略
```

#### toString() 方法

ECMAScript 指定了这个方法需要返回一个符合函数声明句法的字符串。大多数情况下会返回函数的源代码。内嵌函数通产会返回"[native code]"作为函数体。

```js
Function.toString() // 构造函数，返回 "function Function() { [native code] }"
```

#### Function() 构造函数

因为函数是对象，他也拥有构造函数：

```js
const sum = new Function('x','y','return x + y');
// 大致等同于
const f = function(x,y) {return x + y;};
```

构造函数时 JavaScript 函数可以在编译时动态构建。构造函数构造的函数不适用词法作用域（lexical scoping），这意味着它们始终在全局作用域下：

```js
let scope = 'global';
function f1() {
  let scope = 'local';
  return new Function('return scope'); // 使用构造函数
}
function f2() {
  let scope = 'local';
  return function () {return scope}; // 使用函数表达式
}
f1()() // 'global'
f2()() // 'local'
```

### 小结：

这一章的要点：

- 函数可以使用 function 关键字或 => 句法定义
- 函数可以以方法或构造函数的身份被调用
- ... 展开操作符可以用来传入数组元素作为 arguments
- 在函数内部被定义，并且被返回的函数依然可以访问词法作用域，即可以读写其外部函数中的变量。这种使用函数的方式被称为闭包。
- 函数也是对象，所以也可以拥有属性