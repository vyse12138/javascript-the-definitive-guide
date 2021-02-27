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

## 类（Classes）

JavaScript 中的对象已经被介绍过了，我们将每一个对象认为是一个独特的属性的集合，和其他对象不同。如果想要顶一个一类对象时，即它们拥有共同的属性和方法时，我们可以定义一个类（Class）。每一个类的实例都会从类上继承其属性和方法。对于每一个实例，我们可以为其添加新的属性和方法。

在 JavaScript 中，继承是基于原型的。如果两个对象从一个原型对象上继承了同样的属性，我们称它们为类的实例。在 ES6 之后加入的关键字 class 使得创建类变得更为简单了。

### 类和原型（Classes and Prototypes）

在 JavaScript 中，一个类就是从同一个原型对象上继承属性的对象的集合。所以说原型对象便是类的中心点。在前面介绍过 Object.create() 的方法来创建继承原型对象的新对象，我们其实就是定义了一个类。不过通常情况下，一个类需要更多的初始化，而且定义一个可以创建和初始化对象的函数是很常见的手法：

```js
function range(lower, upper) { // 用于构建新对象的工厂函数
  let r = Object.create(range.methods); // 使新对象通过继承原型对象的方法被创建
  r.lower = lower; // 新创建的对象的私有属性
  r.upper = upper;
  return r; // 返回新对象
}
range.methods = { // 使 range 函数的属性 methods 成为原型对象
  includes(x) { // 定义方法
    return this.lower <= x && x <= this.upper;
  },
  toString() {  // 改写 toString() 方法
    return this.lower + '...' + this.upper;
  }
}

let r = range(1,5);
r.includes(3); // true
r.toString(); // '1...3'
```

在上面的例子中：

- 定义了一个工厂函数 range() 用来创建新的 Range 对象
- 在函数的属性 methods 储存了原型对象，用于定义这个类
- range() 函数定义了两个私有属性 upper 和 lower。他们不会被 Range 对象实例共享
- 在原型对象中引用实例的属性时需要用 this 关键字指向那个实例对象


### 类和构造函数（Classes and Constructors）

上面的例子中展示了一种定义类的简单的方法，但并不是最推荐的方法，因为它不包含构造函数（constructor）。构造函数是一个用于初始化新对象的函数。构造函数会在用 new 关键字创建对象时被自动调用。所以说构造函数只需要为新的对象初始化属性即可。一个很关键的特性便是新创建的对象会继承构造函数的原型属性。（正如前几章提到了几乎所有对象都有原型，而只有很少一部分对象拥有原型属性。构造函数正是其一）所以说，所有用同一个构造函数创建的对象都拥有同一个原型，也就会从同一个类上继承方法和属性。改写上面的工厂函数：

```js
function Range(lower, upper) { // 构造函数
  this.lower = lower;
  this.upper = upper;
}
Range.prototype = { // 定义构造函数的原型，每个用构造函数创建的实例都会继承这个原型
  includes(x) {
    return this.lower <= x && x <= this.upper;
  },
  toString() {  
    return this.lower + '...' + this.upper;
  }
}

let r = new Range(1,5); // 创建了一个新的 Range 对象；而非调用 range 函数
r.includes(3); // true
r.toString(); // '1...3'
```

注意上面两个例子的区别：

- 首先，我们将 range() 函数改名成了 Range()，因为构造函数传统来说要用大写开头，而普通函数则以小写开头
- Range() 构造函数使用了 new 关键字调用，而 range() 工厂函数则不用
- Range() 不用格外进行对象的创建，new 关键字在函数调用前就创建好了新对象
- Range() 构造函数中可以通过 this 指向对象实例
- 不使用 new 关键字而直接调用构造函数通常不会正常运作，所以通常由首字母大小写区分构造函数和普通函数
- 使用 prototype 作为原型属性名是必须的，不能使用其他属性名
- 不能使用箭头函数作为构造函数，因为箭头函数不能继承原型，并且 this 指向其定义的上下文而非调用上下文

```js
r instanceof Range; // true 因为 r 继承了 Range 的原型，所以返回 true
Range.prototype.isPrototypeOf(r); // true
```

虽然我们在上面定义了构造函数的原型属性，但其实所用普通函数也拥有原型属性（箭头函数除外），原型属性的值就是原型对象，它拥有一个 constructor 的属性，construcor 的值就是函数对象：

```js
let F = function() {}; // 新的函数对象
F.prototype // 函数的原型对象
F.prototype.constructor // 重新指向了这个原型对象关联的函数
F.prototype.constructor === f // true
```

这个内嵌的原型对象拥有 constructor 属性，这意味着对象通常也会继承这个 constructor 属性：

```js
let o = new F(); // 从上面的 F 类中创建一个实例对象 
o.constructor === F; // true
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/237ba636441a42648043fb58d53afb2f~tplv-k3u1fbpfcp-watermark.image)

上面的图片展示了 Range() 构造函数的内容，但实际上我们前面定义的 Range 类改写了内嵌的原型属性，我们定义的原型属性并没有包含 constructor 属性。我们可以这么改写使其依然包含于原型对象中：

```js
Range.prototype.includes = function(x) {
  return this.lower <= x && x <= this.upper;
};
Range.prototype.toString = function() {  
  return this.lower + '...' + this.upper;
};
```

### 用 class 关键字构造类（Classes with the class Keyword）

从 ES6 开始，JavaScript 终于拥有了其对于类的句法 class 关键字：

```js
class Range { // 使用了 class 关键字
  constructor(lower, upper) { // 将构造函数放在类中
    this.lower = lower;
    this.upper = upper;
  }
  includes(x) { // 方法也被放在类中
    return this.lower <= x && x <= this.upper;
  }
  toString() {  
    return this.lower + '...' + this.upper;
  }
}

let r = new Range(1,5); // 创建了一个新的 Range 对象
r.includes(3); // true
r.toString(); // '1...3'
```

这个方法和前面的构造函数的例子的运行方法是完全一样的，class 关键字并不会对 JavaScript 用原型链进行继承这一特性进行任何改动。即使用了 class 关键字，调用时的方法也是一样的，用 new 构建一个新的对象。class 说到底不过是一个语法糖罢了。

对于 class 句法要注意以下几点：

- 类声明是通过 class 关键字，加类名，加大括号中的本体（class body）组成的
- class body 包含了通过对象字面量来定义的方法，不过不需要使用逗号来分割（虽然看上去很像对象字面量，其实它们并不同，class 不支持用 key-value pair 的形式定义的属性）
- constructor 关键字被用来定义类的构造函数
- 如果类不需要进行初始化，constructor 关键字也可以被省略。一个空的构造函数会被隐式地自动创建
- class body 会默认以严格模式运行
- 类声明不会被提升（hoisted），所以在定义类前无法创建其实例

#### 静态方法

我们可以在 class body 中定义静态函数，我们只需要在定义方法时加上 static 前缀即可。静态方法会作为构造函数的属性而非原型对象的属性：

```js
// 假设以下内容被定义在了上面的 Range 类的例子中
static format() { // 定义在构造函数上的方法
  console.log('lower...upper');
}
Range.format(); // Ok，'lower...upper'
let r = new Range(1,5);
r.format(); // TypeError，r.format 不是一个函数
```

#### Getters，Setters

在 class body 内部，我们也可以定义 getter 和 setter。方法就像我们在对象字面量中定义它们时一样，只不过在 class body 中不需要逗号。

### 为现有的类增添方法（Adding Methods to Existing Classes）

JavaScript 基于原型的继承机制是动态的。这意味着对象可以从原型继承最新的属性，即使原型上的属性在对象被创建后发生了改变。所以对类新增方法只需要简单的加载器原型对象上即可。

我们甚至可以对 Object.prototype 增加属性。这样所有的对象都会有那个新增的属性了。不过不推荐这么做。

### 子类（Subclasses）

在 OOP 中，若类 B 继承了 类 A，我们称 A 为超类（superclass），B 为子类（subclass）。每一个 B 的实例都会继承 A 的方法。B 也可以定义其私有方法，也可以覆写 A 中的方法。通常情况下，B 的构造函数会调用 A 的构造函数以确保 B 初始化成功。

#### 使用 extends 和 super 关键字

在 ES6 及以后，我们可以通过 extends 来创建一个类的子类，我们对内嵌对象也可以这么做：

```js
class MyArray extends Array { // 从数组继承的自定数组
  get first() { // 定义的新的方法
    return this[0];
  }
  get last() {
    return this[this.length - 1];
  }
}
let a = new MyArray(); // 自定数组
a instanceof MyArray; // true a 是 subclass 自定数组的实例
a instanceof Array; // true a 也是 superclass 数组的实例
a.push(1,2,3,4); // a = [1,2,3,4] // 方法会被继承
a.first; // 1 调用自定的方法
a.last; // 4
Array.isArray(a); // true 子类实例也是数组
MyArray.isArray(a); // true 静态方法也会被继承
```

使用 extends 关键字，不仅可以继承原型上的方法，即 MyArray.prototype 会继承 Array.prototype；也可以继承静态方法，即构造函数 MyArray() 会继承构造函数 Array()。

super 关键字是我们可以在子类中调用超类的构造函数：

- 如果用了 extends 关键字，则构造函数中必须使用 super() 来调用 superclass 的构造函数
- 如果在子类中没有定义构造函数，构造函数会被隐式的定义，并且只调用了 super()
- 在构造函数中调用 super() 之前，无法使用 this 关键字。这强调了 superclass 必须在 subclass 之前被初始化


#### 用组合取代继承（ Delegation Instead of Inheritance）

虽然使用 extens 关键字创建子类十分方便，但这并不代表我们需要创建许多的子类。在许多时候，把超类中的方法和属性直接放到类中会更为方便和灵活，当我们不用子类的方法创建类时，相反的而是将其与其他类组合，这种方法被称为组合（composition）。在 OOP 中我们也会经常听到‘组合大于继承’这句话（favor composition over inheritance）。

比如说我们想要创建一个表现接近于 JavaScript Map 类的直方图时，我们会想到使用继承的方法来从 Map 继承方法。我们也可以使用组合的方法把 Map 封装在直方图类中：

```js
class Histogram { // 创建直方图类，不从别的类继承
  constructor() { 
    this.map = new Map(); // 在构造函数中创建一个新的 Map 于其组合
  }
  count(key) {
    return this.map.get(key) || 0; // 封装让其调用实例中 map 的方法
  }
  has(key) {
    return this.count(key) > 0;
  }
  get size() {
    return this.map.size;
  }
  add(key) {
    this.map.set(key, this.count(key) + 1);
  }
  delete(key) {
    let count = this.count(key);
    if ( count === 1) {
      this.map.delete(key);
    } else {
      this.map.set(key, count - 1);
    }
  }
  keys() { return this.map.keys(); }
  values() { return this.map.values(); }
  entries() { return this.map.entries(); }
}
```

上面的例子中，Histogram() 构造函数只不过创建了一个新的 Map 对象罢了，剩下的方法都是用过封装 map 的方法而实现的。这使得这个类的实行变得更为简单和灵活了。因为我们使用组合代替了继承，Histogram 对象也不会是 Map 的实例。在类似于 JavaScript 的弱类型语言中，这已经足够了。正式的继承虽然有时候会很不错，但并不是必须的。

### 小结

这一章的要点：

- 同一个类中的对象实例会从同一个原型对象上继承属性
- 在 ES6 之前，定义类通常需要通过 function 关键字手动定义构造函数，然后让函数的 prototype 属性作为原型对象
- 在 ES6 之后可以使用 class 关键字来创建类，不过这只是语法糖罢了，底层的实现逻辑还是相同的
- 使用 extends 关键字可以定义子类
- 使用 super关键字，子类可以调用其超类的构造函数或覆写超类的方法

## 模块（Modules）

模块化编程的目的就是为了使庞大的项目可以被一个个不同的组件所组成。其中的组件既可以是我们自己编写的，也可以是别的作者开源的代码。模块化的主要目的是为了封装私有的实现细节，只暴露必要的接口，一这种方法使全局命名空间尽可能地整洁。

其实直到近几年之前，JavaScript 一直没有对于模块的内嵌支持，所以对于大型的项目只能使用类，对象，闭包等方法进行一些弱模块化的处理。基于闭包的模块化处理被 Node 采纳，所以产生了 require() 函数用作类模块化的处理，但 Node 环境并为被官方 JavaScript 所采纳。

不过在 ES6 中，模块通过了 import 和 export 关键字被定义。这章介绍了：

- Node require() 实现的模块
- ES6 通过 exprort 和 import 实现的模块

### Node 中的模块（Modules in Node）

在 Node 中编程时，会将代码分到许多个文件中。其原因是这些文件在 fast filesystem 上运行，而非网速很慢的浏览器。

在 Node 中，每一个文件都是一个独立的模块，拥有其私用的命名空间。在不进行 export 和 require 的情况下，文件之间的变量是不会共通的。（Node 13 以后也支持了 ES6 模块，但是大多还是在使用 Node 模块）

#### Node 中的 Exports

Node 定义了一个名为 exports 的全局变量，如果想要 exports 多个值，只需要把它们都定义为 exports 对象上的属性即可：

```js
const sum (x,y) = x + y;
exports.mean = data => data.redue(sum)/data.length; // 将想要导出的值作为 exports 对象的属性
```

但是更多数的情况，一个模块只需要 exports 一个函数或者类，而非一个装满属性的对象。我们可以用 modele.exports 实现：

```js
module.exports = class A2 extends A1 { // 直接导出 A2 这个类
  // class 的内容
}
```

module.exports 的默认值和 exports 指向了同一个对象，所以第一个 exports 的例子中，我们也可以使用 module.exports.mean 代替 exports.mean，或者：

```js
module.exports = { mean }; // 直接 exports 一个对象，将函数添加在中间，对于需要 exports 多个函数就会方便一点
```

#### Node 中的 Imports

Node 中的 imports 使用另一个称为 require() 的函数实现的。这个函数的 argument 就是我们希望 import 的模块名，返回值就是 export 的值。（多数为函数、类或对象)

import 内嵌模块或者在 package manager 中下载的模块时，只需要传入它们的名字即可：

```js
const fs = require("fs"); // 内嵌 filesystem 模块
const http = require("http"); // 内嵌 HTTP 模块
const express = require("express"); // 下载好的第三方模块 express
```

import 自己定义的模块时，模块名要改成其在 file 中的位置。我们可以使用绝对位置（/），不多通常情况会使用相对位置（./ 或 ../）：

```js
const stats = require("./stats.js"); // 其实 js 后缀名也可以被省略  
let avg1 = stats.mean(data); // 在调用 stats 中的方法是要加上前缀
const { mean } = require("./stats.js"); // 可以用解构赋值，只导入我们想要的方法
let avg1 = mean(data); // mean 被加入 local 命名空间后则不用加前缀
```

### ES6 中的模块（Modules in ES6）

ES6 新增了 import 和 export 关键字用于支持模块化，他和 Node 模块的理念是相同的：每一个文件都是一个新模块，任何其中的变量，函数，对象都不会出现在别的文件中，除非进行 export 和 import。它们的不同只不过是他们的句法。任何在 ES6 模块中定义的语句都会自动进入严格模式，所以在编写 ES6 模块时，我们不会需要显式的写 "use strict"。其实 ES6 模块会进入一个跟样恶的模式，在其中 top-level 中的 this 也会指向 undefined（而非全局对象）。

#### ES6 中的 Exports

我们使用关键字 export 来导出需要的函数、类、常量等：

```js
export const PI = Math.PI; // export 常量
export function degToRad(d) {return d * PI / 100 }; // export 函数
export class Circle {
  constructor(r) {
    this. r = r;
  }
  area() {
    return PI * this.r**2;
  }
} // export 类
```

除了上面这种一个一个 export 的方法之外，我们也可以先将他们声明，在最后一起 export：

```js
export { Circle, degToRad, PI }; // 一起 export，虽然句法看上去接近对象字面量，但并不是
```

在只要 export 一个值时（通常时函数或者类），我们会使用 export default 代替 export：

```js
export default class A {
  // code
}
```

使用 default export 会更容易 import，所以在只有一个值时，尽量使用 export default。

需要注意的是，我们只能在 top-level 中 export，而不能在类，函数，条件或循环等中进行 export。这使得静态统计更为便捷，因为每一个模块在每一次运行时都会 export 同样的值。

#### ES6 中的 Imports

import 关键字使我们可以导入被 export 的值，其句法如下：

```js
import A from './A.js'; // A 的值就是 default export 的值
import { mean }  from  "./stats.js"; // mean 的值就是前面 export 的函数，类似于解构赋值
import * as stats from "./stats.js"; // import stats文件中所有被 export 的值
import "./test.js"; // import 一个没有 export 值的文件，它会且只会在第一次被 import 时运行一次
```

被 import 赋值会成为一个常量，就像是被 const 赋值一般，所以无法被更改。同 export 一样，import 也只能出现在 top level。虽然把 import 放在模块的顶部已成为了一个世界通用的规范，但这并不是必需的，import 也会被提升（hoisted）到顶部。

import 会从一个字符串字面量获得值（上面的 './A.js'）。我们不能使用别的值或变量，也不能使用反引号。在浏览器中，这个字面量会指向用来 import 的 URL；在 Node 或者其他打包工具时，则是文件的路径（即使是在同一文件夹下，也需要使用 ./ 来代表路径，而不能直接输入其文件名）。

 #### 对 Imports and Exports 进行改名
 
 如果两个模块 export 了两个拥有同一个名称的不同值，我们需要对他们其中至少一个进行改名。同样的，对于 import 也是如此。我们可以使用 as 关键字：
 
```js
import { render as renderImg } from './a.js'; // render 同名了，改成 renderImg
import { render as renderText } from './b.js'; // 同上
```

对于 export 进行改名也是可行的，但是并不常见。因为在遇到这种情况时，还不如直接将需要 export 的值进行更有意义的命名。对于 export 也是使用 as 关键字：

```js
export {
  render as renderImg, // 将 render 改名成 renderImg
  display as displayImg，
  Math.sin as sin // SyntaxError，as 左边不能是表达式，必须是单一的 identifier
}
```

#### Re-Exports

假如在某一个模块中 export 了两个函数，但是大多是情况下我们只需要其中一个时，我们可以将它们定义到单独的文件中。这样的话，在只 import 一个函数的情况下，我们的 import 方法会变得更为简洁。

但如果我们在将他们放入单独的文件后又需要同时 improt 它们的话该怎么做呢？我们可以把他们 import 进同一个文件之后在进行 export：

```js
import { a } from './a.js';
import { b } from './b.js';
export { a, b };
```

其实在 ES6 中还有更为简单的句法被称为 re-export，使用方法是 export from 句法：

```js
export { a } from './a.js';
export { b } from './b.js';
```

使用 re-export 时也可以用 as 进行改名。

#### 网络上的 JavaScript 模块

在 2020 年初时，许多 ES6 编写的 production code 依然需要用如 webpack 之类的打包工具进行打包。这么做虽然也有弊端（在某些时候，使用多个小模块代替大的打包文件可以更好的利用缓存），不过整体来说，打包过的代码会具有更好的性能。不过随着未来网速的增加，以及浏览器对 ES6 更好的支持，这一点可能会改变。

虽然目前打包工具在 production 时很重要，但是在 development 的环境下时是不需要的，因为现在所有的浏览器都拥有对 module 的支持。因为模块的执行发方式和非模块代码相差巨大，他其实也改变了 HTML。如果想要对浏览器直接进行 import，我们会需要用到改动过的 \<script type='module'\> 标签。

```html
<script type="module">import "./main.js";</script>
```

使用这种标签其实等同于拥有 defer 特性的 script 标签。这意味着它会在 HTML 语道 script 标签时直接开始解析式（但是代码的执行需要在解析完毕之后），在解析完成后，会以出现在 HTML 中的顺序进行执行。

module 的 script 还有和普通 script 的不同便是其同源特性。在普通的 script 中，我们可以加载任何网络服务器上的 JavaScript 文件，但是 module 的 script 只允许进行同源的加载，或者拥有正确的 CORS 请求头来进行跨域加载。

### 使用 import() 进行动态加载

我们前面所见的 import 和 export 都是完全静态的，所以 JavaScript 编译器可以在不运行代码的情况下直接进行加载。使用静态加载可以保证 import 进模块的值会在该模块运行前就被准备好。

在网络上，代码则是通过网络而非 filesystem 进行传输的，而且传输后，代码经常会在 CPU 相对较弱的手机上执行。这时的静态 import 就不适用（因为需要将全部程序加载完才能运行）。

所以在很多时候，web app 只需要先加载可以渲染出第一页的代码即可。这样，用户可以直接进行交互，然后再加载出剩余的代码。浏览器对于动态加载的方法很简单，只需要动态的使用 DOM API 传入一个新的 script 标签即可。

虽然动态加载已经存在了很久了，不过并没有成为 JavaScript 的一部分。直到 ES2020，动态加载 import() 加入了。我们只需要传入一个模块作为 argument，它会返回一个 Promise 对象是我们可以进行异步的加载。在 Promise 完成时，他会产出一个等同于静态 import 加载的对象：

```js
import * as stats from './stats.js'; // 静态加载
import('./states.js').then(stats => { // 动态加载
  let avg = states.mean(data);
}
async analyzeData(data) { // 使用 async 和 await 进行封装
  let stats = await import('./stats.js');
  return {
    avg : stats.mean(data)
  };
}
```

在使用 import() 时，其 argument 不再一定是一个字符串字面量了，也可以是一个产出字符串的表达式。虽然 import() 的形式很像函数调用，但其其实是一个操作符。其原因是在加载 URL 时会进行一些无法在函数中运行的语句。除了浏览器之外，其实类似 webpack 之类的打包工具也可以使用动态 import() 

#### import.meta.url

在 ES6 模块中，import.meta 指向了包含当前运行模块的元数据（metadata）的对象。这个对象的 url 属性就是模块加载的 url。（在 Node 没有 import.meta.url 但可以使用 file:\/\/URL)

import.meta.url 的主要用法就是去指向该模块中储存的图片或文件。使用 URL() 使用绝对 import.meta.url之类的绝对 url 会方便。

### 小结：

这一章的要点：

- 模块的目的时隐藏实现时的许多细节，而只暴露尽可能少的接口
- 在 JavaScript 早期，模块化只能通过调用函数表达式实现
- Node 增加了其对模块的支持，使用 require() 和 module.exports 和 Exports 对象进行 import 和 export
- 在 ES6 中，JavaScript 获得了其对模块化支持的关键字 import 和 export
- 在 ES2020 中，新加入了动态 import() 

## JavaScript 标准库（The JavaScript Standard Library）

某些数据类型，例如数字、字符串、对象、数组，它们都是 JavaScript 中的基础，我们可以将它们认为是 JavaScript 语言本身的一部分。在这一章中，许多其他的很重要的，但是并不是 JavaScript 基础的 APIs。它们被定义成了 JavaScript 的标准库（Standard library）：JavaScript 的内嵌的一些类和函数，在浏览器和 Node 中都可用。其中主要包括了：

- **Set 和 Map** 类，用来用来表示值的集合和两个集合之间的映射
- **TypedArrays**，是一种用数组表示二进制数据的类数组对象
- **Regular expression 和 RegExp** 类，用于表示文本的格式
- **Date** 类用于表示日期和时间
- **Error** 类和其多样的子类，JavaScript 抛出的异常就是 Error 类的实例
- **JSON** 对象，用于构建和解析 JavaScript 数据解构
- **Intl** 对象，用其定义的类可以用于本地化 JavaScript 程序
- **Console** 对象，其方法可以返回字符串用于 debug 和 log
- **URL** 类，使解析和修改 url 变得更为简单
- **setTimeout()** 和其他相关的函数，用于控制函数的执行时间

### Set 和 Map（Sets and Maps）

JavaScript 的对象是一个十分灵活的数据类型，它可以用于将值映射到字符串上（属性名）。如果我们忽略它的值，那么对象就成了字符串的集合了。不过这样的话，仍然会有些许限制。比如对于字符串有限制（某些符号不能使用）；又比如他依然会从对象的原型对象上继承某些不必要的方法。

所以在 ES6 中，真正的 Set 和 Map 类被加入了。

#### Set 类

set 和数组一样是值的集合，不过它和数组不同，set 没有顺序和索引，而且不能有重复的值：对一个值，我们只知道他是否在 set 内，而无法判断它出现了几次。

我们可以通过 Set() 构造函数来创建一个 Set 对象：

```js
let s1 = new Set(); // 一个新的空 set
let s2 = new Set([1,s1]); // 一个拥有两个元素的 set
```

对于构造函数 Set() 的 argument，他并不一定需要一个数组，任何可遍历对象（包括 Set 对象）都可以成为 argument：

```js
let s3 = new Set(s1); // 一个新的，复制了 s1 中每一个元素的 set
let s4 = new Set('abbccccdddd'); // 四个元素的 set，包括 "a", "b", "c", "d"
```

set 拥有一个 size 属性，等同于数组的 length 属性，返回 set 中的元素数量：

```js
s4.size // 4
```

在创建 set 时，我们并不一定要同时进行初始化，我们可以新增或者移除其中的元素使用 set 上的方法。不过要记住的是，因为 set 不包含重复元素，所以在加入重复元素时并不会产生效果：

```js
let s = new Set(); // 新建空 set
s.size; // 0
s.add(1); // 加入一个数字作为元素
s.size // 1
s.add([1,2,3]); 加入一个数组作为元素
s.size // 2
s.delete(1) // true，删除成功 
s.size // 1
s.delete(4) // false，没有 4，删除失败
s.delete([1,2,3]); // false，set 中数组的引用不同
s.size // 1
s.clear(); // 删除所有元素
s.size // 0
```

需要注意的是： 

- add() 方法只能接受一个 argument，即使传入数组，也只不过是将数组直接加入 set 作为元素。add() 的返回值是调用其的 set，所以可以使用 chained 方法调用：

  ```js
  s.add(1).add(2).add(3);
  ```

- delete() 方法也只能接受一个 argument，然后删除一个元素，不过返回值是一个布尔值，用来表示是否成功删除

- set 的元素是通过全等 === 来判断的，一个 set 可以同时包含 1 和 '1'，因为它们并不全等。对于对象也是通过全等进行比较的，只两个引用着同一个对象的值才可以成为全等

set 还有一个很重要的方法是使用 has() 来判断其是否包含一个值：

```js
let s = new Set([1,2,3,4]);
s.has(2) // true
s.has('2') // false
s.has(5) // false
```

因为 set 不存在重复的元素，所以其 has() 方法的运行速度是被优化过的。has() 会比 includes() 快很多。所以使用数组来表示 set 对象会比使用 Set 来慢得多。

Set 类也是可遍历的，所以可以使用 for of 来遍历其所有元素，也可以使用展开操作符来将其转换成数组。虽然 set 通常被定义为未经排序的集合，但是 JavaScript 的 Set 类其实记住 Set 元素加入时的顺序，然后 以此作为遍历顺序。除了使用 for of 之外，Set 对象上也有 forEach() 方法用于遍历：

```js
let s = new Set([1,2,3,4]);
let sum = 0, product = 1;
for (let element of s) { // 使用 for of 遍历 set
  sum += element
}
sum // 10
s.forEach(element => { // 使用 Set 上的 forEach 方法遍历
  product *= element;
});
product // 24
```

#### Map 类

Map 对象表示了值的集合，这些值被称为 keys。每一个 key 会对应另一个值。简单地说，map 就像是一个数组，只不过使用了 keys 来代替索引。和数组一样，查找一个值的速度很快（但不如数组），而且和 map 大小无关。我们可以使用 Map() 构造函数来创建一个新的 map：

```js
let m1 = new Map(); // 创建一个新的空 map
let m2 = new Map([ // 使用字符串作为 key，数字作为值的 map
  ['a',1],
  ['b',2]
]);
```

Map() 构造函数的可选 argument 需要是一个包含两个元素 \[key, value\] 的可遍历对象，或者一个其他的 map：

```js
let m3 = new Map(m2); //  复制另一个 map
let o = { x:1, y:2}; // 两个属性的对象
let m4 = new Map(Object.entries(o)); // 这样也可以，因为 Object.entries 返回可遍历的 [key, value]
```
Map 对象上也有许多好用的方法：

```js
let m = new Map(); // 创建新的空 map
m.size // 0 没有 key
m.set('a',1); // 加入 key 为 'a'，value 为 1 的 pair
m.set('b',2); // 加入 key 为 'b'，value 为 2 的 pair
m.size // 2 有两个 keys
m.get('b'); // 1，取得 key 'b' 所对应的值
m.get('c'); // undefined
m.has('b'); // true，判断是否有 key 'b'
m.has(2); // false，只能判断 key，不能判断值
m.delete('a') // true，删除 key 'a' 和其的值
m.delete('a') // false，'a' 已经被删除了
m.clear() // 删除所有元素
```

和 set 一样，所有 JavaScript 的值都可以成为 key 或者 value，这也包含了 null、undefined、NaN 和所有引用类型（如对象）。使用 has() 和 get() 时也是通过全等 === 进行比较的。

Map 对象也是可遍历的，每一个遍历值会返回 \[key, value\]。对 Map 对象使用展开操作符会得到类似于我们传入构造函数那样的 array of arrays：

```js
let m = new Map([["x", 1], ["y", 2]]);
[...m] //  [["x", 1], ["y", 2]]
for (let [key, value] of m) { // 使用 for of 遍历 map，顺序也是插入顺序
  ...
}
m.forEach((value, key) => { // 使用 Map 上的 forEach 方法遍历，顺序也是插入顺序
  ... // 注意，value 是第一个参数，而 key 是第二个
});
```

如果我们只想遍历 map 的 keys 或着 values 可以使用 keys() 或 valuse() 方法，它们会返回只包含 keys 或者 values 的可遍历对象。（还有 entries() 方法用于返回 key value pair）：

```js
let m = new Map([["x", 1], ["y", 2]]);
m.keys(); // MapIterator {"x", "y"}
m.values(); // MapIterator {"1", "2"}
m.entries(); // MapIterator {"x" => 1, "y" => 2}
// 可以对可遍历对象使用展开操作符 ... 
[...m.keys()] // ["x", "y"]
[...m.values()] // ["1", "2"]
[...m.entries()] // [["x", 1], ["y", 2]]
```

#### WeakMap 类和 WeakSet 类

WeakMap 类是 Map 类的变种（而非子类)。他不会阻止它的键和值被垃圾回收程序（Garbage collection）回收。垃圾回收是一个 JavaScript 编译器重新取回不再可以被使用的内存的过程。普通的 amp 会对器键值持有强引用，所以只要 map 存在，键值就一定存在，不论是否有其他的地方在引用他们。WeakMap，则只拥有‘弱’引用，如果键值不再 reachable，他们的存在便不再重要，会被垃圾回收掉。

WeakMap() 构造函数和 Map() 构造函数十分接近，不过:

- WeakMap 的 keys 必须是对象或者数组；因为基本类型并不在垃圾回收范围之内
- WeakMap 不可遍历，没有定义 keys()、values()、entries()、forEach()方法。因为如果它可以被遍历，它的 keys 就会被使用
- 同理，WeakMap 也不用有 size 属性，因为 size 属性会在垃圾回收后改变

使用 WeakMap 的主要目的就是为了防止内存泄漏（通常会在缓存时使用）。

WeakSet 对于 Set 的改变基本等同于 WeakMap 对于 Map 的改变。

### 类型化数组和二进制文件（Typed Arrays and Binary Data）

一般的 JavaScript 数组可以拥有任何类型的元素，并且长度也是动态的。JavaScript 的实现对它们进行了很多的优化，所以一般情况下都非常快。尽管如此，JavaScript 的数组和更底层语言（如 C 或 Java）中的数组类型还是有很大区别的。

类型化数组（Typed arrays） 使在 ES6 引入的新特性。它们更接近于底层语言。Typed arrays 从技术层面上讲其实并不是数组（Array.isArray() 会返回 false），但是它们拥有所有数组所拥有的方法。Typed arrays 和普通数组的区别主要是：

- typed array 的元素均为数字，并且可以指定数字的类型（signed integer、unsigned integer、IEEE-754 浮点数）和长度（ 8 bits 到 64 bits）。
 - 在创建 typed array 时，必须指定其长度；typed array 的长度在定义以后不能再改变
 - 在创建 typed array 时，所有的元素都被初始化为 0

#### Typed Array 类型

JavaScript 并没有定义 typed array 类，其原因是这一共有 11 种不同的 typed array，他们拥有不同的元素类型和构造函数：

- **构造函数** --- 数字类型
- **Int8Array()** --- signed bytes
- **Uint8Array()** --- unsigned bytes
- **Uint8ClampedArray()** --- unsigned bytes without rollover
- **Int16Array()** --- signed 16-bit short integers
- **Uint16Array()** --- unsigned 16-bit short integers
- **Int32Array()** --- signed 32-bit integers
- **Uint32Array()** --- unsigned 32-bit integers
- **BigInt64Array()** --- signed 64-bit BigInt values (ES2020 新增)
- **BigUint64Array()** --- unsigned 64-bit BigInt values (ES2020 新增)
- **Float32Array()** --- 32-bit floating-point value
- **Float64Array()** --- 64-bit floating-point value (也就是 JavaScript 中的 Number）

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d56251a0c334cc4b71cff018bed9b27~tplv-k3u1fbpfcp-watermark.image)

由 Int 开始命名的类型是 1，2，4 字节（8，16，32 位）的有符号整数的数组；由 Uint 开始命名的则是同长度的正整数的数组；而 BigInt 类型则是拥有 8字节（64位），表示了 JavaScript 中  BigInt 值的数组；Float64Array 的元素就是 JavaScript 中的数字；Float32Array 中的元素则拥有更低的进度和更短的长度（C 和 Java 中的 float）；Unit8ClampedArray 则是 Uint8Array 的变种，它们都可以用来表示 0 到 255 的值。如果想起存储了大于 255 或者小于 0 的值时，Uint8Array 会进行回绕（wraps around），所以会得到不一样的值；而 Uint8ClampedArray 则会进行类型检验，若大于 255 或小于 0 则会编程 255 或 0（这会在 canvas 的底层 API 操作像素颜色时被用到）。

每一个构造函数都拥有 BYTES_PER_ELEMENT 的属性用来表示其字节数。


#### 创建 Typed Array

创建 typed array 最简单的方法就是调用其构造函数，并传入长度作为 argument：

```js
let bytes = new Uint8Array(1024); // 1024 字节
let matrix = new Float64Array(9); // 3x3 矩阵
let point = new Int16Array(3); // 3D 中的一个点
let rgba = new Uint8ClampedArray(4); // 4 字节的 RGBA 像素值
let sudoku = new Int8Array(81); // 9x9 的数独
```

创建完成后，其中的属性都被保证初始化为 0.0 或者 0.0n（BigInt）。如果在构造时就已经知道想要传入的值的话，也可以直接定义值。每一个 typed array 的构造函数都拥有 from() 和 of() 的静态方法，使用起来和数组的一样：

```js
let white = Uint8ClampedArray.of(255,255,255,0); // RGBA 中的 opaque white
let white2 = Uint32Array.from(white); // 转换成 32 位整数
```

最后，这还有一种创建 typed array 的方法，这需要使用到 ArrayBuffer 类。一个 ArrayBuffer 是一个指向内存的不透明引用。我们可以使用构造函数来创建：

```js
let buffer = new ArrayBuffer(1024*1024);
buffer.byteLength // 1024*1024，一兆内存
```

我们并不能在 ArrayBuffer 类上直接进行字节的读写，不过我们可以在其内存中创建 typed array，然后对内存进行读写。我们只需要在调用 typed array 构造函数时传入 ArrayBuffer 作为第一个argument，array buffer 中 一个字节的 offset 作为第二个 argument，typed array 的长度作为第三个 argument 就可以了。第二个和第三个的 argument 是可选的，如果全部省略，则会用 array buffer 中所有内存创建该 typed array；如果省略第三个，他会从创建从 start index 到 end 中所有剩余内存的 typed array。

#### 使用 Typed Array

在 typed array 创建完成后，我们可以对其进行读写，就如同我们读写数组一样。只不过读写速度和内存使用率会根据你定义的 typed array 而大大缩小。虽然 typed array 并不是数组，不过它们重新实行了几乎所有数组的方法。

英文 typed array 的长度是固定的，所以 length 是一个只读的值，然后会改变长度的方法也没有被实现。（例如 push(), pop(), shift(), splice()）

#### Typed Array 的属性和方法

除去基础的数组方法之外，typed array 还实现了一些它们私有的方法。set() 方法可以设置多个 typed array 的元素：

```js
let bytes = new Uint8Array(1024); // 一个 1k 的 buffer
let pattern = new Uint8Array([1,2,3,4]); // 一个四字节的数组
bytes.set(pattern); // 将 bytes 的开始的字节改成 pattern 中的字节
bytes.set(pattern, 4); // 将 bytes 的从 offset（第二个 argument）开始的字节改成 pattern 中的字节
bytes.slice(0,10); // 返回新的 typed array [1,2,3,4,1,2,3,4,0,0]
```

typed array 还实现了一个 subarray() 的方法，它的 arguments 和 slice() 的一样，用法也十分酷似。不过它们是有区别的。 slice() 会返回一个新的，独立的 typed array。这意味着它不会和其原来的 typed array 公用同一段内存。而 subarray() 则不同，它不会在内存中创建新的 typed array，而是直接返回其原数组中的那一段的引用：

```js
let a = new Int16Array([1,2,3,4,5]); // 五个 short integer
let aSub = a.subarray(1,4); // [2,3,4]
let aSlice = a.slice(1,4); // [2,3,4]

a[2] = 33; // a = [1,2,33,4,5]
aSub; // [2,33,4] subarray 会根据原数组的值而改动
aSlice // [2,3,4] slice 则不会
```

每一个 typed array 都拥有三个关于其存在的 buffer 的属性：

```js
aSub.buffer // ArrayBuffer 对象用于储存 typed array
aSub.buffer === a.buffer // 指向同一个 buffer
aSub.byteOffset // 2，subarray 的开始位置
aSub.byteLength // 6，subarray 的字节数（三个 16 位整数，每一个使用 2 字节）
aSub.buffer.byteLength // 10 底层的 buffer 有 10 字节
a.length * a.BYTES_PER_ELEMENT === a.byteLength // true，根据长度和元素字节数计算字节长度
```

#### 字节序和 DataView 类

出于性能考虑，typed array 会使用其底层硬件的原生字节序（endianness）。而字节序有两种：

- little-endian，typed array 的字节会从低位到高位进行排序
- big-endian，typed array 的字节会从高位到低位进行排序

目前大多是 CPU 是使用 little-endian 的，但是许多网络协议或者二进制文件则需要 big-endian 排序。在使用从网络上下载的 typed array 表示的文件时，你并不知道其字节序，这时候我们就需要用到 DataView 类了。它可以让我们指定读写的顺序：

```js
// 假设我们有一个名为 bytes 的 typed array
let view = new DataView(bytes.buffer, bytes.byteOffset, bytes.byteLength);
let int1 = view.getInt32(0); // 根据 big-endian 从第 0 个字节读取第一个整数
let int2 = view.getInt32(4,false); // 根据 big-endian 从第 4 个字节读取第二个整数
let int3 = view.getInt32(8,true); // 根据 little-endian 从第 8 个字节读取第三个整数
view.setInt32(8,int,false); // 以 big-endian 写入 
```

DataView 对 10 种 typed array 各定义了一个 get 方法，名为 getInt16(), getUint32(), getBigInt64() 等等。第一个 argument 是其 byte offset；除去 getInt8() 和 getUint8() 之外，所有方法还有第二个可选的布尔值 argument，若为 true，用 little-endian 顺序，若为 false，用 big-endian 顺序。

DataView 对 10 种 typed array 也各定义了一个 set 方法。第一个 argument 是写入的开始的 offset；第二个 argument 是写入的值；除去 getInt8() 和 getUint8() 之外，所有方法还有第三个可选的布尔值 argument，若为 true，用 little-endian 顺序，若为 false，用 big-endian 顺序。

typed arrays 和 DataView 类给了我们在解压 ZIP 文件，或从 JPEG 文件提取元数据的所有必须的工具。

### 正则表达式（Pattern Matching with Regular Expressions）

正则表达式是用来表示文字符串 pattern 的对象。JavaScript 中的 RegExp 类表示了正则表达式，String 和 RegExp 都有许多调用正则表达式的方法。

#### 定义正则表达式（Defining Regular Expressions）

在 JavaScript 中，正则表达式是通过 RegExp 对象表示的。RegExp 对象可以通过 RegExp() 构造函数来创建，或者直接使用正则字面量句法。正如字符串字面量是在一对引号中定义的，正则表达式字面量是在一对斜杠（slash /）中定义的：

```js
let pattern1 = /a/; 
let pattern2 = new RegExp('a'); // 效果同上
```

上面的代码创建了一个新的 RegExp 对象，然后将其赋值给了一个变量。

正则表达式会包含一系列的字符，大多是是英文字符，其表示了我们希望匹配的字符。就比如说 /a/ 会匹配所有包含子字符串 'a' 的字符串。除了需要匹配的字符串外，其中还包了特殊的元字符，用于表示匹配时的特征。在一对斜杠之后（或构造函数的第二个 argument）还可以包括一个或多个标志（flags）。

##### 字面量字符（LITERAL CHARACTERS）

在正则表达式中，每个英文字母和数字的字面量都能匹配他们本身，JavaScript 也支持某些非字母或者数字的字面量，它们通过在前面加上一个反斜杠（backslash \）来表示。具体的支持如下：

- **字符** --- 匹配
- **字母和数字** --- 其本身
- **\0** --- 空字符，即空格（\u0000）
- **\t** --- Tab（\u0009）
- **\n** --- 换行符（\u000A）
- **\v** --- 垂直 Tab（\u000B）
- **\f** --- 换页符（\u000C）
- **\r** --- 回车符（\u000D）
- **\xnn** --- 两位的十六进制数 nn 表示的字符（如 \x0A 等同于 \n，因为 \n 是 \u000A）
- **\uxxxx** --- 四位的十六进制数 nn 表示的字符（如 \u000A 等同于 \n）
- **\u{n}** --- 十六进制数表示的 UTF-16 代码单元（只能在有 u flag 的正则表达式中使用）
- **\cX** --- 匹配字符串中的一个控制符

许多标点符号在正则中有特殊的含义，他们是：

```js
^ $ . * + ? = ! : | \ / ( ) [ ] { }
```
他们的含义会在等一下被介绍，其中某些只有在指定的上下文中有着特殊的含义，在某些情况又被当作成普通的字面量。总而言之，如果想要在正则匹配中加入这些符号，我们须在其前面加上反斜杠 \ 作为前缀。 虽然某些符号不使用反斜杠 \ 也能正常进行匹配，但是加上了反斜杠 \ 也不会对其造成影响。不过数字和字母则不同，加上反斜杠 \ 可能会带来不同的结果。如果想要匹配一个反斜杠，则需要用 \\ 表示。

##### 字符类（CHARACTER CLASSES）

单一的字符字面量放在一对中括号的情况下可以被组合成字符类，字符类会匹配其中包括的每一个字符。所以正则表达式 /\[abc\]/ 会匹配字母 a，b，c 中的任意一个。

我们也可以反方向进行定义 /\[^abc\]/，这样在中括号中的第一位加上一个插入符（^)可以使该正则表达式匹配所有非 a，b，c 的字符。

字符类也可以通过加上连字符（-）来表示一个 range。比如 /\[a-z\]/ 会匹配所有小写字母；/\[a-zA-Z0-9\]/ 会匹配所有字母或者数字。

因为许多类似的字符类会被经常使用，所以它们也用特殊的句法来表示：

- **字符** --- 匹配
- **[...]** --- 任何一个中括号内的字符
- **[^...]** --- 任何一个非中括号内的字符
- **.（小数点）** --- 匹配除去换行符以外的任何字符（在拥有 s flag 时，他也会匹配换行符）
- **\w** --- 任何 ASCII 单字字符，等同于 /\[a-zA-Z0-9_\]/（包含下划线 _ ）
- **\W** --- 任何非 ASCII 单字字符，等同于 /\[^a-zA-Z0-9_\]/
- **\s** --- 任何 unicode 的空白字符，包括空格、Tab、换页符、换行符
- **\S** --- 任何非 unicode 的空白字符
- **\d** --- 任何 ASCII 数字，等同于 /\[0-9\]/
- **\D** --- 任何非 ASCII 数字的字符，等同于 /\[^0-9\]/
- **\[\b\]** --- 退格 backspace 的字面量

##### 重复（REPETITION）

基于前面的句法，我们现在可以用 /\d\d/ 来描述一个两位数，或者 /\d\d\d\d/ 来描述一个四位数。但其实我们还有更简便的方法，比如，对于一个可能包含任何长度的数字，或者数字和字符的结合来说时，正则表达式其实拥有特有的句法来表示重复的次数。这些用于表示重复次数的字符会一直紧跟在它们需要描述的 patter 之后：

- **字符** --- 匹配
- **{n, m}** --- 匹配其前面的字符至少 n 次，但不大于 m 次
- **{n,}** --- 匹配其前面的字符至少 n 次
- **{n}** --- 匹配其前面的字符 n 次
- **?** --- 匹配其前面的字符 0 次 或 1 次，即 {0, 1}
- **+** --- 匹配其前面的字符至少 1 次，即 {1, }
- **\*** --- 匹配其前面的字符至少 0 次，即 {0, }

```js
let p1 = /\d{2,4}/; // 匹配两位数到四位数
let p2 = /\w{3}\d?/; // 匹配一个长度为三的单词加上一个可选数字
let p3 = /\s+hello\s+/; // 匹配字符拥有开头和结尾空白的 'hello' 
let p4 = /[^(]*/; // 匹配任意数量的非左括号字符
```

需要注意的时，任何表示重复次数的字符只会影响其前一个字符或字符类，若需要用重复数字来影响更多的字符，则需要使用括号将它们包裹。这会在接下来被介绍。

在使用 * 和 ? 要注意它们会从零开始匹配，所以即使没有那个需要匹配的字符，它们也会成功匹配。比如 /a*/ 也能匹配到 'bbb'。

##### 非贪婪重复（NON-GREEDY REPETITION）

在使用前面的重复匹配模式中，它们会尽可能多次的匹配。即使已经有匹配了，它们也会接着进行匹配，我们称之为贪婪（greedy）重复。

定义一种非贪婪重复其实也十分简单，只需要在原本的重复字符后加上一个问号 ? 即可。比如 ??, +?, \*?, {1,5}? 等等。 比如在对 'aaa' 使用 /a+/ 匹配时，三个字符都会被匹配；但是 /a+?/ 则只会匹配 'aaa' 中的第一个字母 a。 

##### 选项，组合和引用(ALTERNATION, GROUPING, AND REFERENCES)

正则表达式的语法中也包含了用来进行选项匹配，

在进行选项匹配时，| 字符被用于分开选项。比如 /ab|cd|ef/ 可以匹配字符串 'ab' 或 'cd' 或 'ef'；/\d{3}|[a-z]{4}/ 可以匹配三个数字或者四个小写字母。需要注意的时匹配是通过从左往右的顺序进行的，只要匹配到就会返回匹配值，不论右边是否有更好的匹配。比如用 /a|ab/ 匹配 'ab' 时，匹配到的会是 'a'。

在正则表达式中，括号有许多的目的，其中之一便是将多个分开的字符或字符类整合到一起作为一段子表达式，然后可以被 |, ?, + 等符号当成一个元素处理。比如 /java(script)?/ 会匹配 'java'，以及之后可选的 'script'； /(ab|cd)+|ef/ 会匹配 'ef' 或者 至少重复一次的任意字符串 'ab' 或 'cd'。

括号的另一个目的则是在 pattern 中定义 subpatterns。在正则表达式匹配成功时，我们可以从中提取一段字符串。比如在匹配一段字母加一段上数字的时候，我们也想同时提取其中的数组，我们可以使用 /[a-z]+(\d+)/，用括号来包裹数字部分，这样在匹配时，数字部分也会被匹配。

在使用括号定义完子表达式后，我们还可以在同一正则表达式中重新引用那一段子表达式。这可以使用反斜杠 \
加上一个数字来表示。数字用于表示括号中子表达式的位置。比如 \1 会指向第一个括号中的子表达式，\2 会指向第二个。因为子表达式也可以内嵌在子表达试中，所以对于顺序定义是其左括号出现的顺序。

```js
let r = /([Jj]ava([Ss]cript)?)\sis\s(fun\w*)/;
```

比如上面的例子中，子表达式 ([Ss]cript) 就出现在了第二位，我们可以用 \2 来引用它。但其实在引用时，他并不是以用其 pattern，而是引用匹配到的文本。所以这个引用可以被用于加强对于两段分开的字符串的限制:

```js
let p = /['"][^'"]*['"]/;
```

比如上面的例子中，会匹配单引号或双引号中的一段字符，但是它并不要求起始的引号和结尾的引号是同样的。如果想要引号相同，则可以这么更改：

```js
let p = /(['"])[^'"]*\1/;
```

其中的 \1 会匹配和第一对括号中相同的文本，它规定了结束的引号必须和起始的引号相同，否则就不匹配。（注意，无法在字符类中引用子表达式，即无法在中括号 [...] 中使用 \1）

我们也可以在不创建数字引用的情况下将正则表达式中的元素组合。要这么做的话，我们需要使用 (?: ... ) 来代替 ( ... )：

```js
let p = /([Jj]ava(?:[Ss]cript)?)\sis\s(fun\w*)/;
```

在这个例子中，子表达式  (?:[Ss]cript) 可以用来组合其中的字符和字符类，所以之后的 ? 字符会对这个子表达式生效；但是它们不会产生数字引用，所以使用 \2 时会直接指向 (fun\w*)。

选项，组合，引用的字符小结：

- **字符** --- 含义
- **|** --- 选项：用于分割选项，只需要匹配其左边或者右边的子表达式即可
- **( ... )** --- 组合：将字符和字符类归类组合到一起，是它们可以以用被 \*, +, ?, | 等字符操控，并且创建引用
- **(?: ... )** --- 无引用组合：将字符和字符类归类组合到一起，是它们可以以用被 \*, +, ?, | 等字符操控
- **\n** --- 引用：其中 n 为数字，引用被括号分割的子表达式，子表达式的顺序是根据其左括号出现的顺序而定的，无引用括号不会创建引用

在 ES2018 中，对于正则表达式有了一个新的特性，使其可以使用名为 named capture groups 来给每一个括号中的子表达式命名，并且方便之后的引用：

```js
let p = /(?<city>\w+) (?<state>[A-Z]{2})/; // 对每一个子表达式用 <> 的形式来命名，增加可读性
let p2 = /(?<quote>['"])[^'"]*\k<quote>/; // 以标签名引用子表达式
```

##### 指定匹配位置（SPECIFYING MATCH POSITION）

在前面的介绍中，许多正则表达式的元素都只能能对字符串中的一个字符进行匹配。但其实这里还有许多字符使我们可以对字符的位置进行匹配。比如说 '\b' 字符，它对一个 ASCII 单词进行边界（boundary）的匹配；或者 ^ 表示了 pattern 要在字符串的开始；或者 $ 表示了 pattern 要在字符串的结束。

比如说要去匹配单行的单词 'JavaScript' 我们可以使用 /^JavaScript$/。

指定匹配位置的字符有以下这些：

- **字符** --- 含义
- **^** --- 匹配字符串的开始（在有 m flag 时，匹配一行的开始）
- **$** --- 匹配字符串的结束（在有 m flag 时，匹配一行的结束）
- **\b** --- 匹配一个单词的边界，一个 \w 字符的开头或者结尾为一个 \W 字符
- **\B** --- 与 \b 相反
- **x(?= y)** --- 向前断言：x 在被 y 跟随时，匹配 x
- **x(?! y)** --- 向前否定断言：x 在不被 y 跟随时，匹配 x
- **(?<= y)x** --- 向后断言：x 跟随在 y 之后时，返回 x
- **(?<! y)x** --- 向后否定断言：x 不跟随在 y 之后时，返回 x

##### 标志（FLAGS）

每一个正则表达式都可以拥有一个或多个的于其关联的 flag 用于改变其匹配方式。在 JavaScript 中，一共有六个 flags，每一个都由单一的字母表示。flags 会在正则表达式字面量的第二个反斜杠之后定义，或者作为构造函数的第二个 argument 传入。支持的 flags 如下：

**g：** g flag 表示正则表达式是全局的（global）。这意味着他会去寻找所有的匹配值而非在找到第一个后停下。他并不会改变匹配的方法。

**i：** i flag 指定了匹配时不用区分大小写。

**m：** m flag 指定了匹配会在多行（multiline) 模式下进行。这意味着匹配会对每一行进行，所以类似 ^ 和 $ 的字符不仅会匹配字符串的开始和结束，还会匹配行的开始和结束。

**s：** s flag 也和 m flag 一样，经常被使用于多行匹配的情况。通常情况下，'.' 可以匹配除换行符以外的所有字符；在拥有 s flag 时，'.' 也可以匹配换行符。

**u：** u flag 表示 Unicode，他是正则表达式可以对 Unicode 的 codepoints 进行匹配而非 16 位值的字符。（通常情况下可以一直对正则表达式开启这个 flag，否则正则表达式在对于类似汉字或者 emoji 时无法正确匹配）

**y：** y flage 表示正则表达式是执行粘性（sticky）匹配的。他会匹配到字符串的开头或者从其上一个匹配值开始的字符。对于只要寻找一个匹配值的正则表达式，他等同于使用了 ^ 前缀；对于需要找更多匹配值时，他则会比较有用。

这些 flag 可以以任何顺序被组合然后影响同一个正则表达式。

#### 使用正则表达式的字符串方法（String Methods for Pattern Matching）

到目前为止，我们已经知道如何定义正则表达式和它的语法了，但是并没有节是如何在 JavaScript 中使用它们。现在我们会讲一下字符串使用 RegExp 时的 API。

##### search() 方法

字符串一共有四个和正则表达式有关的方法，其中最简单的便是 search()。它会接受一个正则表达式作为 argument，然后返回找到的匹配的位置，或者在找不到时返回 -1:

```js
"JavaScript".search(/script/ui) // 4
"Java".search(/script/ui) // -1
```

如果传入的参数不是正则表达式的话，他会先将其传入正则表达式的构造函数然后再进行匹配；search() 也不支持全局搜索，所以会忽略 g flag。

##### replace() 方法

replace() 方法会进行先找到，后替替换的操作。他会接受一个正则表达式作为第一个 argument，用于替换的字符串作为第二个 argument。如果正则表达式拥有 g flag，则它会替代所有匹配的字符串，若没有 g flag，则只会替代第一个匹配的字符串。在第一个 argument 为字符串的情况下，他不会将其转换成正则表达式，而是直接搜索那个字符串字面量。

```js
'java java'.replace(/java/gu, "JavaScript"); // "JavaScript JavaScript"，传入 g flag，全部替换
'java java'.replace(/java/u, "JavaScript"); // "JavaScript java"，不传入 g flag，替换第一个
'java java'.replace('java', "JavaScript"); // "JavaScript java"，也可以直接传入字符串
```

事实上，replace() 还拥有更为强大的功能。回想一下，我们前面通过括号来定义了子表达式，并且可以通过数字来对他们进行从左到右的引用。如果在用于替代的字符串中出现了 '$n' （n 为数字）的话，它会根据匹配到的字符串来进行替代：

```js
let quote = /"([^"]*)"/g; // 匹配所有双引号中的内容
'He said "hello"'.replace(quote, '$1!!!')  // "He said hello!!!"

let quote = /"(?<quotedText>[^"]*)"/g; // 使用了 name capture group 时也可以
'He said "hello"'.replace(quote, '$<quotedText>!!!') // // "He said hello!!!"
```

我们甚至可以传入一个函数作为用于替代的字符串，这个函数会被调用然后其返回值会作为用于替代的字符串：

```js
"11 times 22 is 242".replace(/\d+/gu, n => (+n).toString(2));
// "1011 times 10110 is 11110010" 这个函数将每一个匹配到的数字转换成了 2 进制数
```

##### match() 方法

match() 方法也是最常见的方法之一了，他会接受一个正则表达式作为 argument（若不为正则表达式则传入正则表达式构造函数进行构造），然后返回一个获得的匹配值的数组，或者在没有找到时返回 null。在正则表达式有 g flag 时，返回所有匹配到的结果：

```js
"11 times 22 is 242".match(/\d+/g); // ["11", "22", "242"]
```

但如果正则表达式没有 g flag 的话，它会找到第一个匹配值，然后返回一个数组。但是这个数组中的元素则和上面的不同，其中第一个元素是找到的字符串，剩余的元素则是其正则表达式括号中的子表达式的匹配值。这个数组也拥有一些出额外的属性：

```js
let urlPatter = /(\w+):\/\/([\w.]+)\/(\S*)/;
let url = 'the url is https://www.abc.com/about/me';
let match = url.match(url);
match // ["https://www.abc.com/about", "https", "www.abc.com", "about/me"]
match.input // 返回输入的文本，"the url is https://wwasdom/about/asd"
match.index // 返回找到匹配时的字符串索引， 11
match.group // undefined
// match.group 在正则表达式包含 named capture groups 时返回一个group 对象，
// group 对象包含了 name  groups name 作为属性名；匹配的文本作为值。
// 若没有使用 named capture groups，返回 undefined
```

##### matchAll() 方法

matchAll() 方法是在 ES2020 新增的方法，它接受一个拥有 g flag 的正则表达式，然后返回一个可以 yields 每一个对 match() 使用非全局正则表达式的返回值的 iterator：

```js
const wordsPatter = /\b\w+\b/gu; //匹配每一个单词
let words = 'how are you';
for (let word of words.matchAll(wordsPatter)) {
  console.log(word.index + ': ' + word[0]);
}
// 0: how
// 4: are
// 8: you
```

##### split() 方法

split() 方法其实也可以接受一个正则表达式：

```js
'1,2,3,4,5'.split(','); // ['1','2','3','4','5']
'1,2,3,4,5'.split(/,/); // ['1','2','3','4','5']
'1,2,3,4,5'.split(/\d/); // ["", ",", ",", ",", ",", ""]
```

#### RegExp 类（The RegExp Class）

##### RegExp 的构造函数

首先要介绍的就是 RegExp 类的构造函数了，他可以接受一到两个字符串作为 arguments，然后构建一个新的 RegExp 对象。第一个 argument 是一个表达正则表达式本体的字符串，即出现在反斜杠之间的部分，第二个 argument 是表示 flags 的字符串，即出现在反斜杠之后的部分：

```js
let p = new RegExp('a','g'); // 效果等同于 let p = /a/g; 
```

在正则表达式是动态的时候，使用构造函数就很有必要了。在传入第一个 argument 时，我们其实也可以直接传入一个正则表达式，这样的目的主要是复制这个正则表达式然后改变它的 flag：

```js
let p1 = /JavaScript/g;
let p2 = new RegExp(p1, "i"); // p2 = /JavaScript/i
```

##### RegExp 的属性

RegExp 对象上拥有以下属性：

- **source：** 只读属性，表示了正则表达式的出现在反引号之间的源字符串
- **flags：** 只读属性，表示了正则表达式的 flag
- **global：** 只读布尔值属性，在拥有 g flag 时为 true
- **ignoreCase：** 只读布尔值属性，在拥有 i flag 时为 true
- **multiline：** 只读布尔值属性，在拥有 m flag 时为 true
- **dotAll：** 只读布尔值属性，在拥有 s flag 时为 true
- **unicode：** 只读布尔值属性，在拥有 u flag 时为 true
- **sticky：** 只读布尔值属性，在拥有 y flag 时为 true
- **lastIndex：** 可读写属性，在拥有 g 或 y flags 的正则表达式上，这个属性的值表示了下一次搜索开始的索引，会在接下来的方法中被用到

##### RegExp 的方法

**test() 方法**

RegExp 类中的 test() 方法是使用正则表达式最简单的方法之一。它接受一个字符串作为 argument，然后在匹配时返回 true，在不匹配时返回 false。


test() 的运作方式很简单，他会直接调用下面的 exec() 方法然后再其返回一个非 null 值的时候返回 true。

**exec() 方法**

RegExp 类中的 test() 方法则是最常被使用的方法。它会接受一个字符串作为 argument，然后将其和正则表达式比较，如果没有匹配值，返回 null，如果有匹配值，它会返回一个等同于调用 match() 进行非全局搜索时的返回的数组。索引为 0 的元素是其匹配值，剩下的元素则是对子表达式的匹配。

不像字符串的 match() 方法，exec() 不论有没有 g flag，都会返回一样类型的数组。所以它也只会返回其中第一个匹配值。在对拥有 g 或者 y flag 的正则表达式进行调用时，它会根据 lastIndex 属性作为其开始查找的位置。

对于新创建的 RegExp 对象，其 lastIndex 属性是 0，每当对其调用 exec() 并且成功的找到一个值时，lastIndex 的值会被更新为匹配值的下一个字符的索引。这可以让我们在循环中多次调用 exec() 然后查找所有匹配值。（在 ES2020 后，使用 字符串的 matchAll() 方法会是一个更好的选择）。

### 日期和时间（Dates and Times）

JavaScript 中的 Date 类是一个用于处理日期和时间的 API。在不传入 argument 的时候调用 Date() 构造函数会返回一个表示当前时间的 Date 对象：

```js
let now = new Date(); 
now // Sat Feb 20 2021 18:57:17 GMT+1100 (Australian Eastern Daylight Time)
```

如果在 argument 的位置传入一个数字，则其会被编译成从 1970 年开始计时的毫秒数：

```js
let d1 = new Date(0); 
d1 // Thu Jan 01 1970 10:00:00 GMT+1000 (Australian Eastern Standard Time)
let d2 = new Date(1000000000000); 
d2 // Sun Sep 09 2001 11:46:40 GMT+1000 (Australian Eastern Standard Time)
```

在传入两个或者更多整数作为 arguments 时，它们会被编译成年份、月份、日期、小时、分钟、秒钟、以及毫秒（在本地时间中）：

```js
let d = new Date(2021, // 2021 年
                 0, // 一月
                 1, // 一号
                 2,3,4,5) // 02:03:04.005 本地时间
d // Fri Jan 01 2021 02:03:04 GMT+1100 (Australian Eastern Daylight Time)
```

需要注意的是，月份是从 0 开始计数的，即 0 代表 1 月，11 代表 12 月。如果省略了任意 arguments 时，它们会被设为 0（日期为 1）。

因为时间会根据本地时区来编译，所以在想要指定一个 UTC（Universal Coordinated Time，aka GMT）时，我们可以使用 Date.UTC() 方法来创建。这个静态方法会接收和 Date() 构造函数一样的 arguments 然后编js译为 UTC 时间表示的毫秒数：

```js
let utc = Date.UTC(2100, 0, 1);
let d = new Date(utc);
d // Fri Jan 01 2100 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
// 注意其时间被改为了本地时间
```

在对日期对象进行 console.log() 时，它默认会返回本地时间，而如果想要返回 UTC 表示的时间的话，则需要使用 toUTCString() 或者 toISOString() 方法：

```js
let d = new Date();
console.log(d); // Sat Feb 20 2021 19:17:23 GMT+1100 (Australian Eastern Daylight Time)
console.log(d.toUTCString()); // Sat, 20 Feb 2021 08:17:23 GMT
console.log(d.toISOString()); // 2021-02-20T08:17:23.328Z
```

最后需要一提的是，在构造函数中传入字符串作为 argument 时，构造函数会尝试将这个字符串解析成为其表示的日期和时间，toString(), toUTCString() 或者 toISOString() 返回的字符串就可以被构造函数解析：

```js
let d1 = new Date();
let dISOString = d1.toISOString(); // 2021-02-20T08:17:23.328Z
let d2 = new Date(dISOString);
d2.toString() === d1.toString() // true
```

在创建完 Data 对象之后，它包含了许多的 get 和 set 方法用于读写其年份、月份、日期、小时、分钟、秒钟、以及毫秒。每一种 get 和 set 都有两种形式，用于接收本地时间或者 UTC 时间：

```js
// 比如年份的读取包括了：
let d = new Date();
d.getFullYear();
d.getUTCFullYear();
d.setFullYear(2021);
d.setUTCFullYear(2021);
```

对于其他的方法只需要将 'FullYear' 改成相应的 'Month', 'Date', 'Hours', 'Minutes', 'Seconds', 'Milliseconds'。除了 date 之外我们还可以通过 Day 或者 UTCDay 来取得日期是周几（0 表示周日，6 表示周六）：

```js
let d = new Date(); // 周日
d.getDay() // 0
d.getUTCDay() // 6
```

#### 时间戳（Timestamps）

在 JavaScript 内部，日期和时间是通过一个整数储存的。这个整数代表了从 1970 年 1 月 1 日开始的毫秒数，最大可以支持到  2<sup>53</sup> 的整数，即 270000 多年。

对于这个整数，我们也可以直接对其进行 get 或者 set：

```js
d.setTime(d.getTime() + 10000); // 往后推迟十秒
```

这个毫秒数表示的值也被称为时间戳（thimestamps），很多时候直接改动它会比操作 Date 对象更为便捷，静态的 Date.now() 方法会返回表示当前时间的时间戳，它可以被用来衡量运行程序的时间：

```js
let d1 = Date.now();
let a = new Array(10000000).fill(1);
let d2 = Date.now();
console.log(`It took ${d2 - d1} ms.`); // 'It took 32 ms.' 显示创建数组花费的时间
```
在需要更高精度的时间时，可以使用 performance().now() 来代替 Date.now()，它也会返回毫秒数，不过不再是一个整数，而是拥有更高精度的小数。不过 performance 对象是 Performance API 下的一员，不过并不是 ECMAScript 标准下的成员。不过浏览器和 Node 都实现了这一 API：

```js
let d1 = performance.now();
let a = new Array(1);
let d2 = performance.now();
console.log(`It took ${d2 - d1} ms.`); // 'It took 0.005000054836273193 ms.'
```

#### 日期运算（Date Arithmetic）

Date 对象可以通过 JavaScript 标准的大于小于进行比较，并且可以对两个 Date 对象进行减法，会返回他们相差的毫秒数。对于加法来说，使用前面的 set 方法会更为简单。

set 方法同时也会处理溢出数据：

```js
let d = new Date();
d // Sun Feb 21 2021 07:15:32 GMT+1100 (Australian Eastern Daylight Time)
d.setMonth(100); // 超出 11 （十二月）的部分会被转换成年份，注意下面的年份被增加了 8 年
d // Mon May 21 2029 07:16:03 GMT+1000 (Australian Eastern Standard Time)
```

### 格式化日期（Formatting and Parsing Date Strings）

如果想对用户展示时间的话，Date 对象也包括了许多转换成字符串的方法，我们可以选择需要使用的那个：

**toStirng()：** 使用本地时区表示日期和时间，但不会以本地的时间形式表现

**toUTCString()：** 使用 UTC 时区表示日期和时间，但不会以本地的时间形式表现

**toISOString()：** 使用 ISO-8601 标准表示日期和时间，以 'year-month-dayThours:minutes:seconds.msZ' 的形式来表现，字母 T 分割了日期和时间，最后的字母 Z 表示时间表示了 UTC 时间。

**toLocaleString()：** 使用本地时区表示日期和时间，并且以本地的时间形式表现

**toDateString()：** 使用本地时区表示日期部分，但不会以本地的时间形式表现

**toLocaleDateString()：** 使用本地时区表示日期部分，并且以本地的时间形式表现

**toTimeString()：** 使用本地时区表示时间部分，但不会以本地的时间形式表现

**toLocaleTimeString()：** 使用本地时区表示时间部分，并且以本地的时间形式表现

```js
let d = new Date();
d.toString(); // "Sun Feb 21 2021 07:29:08 GMT+1100 (Australian Eastern Daylight Time)"
d.toUTCString(); // "Sat, 20 Feb 2021 20:29:19 GMT"
d.toISOString(); // "2021-02-20T20:29:27.657Z"
d.toLocaleString(); // "2/21/2021, 7:29:38 AM"
d.toDateString(); // "Sun Feb 21 2021"
d.toLocaleDateString(); // "2/21/2021"
d.toTimeString(); // "07:30:04 GMT+1100 (Australian Eastern Daylight Time)"
d.toLocaleTimeString(); // "7:30:12 AM"
```

在想要对用户展示日期，其实上面的方法时都不是最理想的，在接下来的 Intl 部分会有对更好的方法的介绍。

最后，还要介绍一下 Date.parse() 的静态方法，它会接收一个字符串作为 argument，然后将其解析为时间戳。它可以解析 toString(), toUTCString(), toISOString(), toLocaleString() 的返回值，若无法解析时返回 NaN。

### Error 类（Error Classes）

JavaScript 中的 throw 和 catch 关键字可以用于抛出或者任何 JavaScript 的值，包括基础值。所以对于 error，它并不会有一个指定的值。JavaScript 定义了 Error 类，但在抛出错误时，会更偏相于使用 Error 的实例或者其子类。

在创建 Error 时，他会保存目前栈的状态，所以在异常发生时，会同时抛出 stack trace（stack trace 位于 Error 对象创建的位置，不是 throw 的位置）。

Error 对象拥有两个属性，message 和 name，以及一个 toString() 方法。message 的值是传入 Error() 构造函数的值转换成的字符串。通过 Error() 构造函数创建的 error 对象的 name 属性会一直是 'Error'。toString() 方法会以 name: message 的形式返回。

主流浏览器和 Node 其实都还在 Error 对象上定义了已给 stack 属性，即使这不属于 ECMAScript 规范。stack 的值是一个多行的字符串，包含了 JavaScript call stack 的 stack trace。

除去 Error 类之外，JavaScript 还定义了许多其子类，用于表示错误类型。其子类为: EvalError, RangeError, ReferenceError, SyntaxError, TypeError 以及 URIError。如同其超类 Error 类，它们也拥有一个构造函数，每个实例也拥有 name 属性指向了其构造函数的名字。

我们可以随时定义新的 Error 的子类，并且修改其 name 的 message 属性，我们也可以传入新的属性去表示更多的细节。

### JSON 的序列化和解析（JSON Serialization and Parsing）

在传输或接收别的程序的数据时，就可以使用 JSON，（Javascript Object Notation）。它使用了 JavaScript 对象和数组的形式来表示数据。它支持了基础的字符串，数字，布尔值和 null 以及数组和对象，但不支持如 Map，Set，Date，Typed Arrays 之类的对象。

JavaScript 支持 JSON.stringify() 和 JSON.parse() 方法用于序列化和解析 JSON。对于不包含不可序列化对象的数组和对象，我们可以直接将其传入 JSON.stringify() 作为 argument 来取序列化它们；对于 JSON 我们可以使用 JSON.parse() 来解析它表达的对象和数组：

```js
let o1 = {s: "", n: 0, a: [true, false, null]}; // 可序列化对象
let s = JSON.stringify(o); // 序列化
s // "{"s":"","n":0,"a":[true,false,null]}"
let o2 = JSON.parse(s); // 解析 JSON， o2 中的属性名和值和 o1 相同
```

我们可以使用这两个方法来：

- 序列化对象进入一个文件，然后使其他程序使用
- 同时调用两个方法来深复制一个可序列化对象

虽然大多时候我们只会传入一个 argument，但这两个方法其实可以接收第二个可选的 argument，并且 JSON.stringify() 还可以接收第三个 可选的 argument。

我们先说 JSON.stringify() 的第三个 argument，他表示了 JSON 格式下的缩进，使 JSON 的可读性更高。若传入的是一个数字，这会表示其缩进的空格数，如果是一个字符串，则会用该字符串进行缩进：

```js
let o1 = {s: "", n: 0, a: [true, false, null]}; // 可序列化对象
let s1 = JSON.stringify(o,null,2); // 使用两个空格进行缩进
s1 /*
"{
  "s": "",
  "n": 0,
  "a": [
    "2021-02-20T21:34:22.825Z",
    {},
    null
  ]
}" */

let s2 = JSON.stringify(o,null,'\t'); // 使用 tab 进行缩进
s2 /*
"{
	"s": "",
	"n": 0,
	"a": [
		"2021-02-20T21:36:08.858Z",
		{},
		null
	]
}" */
```

因为 JSON.parse() 会忽略空白符，所以在序列化时传入空白符不会有解析上的任何影响。

#### 自定义 JSON

如果 JSON.stringify() 接受的 argument 不支持 JSON 的格式（如 Set，Date），它会去寻找该值是否拥有 toJSON() 的方法，如果有，这个方法会被调用，然后序列化这个方法的返回值。比如 Date 对象就拥有 toJSON() 方法，返回 toISOStrring() 的返回值。但是在解析时，他并不会在被解析成 Date。

但如果想要重新解析出 Date，或者对解析出的对象进行别的改变的话，我们就可以在 JSON.parse() 的第二个 argument 传入一个 reviver 函数。这个函数会在每一次出现基础值时被调用（但是不对包括基础值的数组或者对象调用）。这个函数拥有两个 arguments，第一个 argument 是属性名，可以是对象的属性名，或者数组的索引转换成的字符串；第二个 argument 是对象属性或者数组索引所对应的基础值。这个函数会以该对象或者数组的方法的形式被调用，所以 this 会指向包含其的对象。

这个函数的返回值会成为该属性名下新的值。如果返回了 undefined，这一属性或元素会被删除。

比如我们可以使用下面的例子来重新创建 Date 对象：

```js
let d = new Date(); 
let o1 = { // 创建一个包含 Date 的对象
    x: d,
    y : [1,2,d] // 包含 Date 的数组
}
let s = JSON.stringify(o1,null,2); // 序列化成 JSON
let o2 = JSON.parse(s, function(key, value) { // 解析时传入第二个 reviver 函数
  if (typeof value === 'string' && /^\d\d\d\d-\d\d-\d\dT\d\d:\d\d:\d\d.\d\d\dZ$/.test(value)) { 
    return new Date(value); // 在符合 Date 时创建新 Date 对象并返回
  }
  return value; // 不符合时直接返回
});
o1.x.toISOString() === o2.x.toISOString() // true，成功创建新 Date 对象，可以调用其中的方法
o1.y[2].toUTCString() === o2.y[2].toUTCString() // true，数组中的 Date 也被成功创建
```

除了使用在前面提到的 toJSON() 方法之外，JSON.stringify() 的第二个 argument 也使我们可以自定义序列化后的结果。传入的 argument 可以是一个数组或者是一个函数。

如果由字符串组成的数组被传入成为第二个 argument 的话，它们会成为对象的属性名（或数组元素）。任何不在该数组中的属性名则会在序列化时被忽略，序列化后的顺序也是根据数组顺序而定的：

```js
let address = {
    state: 'B',
    country: 'A',
    road: 'D',
    number: 'E',
    city: 'C'
}
let s = JSON.stringify(address,
["city","state","country"]);
s // "{"city":"A","state":"B","country":"C"}" // 只包含了数组中的属性，忽略剩余的属性，并且排序
```

如果传入了一个函数作为第二个 argument，它会被成为 replacer 函数，即 JSON.parse() 中的 reviver 函数的对立。这个函数会被每个进行序列化的值调用，其中第一个 argument 是属性名或数组索引，第二个 argument 是属性值或元素值，同样可以使用 this 进行指向，它的返回值会被成为序列化后的值。如果返回了 undefined，则不传入任何值：

```js
o = {
  x: /abc/g,
  y: 'abc'
};
let s = JSON.stringify(o, (key, value) => value instanceof RegExp ? undefined : value);
s // "{"y":"abc"}" // 上面的 replacer 将正则改为了 undefined，即不返回正则
```

通常情况下，在我们定义了 toJSON() 方法或者定义了 replacer 函数时，都会创建一个对应的 JSON.parse() 中的 reviver 函数。这使我们可以序列化和解析原本不可以被序列化和解析的对象。但如果这么做，我们就要知道我们在 JSON 的基础上定义了一个新的数据形式，这会牺牲 JSON 原本对许多工具和语言的便携性和兼容性。

### The Internationalization API

JavaScript 的国际化（Internationalization） API 由三个类组成，分别是 Intl.NumberFormat, Intl.DateTimeFormat 以及 Intl.Collator。这使得我们可以格式化日期，时间，以及对比字符串。虽然这些类并不存在于 ECMAScript 标准中，但是在 ECMA402 标准中被定义了。

#### 格式化数字（Formatting Numbers）

不同地区的用户会根据其地区拥有不同的数字格式：小数点可能是逗号或者句号，千分符可能是句号或者逗号，不使用阿拉伯数字等等。

Intl.NumberFormat 类定义了一个 format() 方法，它会以定义的格式来格式化其 argument。Intl.NumberFormat 类的构造函数接收两个 arguments，第一个 argument 指定了数字以什么地区的形式进行格式化，第二个 argument 是一个指定了具体细节的对象。

如果第一个 argument 被忽略了或者是 undefined，就会使用系统的地区。如果是一个字符串，他会表示一个期望地区，如 'en-US', 'fr' 或者 'zh-Hans-CN' 等等。

第二个 argument 是一个可以拥有以下属性的对象：

- **style：** 指定了数字种类的格式，默认为 'decimal'，指定为 'percent' 可以将数字展示位百分比，或者 'currency' 将其展示成货币数量
- **currency：** 如果 style 属性的值是 'currency'，这个属性会用于指定货币码（三位字母的 ISO currency code，如 USD，CNY）
- **currencyDisplay：** 如果 style 的值是 'currency'，这个属性指定了该货币如何被显示，默认值为 'symbol'，会使用货币符号；或改为 'code' 来展示 三位字母的 ISO code；或改为 'name' 来拼写出货币名
- **useGroping：** 如果想要有千分位分隔符，设置为true，反之亦然
- **minimumIntegerDigits：** 指定整数部分至少要有几位 digits，若小于这个数，则会在左边 pad with 0，默认值为 1，最大值为 21
- **minimumFractionDigits：** 指定了小数部分至少要有几位 digits，若小于这个数，则会在右边 pad with 0，默认值为 0，最大值为 20
- **maximumFractionDigits：** 指定了小数部分至多能有几位 digits，若大于这个数，则会四舍五入，默认值为 3，可取值为 0 - 20

在创建完一个 Intl.NumberFormat 对象之后，可以使用其 format() 方法来格式化传入的数字：

```js
let formatToCNY = Intl.NumberFormat("zh-CN", {
style: "currency",
currency: "CNY", 
currencyDisplay:'code',
maximumFractionDigits: 1,
minimumIntegerDigits: 3
}).format;
formatToCNY(10.23); //"CNY 010.2"
```

#### 格式化日期和时间（Formatting Dates and Times）

Intl.DateTimeFormat 类使我们可以格式化日期和时间，它的构造函数和前面的 Intl.NumbeFormat 一样接收两个 arguments，用于指定格式化的内容。第一个 argument 和前面一样。

相较于 Date 对象，Intl.DateTimeFormat 提供了更为全面的格式化方法，它的第二个 argument 是一个可以拥有以下属性的对象：

- **year：** 可选 'numeric' 或 '2-digit' 来指定 4 位数或 2 位数
- **month：** 可选 'numeric' 来表示可选的一位数表示如一月为 '1' 或 '2-digits' 即一月为 '01' 或使用 'long' 来表示名字如 'January' 或 'short' 表示 'Jan' 或 'narrow' 表示 'J'
- **day：** 可选 'numeric' 来指定 1~2 位数或 '2-digit' 表示 2 位数
- **weekday：** 可选 'long' 来表示名字如 'Monday' 或 'short' 表示 'Mon' 或 'narrow' 表示 'M'
- **era：** 是否是 era 格式，可选 'long' 或 'short' 或 'narrow'
- **hour：** 可选 'numeric' 来指定 1~2 位数或 '2-digit' 表示 2 位数
- **minute：** 可选 'numeric' 来指定 1~2 位数或 '2-digit' 表示 2 位数
- **second：** 可选 'numeric' 来指定 1~2 位数或 '2-digit' 表示 2 位数
- **timeZone：** 指定时区，若忽略则为本地时区，可选 UTC' 或 IANA 时区名
- **timeZone：** 指定时区名表达方式，可选 'long' 或 'short'
- **hour12：** 可选 true 用 12 小时制，false 用 24 小时制
- **hourCycle：** 用于指定午夜的时间，'h11' 代表了午夜是 0 点，'h24' 代表午夜是 24

在创建完一个 Intl.DateTimeFormat 对象之后，可以使用其 format() 方法来格式化传入的 Date 对象。

#### 比较字符串（Comparing Strings）

在面对需要排序进行字母排序时，这对于非英文的语言会变得比较复杂。所以单单使用 sort() 在某些时候是不够的。这时候就需要 Intl.Collator() 对象来进行比较了。Intl.Collator() 和前面两者一样，会接收两个 arguments。第一个 argument 和前面一样，它的第二个 argument 是一个可以拥有以下属性的对象：

- **usage：** 可选 'sort' 或 'search' sort 相较于 search 更为严格（search 会忽略某些 accents）
- **sensitivity：** 可选 'base' 忽略大小写和 accents 或 'case' 比较大小写，忽略 accents 或 'accent'比较 accents。忽略大小写或 'variant' 比较大小写和 accents
- **ignorePunctuation：** 可选 true 忽略空格和标点，false 则不忽略
- **numeric：** 可选 true 会进行数字大小检查，false 则不进行
- **caseFirst：** 可选 'upper' 来使大写在前，或 'lower' 来使小写在前

在创建完一个 Intl.Collator 对象之后，可以使用其 compare() 方法来进行两个字符串之间的比较，或者在 array 的 sort() 方法中传入该 Intl.Collator().compare() 方法作为 argument 来比较该数组。


### The Console API

我们已经见过了许多的 console.log() 函数，并且对其不陌生了，他会浏览器的 devtool 中的 Console 页（或 Node 中的 终端窗口中）打印出字符串。

### console 的方法

但其实 console API 还定义了许多别的函数，这个 API 也并不是 ECMAScript 标准中的一员，但被浏览器和 Node 支持。它一共包含了下面的函数：

**console.log()**

这是最为大众熟知的函数，他将其 arguments 转换成函数饭后在 console 打印出来。它会在每一个 arguments 加入一个空格，在打印完成后开始新的一行。

**console.debug(), console.info(), console.warn(), console.error()**

这些函数几乎等同于 console.log()，不过会在 cosole 的打印前增加一个图标来表示其等级或者严重程度。

**console.assert()**

如果它接收的第一个 argument 是一个真值，即断言通过，这个函数就不会做任何事。但如果第一个 argument 是一个假值，则剩余的 arguments 会以 console.error() 的形式打印出来，并且拥有 'Assertion failed:' 前缀，但并不会抛出异常。

**console.clear()**

这个函数会 clear the console。

**console.table()**

这个函数是一个十分强大，但是不太被人知晓的特性。它可以用于创建一个表格输出。他会尽可能以表格的形式来展示其 arguments，若不行的话，则使用 console.log() 的形式。这个函数在接收以对象构建的数组，且对象属性数量相同时额外的好用。在这种情况下，每一行都是一个对象，每一列都会是一个属性。我们也可以只传入一个对象最为 argument，那么显示的表格会是一列属性名对一列属性值。

**console.trace()**

这个函数会和 console.log() 一样打印出其 arguments，并且加上它的 stac trace。

**console.count()**

这个函数接受一个字符串作为 argument，然后打印该字符串以及以该字符串调用这个函数的次数。

**console.countReset()**

这个函数会接收一个字符串，然后重置那个字符串对应的 count 数。

**console.group()**

这个函数会和 console.log() 一样打印其 arguments，并且在内部状态中记录接下来的所用直到 console.groupEnd() 之前的 console 消息。这些被记录的值会在打印时被缩进，并且可以展开或合起该 group。它的 arguments 通常是用来表示对该 group 特征解释的名字。

**console.groupCollapsed()**

这个函数等同于上面的 console.group()，除了他在显示的时候会默认为合起状态。

**console.groupEnd()**

这个函数不接收 argument，然后会结束其最近的 group 的缩进。

**console.time()**

这个函数接收一个字符串作为 argument，并记录该字符串对应的时间节点，不会打印任何值。

**console.timeLog()**

这个函数接收其第一个字符串作为 argument，如果这个字符串在之前被 console.time() 记录过的话，它便会以 ‘字符串: 经过的时间’的形式打印出值，若有更多的 arguments，则剩余的 arguments 会以 console.log() 的形式打印出来。

**console.timeEnd()**

这个函数会接受一个字符串，如果这个字符串在之前被 console.time() 记录过的话，它便会返回那个 argument 和其经过的时间，然后删除该计时器。

#### 

使用 console.log() 打印出的 arguments（包括以 log 的形式打印的）还有额外的特性：如果第一个 argument 是一个包含了 %s, %i, %d, %f, %o, %O, 或 %c 的字符串，则第一个 argument 会被当成一个格式化字符串（format string）。然后剩余的每个 arguments 会插入到第一个 argument 中每个 %n 的位置（n 为前面出现过的的字母）。若 %n 的数量更多，则尾部的 %n 会直接显示；若 arguments 的数量更多，剩余的 arguments 会以 console.log() 的形式显示：

格式化字符串的意义如下：

- **%s** --- argument 会转换成字符串
- **%i 和 %d** --- argument 会向下取整转换成整数
- **%f** --- argument 会转换成数字
- **%o 和 %O** --- argument 会被作为对象（在浏览器的 console，展示的值是如同对象那样可交互的）。大写的 %O 会根据实现来指定其输出的格式
- **%c** --- 在浏览器中，argument 会被编译成代表 CSS 的字符串，使其接下来的文字以该 css 进行风格化。在 Node 中 %c 和其对应的 argument 会被忽略

### URL APIs

URL 类可以用于解析 URLs，并且是我们可以对其进行修改。它也不属于 ECMAScript 的标准之中，不过被 Node 和浏览器支持。

我们可以通过其构造函数 URL() 创建一个 URL 对象，传入代表 URL 的字符串作为 argument。URL 对象拥有很多属性：

```js
let url = new URL("https://example.com:8000/path/name?q=term#fragment");
url.href // "https://example.com:8000/path/name?q=term#fragment"
url.origin // "https://example.com:8000"
url.protocol // "https:"
url.host // "example.com:8000"
url.hostname // "example.com"
url.port // "8000"
url.pathname // "/path/name"
url.search // "?q=term"
url.hash // "#fragment"
```

### 计时器

setTimeout() 和 setInterval() 方法是两个用于计时的方法，它们也不属于 ECMAScript 的标准之中，不过被 Node 和浏览器支持。

#### setTimeout()

setTimeout() 接收的抵押给 argument 是一个无参数的函数，第二个 argument 是在调用函数之前需要等待的毫秒数：

```js
setTimeout(() => { console.log("a"); }, 1000);
setTimeout(() => { console.log("b"); }, 2000);
setTimeout(() => { console.log("c"); }, 3000);
// 依次在一二三秒后输出 a，b，c
```

每一个 setTimeout() 并不会等待其之前的运行结束再开始，而是从上到下同时运行。如果我们忽略了 setTimeout() 的第二个 argument，它会默认为 0。这不意味着他会直接执行，而该函数会被注册成为尽早执行。

setTimeout() 会接收函数作为第一个 argument，这个函数中也可以包含另一个 setTimeout()。

#### setInterval()

setInterval() 和 setTimeout() 接收两个一样的 arguments。不过每经过指定的时间（大致程度上）便会重复执行一次：

```js
setInterval(() => { console.log("a"); }, 1000);
// 每隔一秒输出一次 a
```

#### clearTimeout() 和 clearInterval()

setTimeout() 和 setInterval() 其实都会返回一个值。如果我们把这个值保存在变量中，我们就可以在稍后通过 clearTimeout() 或者 clearInterval() 来取消计时器的执行。在浏览器中的返回值是一个数字，在 Node 中则是一个对象，不过这并不重要，我们应该把这个返回值当成不透明值来处理，因为这个值除了传入到上述的取消函数中之外，没有别的用处。

```js
let counter = setInterval(() => { // 定义一个计时器
  console.clear();
  console.log(new Date().toLocaleTimeString()); 
}, 1000); // 每个一秒刷新成为当前时间
setTimeout(() => {
  clearInterval(counter);
}, 10000); // 在 10 秒后停止该计时器
```

### 小结

本章的要点：

- 介绍了 JavaScript 中的数据结构，如 Set，Map，Typed Arrays
- 使用 Date 类来获取或改动时间
- 使用 URL 类来获取或改动 URL 信息
- JavaScript 的正则表达式以 RegExp 对象的形式存在
- JavaScript 国际化 API 可用于格式化数字，时间，以及排列字符顺序
- JSON 对象可以用于序列化和解析 JavaScript 的数据结构
- console 对象可用于打印信息

## 迭代器和生成器（Iterators and generators）

可迭代对象（Iterable objects）和其迭代器（iterators）是在 ES6 中新增的特性。例如数组，类型化数组，字符串，Set 和 Map 都是可迭代对象。这意味着我们可以使用 for of 循环来对它们进行迭代：

```js
let sum1 = 0;
for (let i of [1,2,3]) { // 对数组元素的值进行迭代
  sum1 += i;
}
sum1 // 6

let sum2 = 0;
for (let s of '123') { // 对字符串的字符进行迭代
  sum2 += +s;
}
sum2 // 6 
```

迭代器也使得我们可以对可迭代对象进行展开操作和解构赋值：

```js
let chars = [..."abcd"]; // chars == ["a", "b", "c", "d"]
let [w, x, y, z] = chars; // z = "d"
```

Map 会以 [key, value] 形式进行迭代，或者可以通过 keys() 和 values() 方法进行 key 或者 value 的迭代；Set() 构造函数可以接收一个可迭代对象作为 argument：

```js
let m = new Map([["one", 1], ["two", 2]]);
[...m] // [["one", 1], ["two", 2]]
[...m.entries()] // [["one", 1], ["two", 2]]
[...m.keys()] // ["one", "two"]
[...m.values()] // [1, 2]

new Set("abc"); // 等同于 new Set(["a", "b", "c"])
```

### 迭代器是怎么工作的（How Iterators Work）

套了解迭代器的工作原理，我们要从下面三个方面展开：

- 可迭代对象(iterable object）
- 迭代器（itertor）对象本身，他会被用于进行迭代
- 迭代结果对象（iteration result object），它会用于存放每一次迭代的结果

1. 可迭代对象就是任何拥有迭代方法（Symbol Symbol.iterator 为方法名）的对象，该方法可返回迭代器对象。

2. 迭代器对象是任何拥有 next() 方法的对象，该方法会返回一个迭代结果对象。

3. 迭代结果对象是一个拥有属性 value 和 done 的对象。

若想要迭代一个可迭代对象，我们需要先调用其迭代方法去获得一个迭代器对象，然后调用该迭代器对象的 next() 方法来迭代对象，直到返回值的 done 属性变成了 true。所以我们可以如下实现 for of 循环：

```js
let a = [1,2,3,4,5]; // 创建一个数组（可迭代对象）
let iteratorOfa = a[Symbol.iterator](); // 储存数组的迭代方法
// 将 result 赋值为 next() 的返回值，即迭代结果对象。在迭代结果对象的 done 属性不为 false 时，进行迭代
for (let result = iteratorOfa.next(); !result.done; result = iteratorOfa.next()) {
  console.log(result.value); // 打印迭代结果对象的 value 属性，即数组元素值
}
```

### 实现一个可迭代对象（Implementing Iterable Objects）

想要让一个对象可以被迭代的话，我们需要为其实现一个迭代方法。该方法名是一个叫做 Symbol.iterator 的 Symbol；这个方法又必须返回一个可拥有 next() 方法的迭代对象；next() 方法又必须返回一个拥有 value 和 done 属性的迭代结果对象：

```js
class Range { // 创建一个新的类
  constructor(lower, upper) { // 定义其构造函数
    this.lower = lower;
    this.upper = upper;
  }
  [Symbol.iterator]() { // 定义其迭代方法
    let next = this.lower; // 下一个返回值
    let last = this.upper; // 最后的返回值
    return { // 返回可迭代对象
      next() { // 可迭代对象的 next() 方法，其返回值是迭代结果对象
        if ( next <= last) { // 在迭代未结束时返回拥有 value 属性的迭代结果对象
          return { value : next++};
        } else { // 否则返回拥有 done 为 true 的迭代结果对象
          return { done : true};
        }
      },
      [Symbol.iterator]() { return this; } // 可以将迭代器本身设置为可迭代对象
    }
  }
}
[...new Range(-3,5)]; // [-3, -2, -1, 0, 1, 2, 3, 4, 5]
```

除去定义类时让其可被迭代，我们也可以让函数返回可迭代的值，就比如我们可以用这种方法来实现数组的 map() 或者 filter() 方法：

```js
function map(iterable, f) { // 实现 map 方法，传入可迭代对象和其 map 时调用的函数
  let iterator = iterable[Symbol.iterator](); // 取得可迭代对象的迭代器
  return { //返回一个对象，它既是可迭代对象，也是迭代器
    [Symbol.iterator]() {
      return this;
    },
    next() {
      let v = iterator.next();
      if ( v.done ) {
        return v;
      } else {
        return { value: f(v.value) };
      }
    }
  };
}
[...map([1,2,3,4],a => 2*a )] // [2, 4, 6, 8]
```

#### 终止迭代器（“Closing” an Iterator: The Return Method）

在某些时候，迭代器一路迭代到底，而是在中间就停止，比如 for of 循环中的 break 或者 return 或者一个异常；或者解构赋值时不需要所有可迭代对象中的元素。

在迭代器对象上我们也可以实现一个 return() 方法，如果迭代器在 next() 还没有返回到 done 为 true 的迭代结果对象时就停止了的话，编译器便会开始查找是否有 return() 方法，若有，则会不传参数然后调用它。

### 生成器（Generators）

虽然迭代器使我们可以使用 for of 循环和展开操作符之类便捷的新特性。但是对于实现新的可迭代对象、其迭代器、以及其迭代结果对象则会十分复杂。这就要介绍到生成器了（generators），它使我们可以更便捷的实现新的可迭代对象。

生成器其实也是一种迭代器，不过拥有了 ES6 新增的句法，使其变得更为灵活和便捷。

如果想要定义一个生成器，我们需要先定义一个生成器函数。生成器函数的句法和普通函数无异，只不过是通过 function* 而非 function 关键字而定义。当我们调用生成器函数时，他并不会执行函数体，而是返回一个生成器对象，而这个生成器对象也是一个迭代器。在调用该迭代器的 next() 方法时，会继续执行生成器函数体中的语句，直到遇到下一个 yield 关键字。yield 是一个 ES6 新增的关键字，它类似于 return，所以 yield 之后的值便会成为调用 next() 的返回值：

```js
function* genF() { // 定义一个生成器函数，但调用该函数不会直接运行其函数体，而是创建新的生成器对象
  yield 1; // 生成器对象的 next() 方法会执行函数体，直到遇见 yield 关键字然后停下
  yield 2; // 然后该 yield 之后的值便会作为返回值返回
  yield 3;
  yield 4;
  yield 5;
}
let gen = genF(); // 在调用该函数时，获得一个生成器
gen.next().value; // 1，生成器也是一个可迭代对象，并且可以迭代 yield 的值
gen.next().value; // 2
gen.next().done; // false
gen.next().value; // 4
gen.next().value; // 5
gen.next().done; // true

gen[Symbol.iterator]() // 该生成器也有迭代方法，所以也可以使用 for of 或解构赋值等
```

在对象或者类中，我们也可以使用函数简写来实现生成器：

```js
let o = {
  x: 1, y: 2, z: 3,
  *g() {
    for(let key of Object.keys(this)) {
      yield key;
    }
  }
};
[...o.g()] // ["x", "y", "z", "g"]
```

不过需要注意的是，箭头函数不能用来创建生成器函数。

使用生成器函数可以使我们在定义一个可迭代类时变得更为简单，我们不需要一层一层编写迭代方法、迭代器、迭代结果对象等等，我们只需要编写 \*\[Symbol.iterator]() 方法即可：

```js
*[Symbol.iterator]() { // 使用生成器函数编写迭代方法
  for (let x = this.lower; x < this.upper; x++) {
    yield x;
  }
}
// 等同于前面的例子：
[Symbol.iterator]() { // 定义其迭代方法
  let next = this.lower; // 下一个返回值
  let last = this.upper; // 最后的返回值
  return { // 返回可迭代对象
    next() { // 可迭代对象的 next() 方法，其返回值是迭代结果对象
      if ( next <= last) { // 在迭代未结束时返回拥有 value 属性的迭代结果对象
        return { value : next++};
      } else { // 否则返回拥有 done 为 true 的迭代结果对象
        return { done : true};
      }
    },
    [Symbol.iterator]() { return this; } // 可以将迭代器本身设置为可迭代对象
  }
}
```

可见使用生成器会变得便捷得多。

**yield***

使用 yield* 使我们可以更为方便的迭代多个可迭代对象的元素：

```js
function* genF(...iterables) { // 不使用 yield*
  for(let iterable of iterables) {
    for(let element of iterable) {
      yield element;
    }
  }
}
[...genF('abcd',[1,2,3])]; // ["a", "b", "c", "d", 1, 2, 3]

function* genF(...iterables) { // 不使用 yield*
  for(let iterable of iterables) {
    yield* iterable;
  }
}
[...genF('abcd',[1,2,3])]; // ["a", "b", "c", "d", 1, 2, 3]

// 两个方法效果一样
```

### 进阶生成器特性（Advanced Generator Features）

虽然生成器最常被用于创建迭代器，但其实它还有一个很重要的特性便是可以暂停和重新开始运行，并在其中产出中间值。

#### 生成器函数的返回值

在生成器函数中，我们并没有提到它的返回值，即其 return 关键字。return 可以在生成器函数中被使用，但是它并不会直接返回值，而是终止生成器的执行。这个返回值虽然不会直接返回，但是可以在等下被用到：

回想一下，迭代器的 next() 方法的返回值是一个拥有 value 和 done 属性的迭代结果对象，对于正常的迭代器和生成器来说，如果 value 被定义，则 done 因为 false 或 undefined；若 done 为 true，则 value 因为 undefined。

但如果生成器返回了一个值，则最后一次调用 next() 返回的迭代结果对象的 value 和 done 都会被定义：value 会包含该返回值，而 done 为 true。他虽然会被 for of 循环或者解构赋值等忽略，但是我们可以显示的去调用其 next() 方法来取得该值：

```js
function* genF() { 
  yield 1; 
  yield 2;
  return 3;
  yield 4;
}
[...genF()] // [1, 2]，return 3 终止迭代，并且返回值也会被忽略，之后的 4 也会被忽略

let gen = genF();
gen.next(); // {value: 1, done: false}
gen.next(); // {value: 2, done: false}
gen.next(); // {value: 3, done: true} // 可以显示的调用 next() 取得返回值
gen.next(); // {value: undefined, done: true}，迭代终止，无法取得 4
```

#### yield 表达式的值

在前面，我们将 yield 用作为一个产出值的语句，但其实它是一个表达式，也可以拥有自己的值。

在生成器的 next() 方法被调用时，它会一路运行到 yield 表达式，其之后紧跟的表达其会被运算然后作为 next() 的返回值。然后生成器函数会停止执行，直到下一次 next() 的调用。然后该 next() 的 argument 就会成为 yield 的值。

所以对应关系是：

- 生成器会将 yield 之后的值返回给其调用者
- 调用者又可以在 next() 方法中传入 argument 作为 yield 的值

```js
function* genF() {
  let y1 = yield 1;
  let y2 = yield 2;
  let y3 = yield 3;
  console.log(y1+y2+y3);
  return 4;
}

let g = genF();
g.next(); // 第一次调用 next 时，结果对象的 value 为 1
g.next(4); // 第二次调用 next 时，结果对象的 value 为 2，同时为 y2 进行了 y1 = 4 的赋值
g.next(5); // 第三次调用 next 时，结果对象的 value 为 3，同时为 y2 进行了 y2 = 5 的赋值
g.next(6); // 第四次调用 next 时，结果对象的 value 为 4，同时为 y2 进行了 y3 = 6 的赋值，并且返回 15
```

#### return() 和 throw() 方法

在生成器上，它除了拥有 next() 方法，也拥有了 return() 和 throw() 方法。如它们的名字一样，调用它们会返回值或者抛出异常，就如同在生成器中加入了 return 或者 throw 关键字一样：

```js
function* genF() { 
  yield 1; 
  yield 2;
  yield 3;
}
let g = genF();
g.return(); // 提前结束迭代，返回 {value: undefined, done: true}
g.next() // 迭代已经结束，返回 {value: undefined, done: true}
let g2 = genF();
g2.throw(); // 提前结束迭代，抛出 Uncaught undefined
```

throw() 方法会在生成器内部抛出一个异常，但如果生成器函数内部拥有异常捕获机制，则异常就不一定会是致命的，这意味着生成器可以接着执行:

```js
function* count() { // 计数生成器函数，在遇到异常时重置计数器
  let i = 1;
  while(true) {
    try {
      yield i++;
    }
    catch {
      i = 0;
    }
  }
}
let counter = count();
counter.next();
counter.next(); // {value: 2, done: false}
counter.throw(); // {value: 0, done: false}，重置计时器，不会抛出异常，因为被生成器处理了
counter.next(); // {value: 1, done: false}
```

### 小结

这一章的要点：

- for of 循环和 ... 展开操作符可以对可迭代对象使用
- 具有 Symbol 方法 \[Symbol.iterator] 的对象，且该方法会返回一个迭代器，为可迭代对象
- 迭代器对象拥有 next() 方法用于返回迭代结果对象
- 迭代结果对象有一个 value 属性保存迭代值，和一个 done 属性来确定迭代是否完成
- 生成器函数（function\*）可以使我们更便捷的定义迭代器
- 在调用生成器函数时，函数体不会直接执行，而是返回一个可迭代的迭代器对象
- 每次调用生成器的生成的迭代器的 next() 时，会执行函数体直到遇见 yield 并将其返回

## 异步 JavaScript（Asynchronous JavaScript）

虽然说大部分 JavaScript 的代码都是同步执行的，但其实也会有许多需要异步执行的地方。这一章会介绍 JavaScript 对于异步编程的简化的一些特性。如在 ES6 中新增的 Promises，ES2017 中新增的 Async 和 await 关键字，以及 ES2018 中新增得 for await 循环等。

### 使用回调函数进行异步编程（Asynchronous Programming with Callbacks）

从最基本的角度考虑，JavaScript 的异步编程是通过回调函数（callbaks）完成的。一个回调函数是一个被传入另一个函数的函数，那另一个函数会在某个异步事件发生时调用我们传入的回调函数。

#### 计时器（Timers）

最简单的异步就是定义一个我们希望在一段时间以后再运行的代码，我们可以使用 setTimeout() 或者 setInterval() 函数来实现：

```js
setTimeout(f1, 1000);
setInterval(f1, 1000);
```

就比如上面的例子，第一个 argument 是一个函数，而第二个则是以毫秒计时的时间。所以 f1 会在一秒以后被调用（或重复调用）。

#### 事件（Events）

客户端 JavaScript 的程序基本上都是通过事件驱使（event driven）的：

相较于运行提前指定的运算，JavaScript 更多情况下会等到用户进行了操作以后再运行。浏览器会在用户按下键盘，移动鼠标或点击时生成一个事件，然后每当事件发生时，我们就可以调用其对应的回调函数。这些回调函数被称为 event handlers/listeners，可以通过 addEventListener() 来注册：

```js
let button1 = document.querySelector('#button1'); // 返回一个代表了网页中的一个元素的对象
button1.addEventListener('click', f1); // 为其添加 click 事件下的回调函数
```

上面的例子红，回调函数 f1 会在每次 button1 被点击时调用。

#### 网络事件（Network Events）

另一个异步编程经常出现的时机就是网络请求了，浏览器中的 JavaScript 可以这样来异步 fetch 数据：

```js
function getName(nameCallback) { // 传入回调函数
  // 定义一个异步的 http request
  let request = new XMLHttpRequest();
  request.open("GET", "http://www.example.com/api/name");
  request.send();
  
  注册一个回调函数，在接收到 response 时调用
  request.onload = function() {
    if (request.status === 200 ) {
      let name = request.responseText; // 取得 response 数据
      nameCallback(name); // 调用回调函数
    }
  }
}
```

客户端可以使用 XMLHttpRequest 类和回调函数来发送 HTTP requests，然后异步的处理 response（当其抵达后）。

虽然上面的例子没有使用 addEventListener() 来注册回调函数，但其实我们也可以这么做。只不过这次我们直接定义了其 onload 属性的回调函数，即等同于 request.addEventListener('load'，f1)。

### Promises

在介绍完最基础的异步编程之后，我们要介绍一下 Promises 了。 Promise 是一个表示了异步运算的结果的对象。这个结果可能是准备好的，也可能不是。特意模糊该结果的原因是不希望 JavaScript 通过同步的方法来取得 Promise 的值。我们只能定义一个在值准备好之后调用的回调函数。

虽然说 Promise 只不过是另一种使用回调函数的方法，但它其实还有一些优势：对于嵌套回调函数时，它会不停的缩进，使代码难以阅读，而使用 Promise 可以被链式调用，这使得代码更具有可读性；Promise 也标准化了异步编程的异常处理机制，也使得 error 可以正确在链式调用的 Promise 中传播。

注意 Promise 表示了单一异步运算的结果，它们不能表示重复的异步运算的结果。对于前面的 setTimeout() 和网络事件部分，我们都可以用 Promise 来替代，但是对于按钮的点击事件和 setInterval()，我们则不能用 Promise 来替代，因外它们会被多次调用。

虽然 Primises 看上去不难，对于它的基本使用也不难，但它其实很容易变得复杂。

#### 使用 Promises

在 Promises 被加入 JavaScript 后，浏览器就开始使用 Promise-based APIs 了。比如：

```js
getData(url).then(data => {
  // 这是回调函数
});
```
getData() 会发送一个异步的 HTTP request，在 request 在待定时，返回一个 Promise 对象。Promise 对象上拥有一个 then() 方法，我们在其中传入一个回调函数，在 HTTP response 抵达时，该回调函数会被调用。 

then() 方法虽然类似于 event listener，不过每个 then() 中的回调函数之会被调用一次。该回调函数也是异步调用的。

通常情况下，我们会以动词的形式命名会返回 Promise 的函数，以及使用 Promise 结果的函数。这使得代码更容易阅读：

```js
function showData(data) {...}; // 回调函数
getData（'www.example.com').then(showData); // 几乎等同于英文，易于阅读
```

#### 使用 Promises 处理异常

异步函数，尤其是包含网络的部分，理论上来说有很多种失败的情况，我们可以在 Promise 的 then() 方法中传入第二个函数用于处理错误：

```js
getData（'www.example.com').then(showData, handleDataError);
```

在 Promise-based 的异步运算中，如果运算成功完成了，它会将其 result 传入 then() 的第一个函数作为 argument；但如果运算失败了，它会抛出一个异常，并传入 then() 的第二个函数作为 argument。

但在大多数情况下，我们不会为 then() 传入两个函数，而是：

```js
getData（'www.example.com').then(showData).catch(handleDataError);
```

catch() 其实等同于调用了 then()，不过传入了 null 作为 then() 的第一个 argument。在此之上，catch() 也可以捕获 showData 函数中抛出的异常。


一个 Promise 必然处于一下状态之一：
- 待定（pending）
- 已兑现（fulfilled）
- 已拒绝（rejected）

假设我们已经调用了一个 Promise 的 then() 方法并且传入了两个回调函数时：
- 若第一个回调函数被调用，该 Promise 的状态便会改成 fulfilled
- 若第二个回调函数被调用，该 Promise 的状态便会改成 rejected
- 若一个 Promise 既不是 fulfilled 或者 rejected，则称其为待定
- 只要 Promise 进入 fulfilled 或者 rejected 状态，我们称它为已敲定（settled）
- settled 的 Promise 不会再改变其状态
- 一个 Promise 不会同时在 fulfilled 和 rejected 状态之下
- Promise 也可能处于已决议（resolved）的状态下，这个状态和 fulfilled 或者 rejected 都不同


#### 链式调用 Promise

Promise 带来的另一个好处就是使得表达一列异步操作更为自然，使它们不用内嵌在上一个回调函数中。

在调用一链 then() 方法时，我们不再同一个 Promise 对象上定义多个回调函数，而是每次调用 then() 时返回一个新的 Promise 对象。

#### Resolving Promise

正如前面提到的，Promise 也可能处在 resolved 的状态。我们来看下面的例子：

```js
function c1(response) { // 回调函数 1
  let p4 = response.json();
  retun p4; // 返回 promise 4
}

function c2(data) { // 回调函数 2
  showData(data);
}

let p1 = fetch('/api/data'); // promise 1, task 1
let p2 = p1.then(c1); // promise 2, task 2
let p3 = p2.then(c2); // promise 3, task 3
```

如果想要让链式调用正确的执行，task 2 的输出就必须成为 task 3 的输入（即一个 Promise 对象），而这个例子中 task 3 的输入时 fetched 到的 URL body，并被解析成了 JSON 对象。但是回调函数 c1 的返回值并不是一个 JSON 对象，而是 JSON 对象的 Promise p4。这看上去很矛盾，但其实不然：

在 p1 兑现时，c1 被调用，然后 task 2 开始执行。在 p2 兑现时，c2 被调用，然后 task 3 开始执行。虽然 task 在 c1 被调用时就开始执行了，但这并不意味着 task 2 在 c1 返回时必须结束。这意味着如果 task 2 是异步执行的，这个任务并不会在回调函数返回之前就执行完毕。

在看完上面的例子后，我们来讲一下 Promise 的 resolve 状态：

在传入回调函数 c 进入 then() 方法时，then() 返回了一个 Promise p 并且安排 c 在一段时间以后被异步调用。然后 c 执行了运算并返回了一个值 v，在回调函数返回时，p 就进入了 resolved 的状态并且持有值 v。当一个 Promise 进入 resolved 的状态并且持有一个值，且该值 v 不是 Promise 时，它会立刻进入 fulfilled 的状态，并且持有那个值。

所以说，如果 c 返回了的值 v 是 non-Promise，那个返回值就是 p 的值，且 p 立刻成为 fulfilled，对于这个 Promise 的处理就结束了；但是如果 c 返回的值 v 是一个 Promise 的话，p 就被称为 resolved 而非 fulfilled。在这个状态时，除非 Promise v settles（落定），否则 p 就不能 settles。当 Promise v fulfilled 时，p 也会成为 fulfilled 并且持有和 Promise v 一样的值；如果 Promise v rejected，p 也会因为同样的原因 rejected。

上面就解释了 Promise 的 resolve 状态是什么：即一个 Promise 与另一个 Promise 关联的状态。我们不知道 p 会成为 fulfilled 还是 rejected，不过回调函数 c 对其已经没有操纵权了，p 的命运已经全部交给了 Promise v。

这时候我们再回到刚刚的 URL fetch 的例子中，当 c1 返回 p4 时，p2 就会 resolved，但是 resolved 不等于 fulfilled，所以 task 3 还并没有开始。只有等到 p4 fulfilled 时，即 HTTP body 中的 .json() 方法也被 fetch 到然后解析出 p4 的值时，p2 才会自动变成 fulfilled。只有这个时候 task 3 才会开始。

#### Promises 和 Error

正如在前面提到的，我们可以在 then() 中传入第二个函数，用于在 Promise rejected 时调用。在第二个调用时，其 argument 会是一个表示错误发生的原因的值（通常是一个 Error 对象）。也提到了使用 catch() 来代替第二个函数。在了解了链式调用之后，我们可以更进一步的了解错误处理机制了。

对于异步编程的异常处理会变得更为复杂，因为不像同步变成在错误发生时会提供详细的 stack trace。而在很多时候都是静默发生的错误。

**catch() 和 finally() 方法**

正如前面提到的，以下两种方法是一样的：

```js
p.then(null, c); // c 为回调函数
p.catch(c);
```
在链式 Promise 中，catch() 不仅可以被加载最后，其实也可以被加在中间：

```js
doAsyncThing()
  .then(do1)
  .catch(handleDo1Error) // 若 doAsyncThing 或 do1 中抛出 error 则会处理它，否则这一行被忽略
  .then(do2)
  .then(do3)
  .catch(cleanUp);
```

除去 catch() 方法，在 ES2018 中，新增了 finally() 方法，其使用方法类似于 try/catch/finally 语句。如果在链式 Promise 中增加了 finally() 的调用，则 finally() 中的回调函数会在调用其的 Promise settle 时调用。即无论 rejected 或者 fulfilled 都会调用，且不会传入 argument。

#### 同时执行多个 Promises

如果我们想要同时执行多个异步操作时，函数 Promise.all() 可以帮我们做到。Promise.all() 接受一个数组作为 argument，数组中应当包含为 Promise 对象，然后该函数会返回一个 Promise。返回的 Promise 会在任意一个输入的 Promises rejected 错误 rejected；不然的话，即所有输入的 Promises 都 fulfilled，返回的 Promise 也会成为 fulfilled。

比如我们要从多个 URLs 同时 fetch 数据时，我们可以：

```js
let urls = [/*URLs组成的数组*/];
urlsPromises = urls.map(url => fetch(url).then(r => r.text())); // 转换成 array of Promises
Promise.all(urlsPromises).then(/*所有 URLs fetch 成功*/).catch(/*任意一个 fetch 失败*/);
```

其实 Promise.all() 还可以更简单一些，其输入的数组中也可以包括非 Promise 对象的值，它们会被当做已经 fulfilled 的 Promise 持有的值，并且会不改变它们，直接复制到输出数组中。

Promise.all() 在 rejected 时的返回值就是任何 rejected 的输入 Promise。这会在第一个 rejected Promise 出现时发生，即使还有别的 Promise 依然处于 pending 的状态下。

在 ES2020 中，新增了 Promise.addSettled()，他和 Promise.all() 的输入和输出格式一样，只不过它的返回值绝对不会是 rejected，但是在所有的 Promises 都 settled 之前，不会进入 fulfilled 状态。这个返回值 Promise 会先进入 array of objects 的 resolved 状态，数组中的每一个对象对应一个输入的 Promise。每一个返回的对象拥有一个 status 属性。若该属性为 fulfilled，则这个对象还会拥有一个 value 属性表示其对应的 Promise 的值；若为 rejected，则这个对象回有一个 reason 属性，即其对应的 Promise rejected 的原因。

```js
Promise.allSettled([Promise.resolve(1), Promise.reject(2),3])
  .then(results => {
  results[0] // { status: "fulfilled", value: 1 }
  results[1] // { status: "rejected", reason: 2 }
  results[2] // { status: "fulfilled", value: 3 }
});
```

我们还可以使用 Promise.race() 来代替 Promise.all()，它会返回其输入数组中第一个 settled 的 Promise（若输入数组有非 Promise 值，则其中第一个会被返回）。

#### 创建 Promises

##### 基于其他 Promises 的 Promises

在拥有一个 Promise 时，创建另一个新的 Promise 会十分简单。因为调用 Promise 的 then() 方法会创建并返回一个新的 Promise。

##### 基于同步值的 Promises

使用 Promise.resolve() 和 Promise.reject() 可以直接通过传入的 argument 来创建新的 Promise。Promise.resolve() 接收一个值作为 argument，并且立刻（不过是异步的）返回一个 fulfilled 的 Promise，并持有传入的那个值；Promise.reject() 接收一个 argument，然后返回一个 rejected 的 Promise，argument 成为 rejected 的原因。（它们被返回时并不是 fulfilled 或者 rejected，而是处在 pending 的状态，然后等待当前代码执行完毕，才进入 settled 状态，通常只有几毫秒)

我们在前面提到过，resolved 的 Promise 并不是 fulfilled 的 Promise。所以我们在 Promise.resolve() 中传入一个 Promise 作为 argument 时，其返回的 Promise 只有等到传入的 Promise settled 之后才会 settled

##### 从头开始创建 Promises

在不适用另一个 Promise 的时候，我们可以使用 Promise() 构造函数来创建 Promise。它接收一个函数作为其唯一的 argument，这个函数应该接收两个 arguments，通常将第一个 argument 命名为 resolve，将第二个命名为 reject。构造函数会同步的调用我们定义的函数，然后返回一个新建的 Promise。返回的 Promise 可以通过我们定义的函数来操纵，那个函数应当执行一些异步的操作，然后通过 resolve 函数来 resolve 或 fulfill 返回的 Promise，或者 reject 函数来 reject 返回的 Promise。

```js
function wait(duration) { 
  return new Promise((resolve, reject) => {  // 自定义 Promise
    if (duration < 0) { // 若 argument 小于零，Promise 成为 rejected
      reject(new Error());
    }
    setTimeout(resolve(duration), duration); // 否则等待一段时间后 Promise 被 resolve 成 fulfilled
  });
}
```

注意，Promise 构造函数中函数的 argument 名为 resolve() 和 reject() 而非 fulfill() 和 reject()。所以对 resolve() 传入一个 Promise 的情况下，它会被 resolve 成为新的 Promise。


### aysnc 和 await

ES2017 新增了两个新的关键字 async 和 await。它们代表了对于异步 JavaScript 编程的模式转换。这两个新的关键字大大减少了 Promises 的复杂程度，使我们可以如同写同步代码一样写异步基于 Promises 的代码。

#### await 表达式

await 关键字会接受一个 Promise，并将其转换成一个返回值或者抛出的异常。比如说有一个 Promise 对象 p，表达式 await p 会等待 p settles。若 p 为 fulfilled，则 await p 的值就是 p fulfilled 的值；若 p 为 rejected，则 await p 会抛出 p rejected 的原因。我们通常会在调用一个会返回 Promise 对象的函数前加上 await：

```js
let response = await fetch('/api/data');
```

await 关键字并不会阻塞程序，而且在它的 Promise settled 之前不会做任何事。

#### async 函数

因为任何使用 await 关键字的代码都是异步的，所以这有一个很重要的规则：我们只能在异步函数中使用 await 关键字，即拥有 async 关键字的函数中：

```js
async function getData() {
  let response = await fetch('/api/data');
  return response;
}
```

用 async 关键字来声明的函数意味着它的返回值会成为一个 Promise，即使在函数体中并没有直接出现关于 Promise 的代码。

### 异步迭代（ Asynchronous Iteration）

在前面我们提到过，Promise 无法解决 setInterval()，或者 click event 等会多次触发的异步操作，所以在 ES2018 中，加入了异步迭代器，它类似于前面介绍的迭代器，只不过是基于 Promise，并且使用 for await 改变了 for of 的形式。

#### for await 循环

如同于一个普通的 await 表达式，for await 循环也是基于 Promise 的，总的来说，for await 循环会等到所有的 Promises 都成为 fulfilled 之后，会为为循环变量定义值，最后再进行循环：

```js
for await(const response of responses) {
  handle(response);
}
```

#### 异步迭代器

异步迭代器和迭代器有许多相似之处，不过拥有两个很大的不同：

- 异步迭代器通过定义 Symbol.asyncIterator 方法而非 Symbol.iterator 方法来实现
- 异步迭代器对象的 next() 方法会返回一个 Promise，它会 resolve 成为迭代结果对象

#### 异步生成器

异步生成器可以通过再生成器函数前加上 async 关键字来定义。我们可以在异步生成器中使用 yield，只不过 yield 的值会被封装成一个对象。下面的例子实现了一个自定义的 setInterval()：

```js
function elapsedTime(ms) { // 用于返回新 Promise 的函数
  return new Promise(resolve => setTimeout(resolve, ms));
}
async function* clock(interval, max) { // 异步生成器
  for (let count = 1; count <= max; count++ ) {
    await elapsedTime(interval);
    yield count;
  }
}
async function tickClock() { // 只能在 async 函数中调用 await
  for await (let tick of clock(1000,60)) { // 每秒显示一次，一共六十次
    console.log(tick);
  }
}
tickClock(); 
```

### 小结

这一章的要点：

- 大多真实生活中的 JavaScript 程序都是异步的
- 原来，异步函数是通过 event 和回调实函数现的
- 在 ES6 新增的 Promises 给予了回调函数的新的结构，使其更为简洁
- 在 ES2017 新增的 async 和 await 关键字使 Promises-based 异步代码可以以同步代码的外貌编写
- 在 ES2018 新增的 for await 循环使我们可以迭代可异步迭代的对象。我们可以实现对象的 \[Symbol.asyncIterator\] 方法来使其成为可迭代对象

## 元编程（Metaprogramming）

这一章介绍了一些进阶的 JavaScript 特性，它们不一定会在日常使用中被用到，但在编写可复用的库或者框架时十分有用。

这些特性中的许多都可以被大致归类到元编程（metaprogramming）下：如果普通的编程是用编写操纵数据的代码，则元编程是用来编写其他的代码的代码。但是在 JavaScript 这种动态语言中，编程和元编程的界限是很模糊的，即使像 for in 循环中遍历的对象的能力，也被许多静态语言成为元编程。

### 属性的特性（Property Attributes）

每一个对象上的属性都拥有一个名和值，但除此之外，每一个属性其实还拥有三个与他们关联的特性（Attributes）用于指定这个属性的行为等：

- writable attribute 用来指定这个属性的值是否可以被 set
- enumerable attribute 用来指定这个属性是否会在 for/in 循环中被返回
- configurable attribute 用来指定这个属性是否可以被删除，和这个属性的特性（attribute）可否被修改

在对象字面量中定义的属性会自动将上述属性特性设置为 true，但许多 JavaScript 标准库中对象的属性则不然。这一章会介绍用于查询以及修改这些属性的 APIs，它们在编写框架或库的时候会格外有用，因为：

- 这使得作者可以为对象添加 non-enumerable 的方法，就如同内嵌方法一样
- 也使得作者可以锁定其对象，使其属性无法被更改或者删除

为了便于理解，我们可以将属性的值也称为该属性的特性之一，即每一个属性都用有一个名，以及四个于其关联的特性（value, writable, enumerable, configurable）。访问器属性（即由 getter 和 setter 定义的属性）不拥有 value 和 writable 特性，而其是否能读写会根据其否有 setter 和 getter 而决定。所以访问器属性有以下四个特性（get, set, enumerable, configurable）

在 JavaScript 中，用于查询以及修改这些属性的特性的方法是是使用了一个名为 property descriptor 的对象用于表示这四个特性。每一个 property descriptor 对象上都拥有于属性的特性同名的属性（get, set, value, writable, enumerable, configurable）。其中除了 get，set 和 value 都是布尔值，而 get 和 set 则是函数值。

用于取得某个属性的 property descriptor 对象的方法是调用 Object.getOwnPropertyDescriptor()，传入对象以及其属性名作为 arguments：

```js
Object.getOwnPropertyDescriptor({x:1}, 'x'); // arguments 是对象以及属性名
// 返回 property descriptor 对象：{value: 1, writable: true, enumerable: true, configurable: true}

let a = {get name() {return 'Jake';}; // 在对象中定义了一个只读访问器属性
Object.getOwnPropertyDescriptor(a,'name');
// 返回 property descriptor 对象： {set: undefined, enumerable: true, configurable: true, get: ƒ}

// 对于不存在以及继承下来的属性，会返回 undefined
Object.getOwnPropertyDescriptor({}, 'x'); // undefined，属性不存在
Object.getOwnPropertyDescriptor({}, "toString") // undefined，属性为继承的
Object.getOwnPropertyDescriptor(Object.getPrototypeOf({}), "toString") // 访问其原型，然后取得属性特性
// 返回 {writable: true, enumerable: false, configurable: true, value: ƒ}
```
 
 正如它的名字所表达，这个方法只能获取私有属性（OwnProperty）的 property descriptor 对象。
 
 如果想要修改一个而属性的特性，或者创建一个持有指定特性的新的属性，我们需要调用 Object.defineProperty() 方法，传入我们对象，属性名，property descriptor 对象作为其 arguments：
 
```js
let o = {};
 
Object.defineProperty(o, 'x', { // 新增属性 x，并定义其特性
  value: 1, // 也可以为其赋值
  writable: true,
  enumerable: false,
  configurable: true
}); // 该方法会返回修改后的对象 o

o.x // 1
Object.keys(o); // []，因为设置其 x 属性的 enumerable 特性为 false，所以不出现在 keys 中
Object.defineProperty(o, "x", { enumerable: true }); // 修改 enumerable 为 true
Object.keys(o); // ['x']

Object.defineProperty(o, "x", { writable: false }); // 使属性 x 无法被赋值
o.x = 2; // 静默失败，或者在严格模式下抛出 TypeError
o.x // 1
Object.defineProperty(o, "x", { value: 2 }); // 因为属于依然 configurable，所以可以这样赋值
```

我们在传入 Object.defineProperty() 的 property descriptor 中，并不一定需要设定全部的特性，我们可以只设定一部分。在创建新属性时，被忽略的特性会默认设置为 false 或 undefined；或者在修改已有属性时，被忽略的特性会保持不变。这个方法也无法修改从对象原型上继承的属性。

我们还可以使用 Object.defineProperties() 来同时修改或创建多个属性。它的第一个 argument 是需要修改的对象，第二个 argument 也是一个对象，该对象用于映射属性名以及其 property descriptor：

```js
let o = Object.defineProperties({}, {
  x: {value: 1, writable: true, enumerable: true, configurable: true},
  y: {value: 2, writable: true, enumerable: true, configurable: true},
  sum : { get() { return this.x+this.y;}, enumerable: true, configurable: true}
});
o.sum // 3
```

其实在前面介绍过的： Object.create() 方法也可以接收和 Object.defineProperties() 一样的第二个 argument 来作为新增的属性；Object.assign() 只能复制 enumerable 的属性和它们的值，但无法复制属性的特性。

Object.defineProperty() 和 Object.defineProperties() 会在尝试创建或者修改不允许改动的属性时抛出 TypeError。以下是详细的报错可能：

- 如果一个对象是非 extensible，则我们无法为其添加属性（可修改已有属性）
- 如果一个属性是非 configurable，则我们无法改变其 configurable 或者 enumerable 特性
- 如果一个访问器属性是非 configurable，则我们无法改变其 getter 或 setter 方法
- 非 configurable 的属性无法改变成访问器属性，反过来也不行
- 如果一个属性是非 configurable，则我们无法将 writable 特性从 false 改为 true，但可以从 true 改为 false
- 如果一个属性既是非 configurable 又是非 writable，则我们无法改变它的值

### 对象的可扩展性（Object Extensibility）

extensible（可扩展性）是对象的 attribute（特性）。它指定了该对象可否增加新的属性。普通的 JavaScript 对象会默认 extensible，不过我们可以改变它。

想要判断一个对象石佛 extensible，我们可以将其做为 argument 传入 Object.isExtensible() 来判断。如果我们想要让一个对象改为 non-extensible，我们只需要将其作为 argument 传入 Object.preventExtensions()。在对 non-extensible 的对象添加属性时，会在严格模式下抛出 TypeError，非严格模式下静默失败。在尝试去改变 non-extensible 的对象的原型时，会抛出 TypeError。

需要注意的是，在将一个对象改为 non-extensible 之后，无法将其再改回 extensible。还有就是 Object.preventExtensions() 只会改变该对象的 extensible，所以我们依然可以为其原型增加新的属性，non-extensible 的对象也会进程那些新的属性。

对象的 extensible 特性的目的就是为了锁住该对象的状态，并防止外界的篡改。extensible 特性通常会和属性的 writable，configurable 关联，所以我们可以使用以下函数一起定义它们：

- Object.seal() 的用法和 Object.preventExtensions() 一样，只不过 Object.seal() 会同时修改对象上的所有属性的 configurable 属性，将它们设置为 false。这意味着新属性无法添加，原有属性也无法被删除或者 configure，但是 writable 属性不会被改变。在 seal（封印）一个对象之后，我们无法再将其 unseal（解封)，但可以使用 Object.isSealed() 来查看对象是否 sealed
- Object.freeze() 可以更进一步的锁定对象，它会在改变对象为 non-extensible，改变属性为 non-configurable 以外，同时将所有的非访问器属性改为只读的（访问器属性依然可以通过 setter 方法去改变它）。我们可以通过 Object.isFrozen() 来查看其是否被冻结

这两个方法都只会影响传入的对象 argument，而不会影响它们的原型。所以想要彻底的锁定一个对象，可能需要同时去其原型链中锁定更多的原型对象。

Object.preventExtensions(), Object.seal(), Object.freeze() 调用之后都会返回其接收的对象。

如果我们在编写 JavaScript 库， 并且在用户定义的回调函数中返回对象的情况下，我们可以通过 object.freeze() 来使得用户无法改变对象。

###  原型特性（The prototype Attribute）

一个对象的 prototype attribute（特性）会指向其原型，因为这个特性十分重要，我们在很多情况下都会简称其为 对象的 prototype，而非对象的 prototype 特性。

prototype 特性会在对象被创建时一同被定义。通过对象字面量创建的对象回一 Object.prototype 作为其原型，使用 new 创建的对象会用其构造函数上的 prototype 属性作为原型，使用 Object.create() 则会用其第一个 argument 作为原型。

我们可以使用 Object.getPrototypeOf() 来查询对象的原型：

```js
Object.getPrototypeOf({}) // Object.prototype
Object.getPrototypeOf([]) // Array.prototype
Object.getPrototypeOf(()=>{}) // Function.prototype
```

我们可以通过 isPrototypeOf() 来检查一个对象是否为另一个对象的原型：

```js
let p = {x: 1}; // 定义一个原型对象
let o = Object.create(p); // 继承原型对象的对象
p.isPrototypeOf(o); // true，因为 o 继承了 p
Object.prototype.isPrototypeOf(p); // true，因为 p 继承了 Object.prototype
Object.prototype.isPrototypeOf(o); // true，o 也继承了 p
```

虽然通常情况下在创建完一个对象时会同时定义其 prototype 特性并且不再改变，但我们也可以通过 Object.setPrototypeOf() 来修改一个对象的原型：

```js
let o = {x: 1};
let p = {y: 2};
Object.setPrototypeOf(o, p); // 将 o 的原型改为 p
o.y // 2
```

因为 JavaScript 会默认对象的原型在其创建之后就是固定的，所以在使用 Object.setPrototypeOf() 修改原型之后可能会进行一些反向优化，使得引用改变过的对象的代码运行速度大大降低。

某些早期的浏览器对于 JavaScript 的实行为对象暴露了 \_\_proto\_\_ （左右各两个下划线)属性来指向 prototype 特性。虽然该特性现在已经不推荐使用了，但有许多的早起网页依然依赖于 \_\_proto\_\_，这使得 ECMAScript也对其进行了标准化。\_\_proto\_\_ 是一一个可读写的属性，我们可以用其代替 Object.getPrototypeOf() 和 Object.setPrototypeOf()（但不推荐！）：

```js
let p = {y: 2};
let o = {
  x: 1,
  __proto__: p // 将 o 的原型改为 p，不推荐这么做
};
o.y
```

###  Well-Known Symbols

Symbol 类是在 ES6 引入 JavaScript 的，引入 Symbol 的主要原因之一就是为了更安全的增加 extensions（在不改变打破原有代码兼容性的基础上）。我们在之前见过 Symbol 作为属性名的属性 Symbol.iterator。

Symbol.iterator 是 well-known Symbols 的例子之一，well-known Symbols  就是在 Symbol() 工厂函数中储存的 Symbol 值的集合，这些 Symbols 可以通过操纵对象或者类的底层的行为来。接下来会介绍 JavaScript 中的 well-known Symbols。

#### Symbol.iterator 和 Symbol.asyncIterator

这两个 Symbols 使得对象或者类可以让它们成为可迭代的。更多的细节可以在前两章的迭代器和生成器以及异步 JavaScript 中找到。

#### Symbol.hasInstance

正如之前提到过的，instanceof 操作符需要在右边传入一个构造函数。比如表达式 o instanceof f，它会先去在 o 的原型链中查找 f.prototype。

在 ES6 之后，Symbol.hasInstance 提供了 instanceof 的一个变种。如果 instanceof 右边是一个拥有 \[Symbol.hasInstance] 方法的对象，则那个方法会被调用，调用时传入左边的值作为 argument，然后该方法的返回值会转换成一个布尔值，成为 instanceof 操作符的结果。只有在右边不是一个拥有 \[Symbol.hasInstance] 方法的对象，才会以传统的方法调用：

```js
let even = {
  [Symbol.hasInstance](x) {
    return !(x%2);
  }
}
33 instanceof even // false
44 instanceof even // true
```

#### Symbol.toStringTag

在对 JavaScript 默认对象调用 toString() 方法时，默认返回的是字符串 "\[object Object]"：

```js
{}.toString(); // "[object Object]"
```

但如果调用同样的 Object.prototype.toString() 函数，不过将其作为某一个内嵌类型的实例的话，会发生奇怪的现象：

```js
Object.prototype.toString.call([]) // => "[object Array]"
Object.prototype.toString.call("") // => "[object String]"
Object.prototype.toString.call(()=>{}) // => "[object Function]"
```

其实我们可以使用 Object.prototype.toString().call() 这个方法来取得任何 JavaScript 的值的类。所以我们可以定义这样的一个方法来取得 JavaScript 值的类：

```js
function classof(o) {
  return Object.prototype.toString.call(o).slice(8,-1);
}
classof(null); // 'Null'
classof(10n); // 'BigInt'
classof(/a/); // 'RegExp'
```

在 ES6 之前，Object.prototype.toString() 方法的这一特性只能对内嵌类型的实例进行判断，在对于自定义的对象则会返回 'Object'。

但在 ES6 之后，Object.prototype.toString() 还会去查询其 argument 对象上的 \[Symbol.toStringTag] 属性，如果有的话，则会使用那个属性的值作为返回值：

```js
class MyClass {
  get [Symbol.toStringTag]() {
    return 'MyClass';
  }
}
let temp = new MyClass();
Object.prototype.toString.call(temp); // "[object MyClass]"
temp.toString(); // 未被覆写，也可以直接调用 toString()，返回"[object MyClass]"
classof(temp); // "MyClass"，若调用前面定义的函数
```

####  Symbol.species

在 ES6 之前，对于内嵌的类（如数组），我们其实很难去创建一个它强壮的的子类。不过在 ES6 之后可以使用 extends 关键字来简化操作：

```js
class MyArray extends Array { // 数组的子类
  get first() { return this[0]; }
  get last() { return this[this.length-1]; }
}
```

但是在数组上的方法 concat(), slice(), map(), filter(), splice() 会返回新的数组。那么我们 MyArray 在继承数组之后也会继承这些方法，不过会返回普通的数组还是我们定义的 MyArray 呢？其实两者都可以，不过 ES6 指定了默认返回值为子类。它具体的实现如下：

- 在 ES6 及以后，Array() 构造函数拥有一个名为 Symbol.species 的属性（大多数别的 well-known Symbols 都表示了原型对象上的方法名，而这个表示了构造函数的属性名）

- 在使用 extends 关键字创建子类时，子类的构造函数也会继承其超类构造函数的属性。所以子类的构造函数也拥有了 Symbol.species 这一属性（我们也可以定义同名的属性，不过不同的值）

- map() 和 slice() 之类的方法会调用 new this.constructor\[Symbol.species]() 来创建新的数组

- 接下来就是有趣的部分了，假设 Array\[Symbol.species] 是一个普通的数据类型的话，我们呢可以这么定义它：

  ```js
  Array[Symbol.species] = Array;
  ``` 
  
  在这种情况下，子类会继承其 species，调用 map() 之类的方法会返回普通的数组。但这并不是 ES6 的默认行为。ES6 的默认情况下，Array\[Symbol.species] 是一个只读的访问器属性，它的 getter 会返回 this。 所以子类的构造函数在继承 getter 函数之后，也会返回 this，即每一个子类会返回它自己的 species。
  
- 如果想要改写这种默认行为，就比如让前面 MyArray 的继承的方法返回 Array 的话，我们只需要将 MyArray\[Symbol.species] 的值设置成为 Array 即可。但是因为这是一个只读属性，我们无法直接赋值，而是要使用 defineProperty()：

  ```js
  MyArray[Symbol.species] = Array; // 尝试写入只读属性会静默失败，在严格模式下会抛出 TypeError
  Object.defineProperty(MyArray, Symbol.species, {value: Array}); // 可以这么做
  ```
  
- 或者可以显式地定义 Symbol.species 的 getter，这种方法更为简单：

  ```js
  class MyArray extends Array { // 数组的子类
    static get [Symbol.species]() { return Array; } // 
    get first() { return this[0]; }
    get last() { return this[this.length-1]; }
  }
  let a1 = new MyArray(1,2,3);
  let a2 = new a1.map(x => x); // 创建的是普通数组，而非 MyArray
  a1.last; // 3
  a2.last; // 普通数组不具有 last 属性
  ```
  
#### Symbol.isConcatSpreadable

数组的 concat() 方法也会使用上面的 Symbol.species 来判定返回数组的类型，不过 concat() 也会使用 Symbol.isConcatSpreadable。以为在 concat() 方法中，arguments 既可以是数组也可以是非数组，数组会被先 falttened，而非数组不会。

在 ES6 之前，concat() 方法使用 Array.isArray() 来判断其 arguments，而在 ES6 之后，其算法进行了一些改变，如果 arguments 或者 this 的值是一个拥有 Symbol.isConcatSpreadable 属性的对象，那么这个属性会用于判断是否要将 argument spread。如果该属性不存在，才会使用 Array.isArray()。

比如我们在创建类数组对象时可以将其 Symbol.isConcatSpreadable 设置为 true：

```js
let arrayLikeObject = {
  length: 1,
  0: 1,
  [Symbol.isConcatSpreadable]: true
}
let a = []
a.concat(arrayLikeObject) // [1]，而非 [[1]]
```

数组的子类的该属性会默认设置为 true，而如果我们不像让其被 falttened 的话，就可以将 Symbol.isConcatSpreadable 设置为 false：

#### Pattern-Matching Symbols

对每一个和正则表达式有关的的字符串方法，即 match(), matchAll(), search(), replace(), split9) 都拥有一个其对应的 well-known SYmbol 如 Symbol.match。

虽然正则表达式十分强大，但在复杂的时候会变得难以阅读，所以我们可以定义这些字符串方法，为其定义自定义的匹配机制。

#### Symbol.toPrimitive

我们在前几章提到过 JavaScript 中三种对象转换成为基本值的逻辑。简单的说在期待一个字符串时，优先调用 toString()，若 toString() 未被定义或不返回基本值，则调用 valueOf()；在期待一个数字时，则先调用 valueOf()；或者在没有期待的情况下，可以让定义的类自己定义转换逻辑。如 Date 优先使用 toString()，而别的类型会优先使用 valueOf()。

ES6 中新增的 Symbol.toPrimitive 是我们可以改写默认的对象转换成基础值的逻辑。Symbol.toPrimitive 代表的方法需要返回一个基础值，用于表示该对象，该方法接收一个字符串作为 argument，用来表示使用什么类型的转换逻辑：

- 若为 'string'，则表示 JavaScript 正在进行期待字符串的转换，如模板字面量中，，即反引号中
- 若为 'number'，则表示 JavaScript 正在进行期待数字的转换，如 >, <, -, *
- 若为 'default'，则表示 JavaScript 正在进行数字和字符串都可以的转换，如 +, ==, !=

### Reflect API

Reflect 对象并不是一个类，就如同 Math 对象，其属性就是与其相关的函数。这些函数在 ES6 被加入，定义了用于反射对象和其属性的 API。Reflect 的函数虽然没有带来新的特性，不过它们把已有的特性组合，使使用起来更为方便。Reflect API 包含了一下函数：

**Reflect.apply(f, o, args)**

这个函数会在 o 上以其方法的形式调用 f，并传入 args 数组中的元素值作为 arguments。等同于 f.apply(o, args)。

**Reflect.construct(c, o, newTarget)**

这个函数会调用构造函数 c，就如同 new 关键字那样，并传入 args数组的元素值作为 arguments。如果可选的 newTarget 存在，它便会成为构造函数调用中的 new.target 的值，如忽略的话，new.target 的值就是构造函数 c。

**Reflect.defineProperty(o, name, descriptor)**

这个函数可以定义对象 o 的一个属性，属性名为 name，descriptor 对象则需要inyi那个属性的值和特性。Reflect.defineProperty 和 Object.defineProperty 十分类似，不过 Reflect.defineProperty 会在成功和失败时返回 true 和 false，而非 Object.defineProperty 的 o 和 TypeError。

**Reflect.deleteProperty(o, name)**

类似于 delete o\[name]，不过会返回 true 或者 false。

**Reflect.get(o, name, receiver)**

返回对象 o 上的属性 name 的值，如果属性是一个访问器属性，则可以接收第三个可选的 receiver 作为 argument，则 getter 函数会以 receiver 的方法来调用。

**Reflect.getOwnPropertyDescriptor(o, name)**

几乎等同于 Object.getOwnPropertyDescriptor()，不过 Reflect 版本中会在第一个 argument 不为 Object 时抛出 TypeError。

**Reflect.getPrototypeOf(o)**

几乎等同于 Object.getPrototypeOf()，不过 Reflect 版本中会在第一个 argument 不为 Object 时抛出 TypeError，而非只在 null 或者 undefined 时抛出。

**Reflect.has(o, name)**

类似于 name in o。

**Reflect.isExtensible(o)**

类似 Object.isExtensible()，不过会在传入 argument 不是对象时抛出 TypeError

**Reflect.ownKeys(o)**

该函数返回属性名的数组，在 o 不是对象时抛出 TypeError 等同于调用 Object.getOwnPropertyNames() 和 Object.getOwnPropertySymbols()，并组合它们的返回值。

**Reflect.preventExtensions(o)**

等同于 Object.preventExtensions()，不过会在 o 不为对象时抛出 TypeError，并且在成功修改时返回 true 而非 o

**Reflect.set(o, name, value, receiver)**

基本等同于 o\[name] = value，若有传入 receiver，则会在 receiver 上调用，而非 o 上调用，若 o 不是对象，抛出 TypeError，返回布尔值表示成功或失败。

**Reflect.setPrototypeOf(o, p)**

类似于 Object.setPrototypeOf()，不过返回布尔值表示成功失败，并在 o 或 p 不是对象时抛出 TypeError。

### Proxy 对象

Proxy 类时在 ES6 中新增的，它是 JavaScript 元编程中最强大的特性。他是我们可以改写 JavaScript 对象最基本的行为。上面提到的 Reflect API 描述了可以访问 JavaScript 对象的 operations 的函数。而 Proxy 类是我们可以定义这些基础的 operations，而非仅仅访问它们，这是我们可以创建普通对象无法创建的对象。

在创建新的 Proxy 对象时，我们会传入两个 arguments，第一个为目标对象，第二个为 handlers 对象：

```js
let proxy = new Proxy(target, handlers);
```

返回的 Proxy 对象并不拥有私用的状态或者行为，在我们对 Proxy 对象进行操作时（如读写属性，加入属性，查找原型），Proxy 对象会将这些操作 dispatch 到目标对象或者 handlers 对象。

Proxy 支持的操作等同于那些被 Reflect API 所定义的操作。比如这有一个 Proxy 对象名为 proxy，我们可以这么写：delete proxy.x。Reflect.deleteProperty(target, x) 函数拥有和 delete 操作符一样的行为，所以在对 Proxy 对象使用 delete 操作符来删除属性时，它会在 handlers 对象上寻找一个名为 deleteProperty() 的方法。若存在，则调用它，若不存在，Proxy 对象则会在目标对象上进行属性删除。

Proxy 对于所用的基本操作都有这样的行为：若对应的方法存在于 handlers 对象上的话，它会调用那个方法来执行操作（对应的方法名和前面 Reflect 函数名相同）；若 handlers 对象上不存在对应的方法，则会在目标对象上进行该操作（即原来的操作方法）。所以说，Prroxy 可以从 handles 对象上取得目标对象的行为并覆写。如果 handler 对象为空的话，Proxy 对象其实就是目标对象的一个透明包装罢了：

```js
let o = {x:1, y:2};
let p = new Proxy(o, {});
p.x // 1
delete p.y // true，o 上的 y 被删除，所以 Proxy 对象 p 上的 y 也被删除
p.y // undefinded
o.y // undefinded
p.z = 3; // 在 p 上定义的属性等同于在 o 上定义属性
p.z // 3
o.z // 3
```

所以说，这种透明包装的 Proxy 基本上等同于被包装的对象，所以没有必要这么做。不过在创建可撤销的（revocable） proxy 时，这个透明包装还可能有点用。我们可以使用 Proxy.revocable() 来代替 Proxy()  构造函数用于创建并返回一个对象。该对象拥有两个属性，第一个属性是 proxy 对象，第二个属性则是 revoke() 方法，在调用该方法时，proxy 会立即撤销其对目标对象的访问权力：

```js
function returnOne() { return 1; };
let {proxy,revoke} = Proxy.revocable(returnOne, {}); // proxy 也可以包装函数
proxy(); // 1，proxy 可以访问其中的对象
revoke(); // 这种访问关系可以被撤销
proxy(); // TypeError，无法调用已经被 revoked 的 proxy
returnOne(); // 原函数正常使用
```

但如果我们传入的 handlers 不是一个空对象的话，则我们定义的就不再是一个透明包装了，而是定义了 proxy 的行为。只要对于 proxy 的设置的当，我们其实就不再需要去直接操作其目标对象：

比如下面的例子中，proxy 就可以保住我们包装出一个拥有无限属性的只读对象，且属性值等同于属性名，即使目标对象其实是一个空对象：

```js
let target = {}; // 目标对象为空对象
let proxy = new Proxy(target, {
  get(o, name, target) { return name; }, // 所有的属性都拥有其属性名作为值
  has(o, name) { return true; }, // 拥有无限，即所有属性
  ownKeys(o) { throw new RangeError('Infinite properties'); }, // 无法取得无限的集合
  getOwnPropertyDescriptor(o, name) { // 只读属性
    return {
      value: name,
      enumerable: false,
      writable: false,
      configurable: false
    };
  },
  set(o, name, value, target) { return false;}, // 只读对象，无法改写属性
  deleteProperty(o, name) { return false; }, // 只读对象，无法删除
  defineProperty(o, name, desc) { return false; }, // 只读对象，无法新增属性
  isExtensible(o) { return false; }, // 不可延展
  getPrototypeOf(o) { return null; }, // 没有原型
  setPrototypeOf(o, proto) { return false; } // 无法改变其原型
});

proxy.x // 'x'
proxy.toString // 'toString'
proxy[321] // '321'
proxy.x = 1; // 无法写入
proxy.x // 'x'
Object.keys(proxy); // RangeError: Infinite properties
target // {}，目标对象依然为空
```

Proxy 对象可以通过 handles 对象改写其目标对象的行为，不过上面的例子中，只用到了这两个对象中的一个，不过在同时使用这两个对象时，Proxy 对象会变得更为有趣。

比如下面的例子，我们使用 Proxy 对象创建了目标对象的一个只读包装，在写入时抛出 TypeError。这种 proxy 可能会在编写测试的时候帮助到我们，比如我们在编写一个接收对象作为 argument 的函数，而且希望那个函数不去改写输入的对象，我们就可以这样用 proxy 对其进行包装：

```js
function readOnlyProxy(o) { // 用于将对象包装成为 proxy 的函数
  function readonlyError() { // 用于抛出异常的函数，等下会被重复调用
    throw new TypeError('Readonly');
  }
  return new Proxy(o, { // 返回只读 proxy
    set: readonlyError,
    defineProperty: readonlyError,
    deleteProperty: readonlyError,
    setPrototypeOf: readonlyError
  });
}

let o = {x:1, y:2}; // 普通对象
let p = readOnlyProxy(o); // 将对象包装成一个只读 proxy
p.x // 1
p.x = 2; // TypeError: Readonly
delete p.x; // TypeError: Readonly
p.z = 3; // TypeError: Readonly
p.__proto__ = {}; // TypeError: Readonly
```

另一种使用 proxy 的方法就是在 handlers 对象上拦截目标对象上的原有操作，执行一些语句，然后再正常的进行目标对象上的操作。因为 Reflect API 提供的函数和 handlers 方法同名，所以在 handlers 的方法中调用 Reflect API 中的函数会十分便捷：

```js
let handlers = { // 定义 handlers 对象
  get(target, property, receiver) { // 拦截 get 行为
    console.log('getting it...'); // 执行一些代码
    return Reflect.get(target, property, receiver); // 同时返回 get 行为原本的返回值
  }
}
let p = new Proxy({x:1},handlers); // 创建 proxy
p.x // 返回 'getting it ...' 和 1
```

### 小结

这一章的要点：

- JavaScript 对象拥有一个名为 extensible 的特性决定
- 对象的属性拥有 writable，enumerable 和 configurable 特性决定
- Symbol 中拥有一些特例被称为 well-know Symbols，使用这些名字的 Symbol 会改变对象的行为
- Proxy 类和其关联的 Reflect API 是我们可以对 JavaScript 的对象的底层行为进行操纵

## 浏览器中的 JavaScript（JavaScript in Web Browsers）

JavaScript 在 1994 诞生时，最初的目的就是为了给浏览器加上动态的行为。从那以后，JavaScript 发生了很大的进化，它的范围和能力也在飞速增长。如今，网页可以成为用 JavaScript 进行开发的完整平台了。我们可以使用 JavaScript 为网页进行：

- 操作文本的内容和风格
- 判断文本元素的在屏幕上的位置
- 创建可复用的展示组件
- 画图
- 生成以及播放音乐
- 管理浏览器的导航栏和历史记录
- 在网络上交换数据
- 在用户本地存储数据
- 进行多线程运算

### 网页编程基础（Web Programming Basics）

这一章介绍了对于网页的 JavaScript 代码的结构是如何的，是如何引入浏览器的，如何操作输入和输出，以及如何执行异步代码的。

#### HTML \<script\> 标签中的 JavaScript

浏览器可以显示 HTML 文本，但是若想要让浏览器执行 JavaScript 代码，则必须将 JavaScript 的代码（或代码的引用）加入 \<script\> 标签中。

JavaScript 可以以 inline 的形式插入 HTML 的 \<script> 和 \</script> 标签中，但是我们通常会使用 \<script\> 的 src 属性（attribute）来指定包含 JavaScript 的 URL：

```html
<script src="scripts/code.js"></script>
```

注意不要忘了结束标签。使用 src 属性代替 inline script 拥有以下的好处：

- 使 HTML 文件更为整洁，即分割内容和表现行为
- 当多个网页公用同一段 JavaScript 代码使，使用 src 只用维护一段代码
- 即使一个 JavaScript 文件被多个网页使用，它也只有在第一次会被下载，剩余的网页会从浏览器缓存（cache）中取得它
- 因为 src 使用 URL 作为值，网页也可以从别的服务器上获取 JavaScript 文件

**模块**

在 JavaScript 模块的部分介绍了用 import 和 export 来将代码分割成模块，如果要加载模块，则在 top-level 模块加载时使用的 \<script> 中需要加入 type="module" 属性，这样该模块以及其子模块都会被递归加载：

```html
<script src="scripts/mainModule.js" type="module"></script>
```

**指定 script 的 type 属性**

在从前，浏览器可能拥有除 JavaScript 外的别的语言，所以有时需要加上 language="javascript" 和 type="application/javascript" 的属性。但是现在大可不必，因为 JavaScript 是默认且唯一的浏览器语言。要修改 script 标签中的 type 属性只会是因为以下两种情况：

- 指定该 script 为模块
- 嵌入数据的同时不进行展示（会在之后介绍）

**script 中的 async 和 deferred**

因为在原来，JavaScript 中唯一用来改变网页展示文本的方法就是使用 document.write()，而该方法必须在 HTML 加载前执行。

所以这一方法使得 script 标签会默认先运行 JavaScript 代码，再进行剩余 HTML 的渲染。而这种默认方法会阻塞王爷的渲染，大大减缓渲染速度。

为了解决这一情况，script 标签可以用有 defer 或者 async 属性，用于改写其默认运行方式。这两个属性都是布尔值，所以只需要再 script 标签中出现即可，且只有于 src 属性同时出现才有意义：

```html
<script defer src="deferred.js"></script>
<script async src="async.js"></script>
```

这两个属性告诉浏览器加载的 JavaScript 代码不适用 document.write()，可以放心的继续加载页面，并在后台继续下载 script。

defer 属性使浏览器推迟 script 的执行到文本已经被加载和解析完成之后，即可被操纵之后；async 属性使浏览器在不阻塞文本解析的情况下，尽可能早的执行 script（须在 script 下载完成之后）。如果两个属性都出现在了同一个 script 标签下，async 优先级更高。

若有多个 script 标签，defer 依然会以 HTML 中的顺序执行，而 async 则会在加载完之后直接执行，所以顺序可能会被打乱。

type="module" 的 script 会默认在 document 加载后执行，即和 defer 一样。不过我们也可以用 async 改写它，使其模块加载完成后便直接执行。

或者说我们可以把代码放在 HTML 文件的最后，这样它们也会在 document 解析完成之后开始运行。

#### DOM（Document Object Model）

JavaScript 客户端程序中最重要的对象之一就是 Document 对象，它表示了页面展示的 HTML 文本。操作 HTML 文本的对象被成为 DOM（Document Object Model）。这里会做简单的介绍，下面的章节会有更详细的内容。

HTML 中的文本包含了 HTML 的元素，元素也会嵌套在别的元素之中：

```html
<html>
  <head>
    <title>Sample Document</title>
  </head>
  <body>
    <h1>An HTML Document</h1>
    <p>This is a <i>simple</i> document.
  </body>
</html>
```

top-level 的 html 标签包含了 head 和 body 标签，head 又包含了 title 标签，body 包含了 h1 和 p 标签，p 又包含了 i 标签。

DOM API 会以树形结构来映射出 HTML document。对于每一个 HTML 标签中，这都会对应一个 JavaScript Element 对象；每一个 document 中的文本都会有一个对应的 Text 对象。Element 和 Text 类，以及 Document 类，它们都是 Node 类的子类。Node 对象被组织成为树形结构，这使得 JavaScript 可以用 DOM API 对其进行查询以及遍历。下图为上面例子的树形结构：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a86897a71a4445f886e6e5cc9e355bb3~tplv-k3u1fbpfcp-watermark.image)

DOM API 包含了用于创建新的 Element 以及 Text 节点的方法，以及移动或者移除它们的方法。

对于每一个 HTML 的标签类型，JavaScript 中都有一个对应的类。每次 HTML 中该标签的出现，都会以该标签对应的类的实例形式在 JavaScript 中表示。比如 body 标签会以 HTMLBodyElement 类的实例表示。每一个 Element 对象都拥有对应 HTML 标签属性的属性。比如 HTMLImageElement 类的实例，即 img 标签，都有一个 src 属性表示了 HTML 中 img 标签中对应的 src 属性，且值也会从 img 标签中继承，在 JavaScript 中改变该对象的值，也会改变 HTML 中 img 标签的中 src 的值，并使得浏览器重新渲染。

#### 浏览器中的全局对象

对于浏览器的每一个页面，这都有一个全局对象，所有的 JavaScript 代码（除了在 worker 线程运行的），都运行在这个全局对象上。

全局对象就是 JavaScript 标准版被定义的地方，比如 parseInt() 函数，Math 对象，Set 类等等。在浏览器中，全局对象也包含了各种 web APIs。比如 document 属性标示了当前展示的 document，fetch() 方法可以进行 HTTP 请求等。

在浏览器中，请求对象还有一个任务。除了定义内嵌的类型和函数外，它还表示了当前浏览器窗口定义的属性如 history，innerWidth 等。

全局对象还拥有一个名为 window 的属性，它的值指向了全局对象本身。在使用窗口指定的特性时，加上 window 前缀会使得表达更清晰。

**共享命名空间的 scripts**

对于 module script 来说，它们拥有其私用的命名空间，即不进行 export 的话，是无法被访问到的。但是对于非 module script，情况则不是这样，所有的 scripts 都会共有一个命名空间，即在一个 script 中的 top-level 定义的变量，在另一个 script 中也可以被访问。

#### JavaScript 程序的执行

JavaScript 程序可以认为是一个 HTML 文件中所有的 scripts。程序的执行可以分为两个阶段：

- 第一阶段是在 document 内容加载完成时，scripts 中的代码会开始执行，默认是情况下或根据其出现的顺序来执行，但可以通过前面提到的 defer 和 async 来改变。对于每一个 script，其都会被从上到下执行。某些 scripts 可能在一开始并不会做任何事，而只是定义了一些函数和类，以便于在第二阶段使用。而某些 scripts 可能在这第一阶段执行了许多工作，而在第二阶段并没有什么事情需要做。比如某一个 script 的作用是提取 document 中所有的 h1 和 h2 来生成并且渲染 table of content，这个 script 就全部在第一阶段执行。

- 第二阶段是在 document 内容加载完，并且所有的代码都执行过一次之后进入的。这个阶段是异步，事件驱使的。如果一个 script 想要参与第二阶段，她至少需要在第一阶段中注册 event handler 或者其他的异步 callback 函数。在这第二阶段中，浏览器会在事件发生时，调用对应的 callback 函数。例如用户点击，按下键盘，加载资源等。

在第二阶段中最先执行的一些事件被称为 DOMContentLoaded 以及 load 事件。DOMContentLoaded 会在 HTML document 加载和解析完毕后触发，load 事件是当所有的 document 中的外部资源（比如图片）也加载完毕后触发。JavaScript 程序通常会调用其中一个来进行一些初始化的设置。

JavaScript 的 loading 阶段相较之下是非常短暂的，理想状态下使少于一秒的。在 loading 阶段结束后，第二阶段，也就是 event-driven 阶段会一直持续，直到浏览器窗口不再显示。

##### 客户端 JavaScript 线程 model

JavaScript 是一个单线程的语言，编写单线程会比较简单：我们可以认定两个 event handlers 不会同时运行；我们也可以知道在修改 document 时，没有其他的线程会对其同时进行修改；所以也不需要考虑 locks，deadlock 或者 race conditions 等。

但单线程执行也意味着在执行 event handlers 时，浏览器会停止对于用户输入的响应，这使 JavaScript 也拥有了弊端：如果一个 event handlers 执行了太久，则在那一段时间内浏览器是完全无响应的，这会使得用户认为浏览器崩溃了，并且会减少用户体验。

为了解决这一情况，浏览器平台定义了 web worker。它是一个后台线程，即使在其中执行长时间的任务也不会使浏览器不响应。web worker 会在接下来被介绍。

##### 客户端 JavaScript 时间轴

我们已经提到了 JavaScript 程序的两个阶段，它们其实可以被细分为以下的步骤：

1. 浏览器创建 Document 对象，并开始解析 HTML 文件，将 HTML 的元素以 Element 对象和 Text 节点的形式加入 Document 对象。在这一状态下，document.readyState 拥有 'loading' 作为值。

2. 在 HTML 解析器遇到 script 标签时，并且其不具有 async，defer 或者 type="module" 属性时，它会将那个 script 标签加入 document，然后执行那段 script。script 是同步执行的，所以 HTML 解析器会先暂停，等待 script 的下载和执行。那样的 script 可以使用 document.write() 来在 input stream 中插入文本，那些文本在解析前重新开始时，就会成为 document 的一部分。虽然这样的 script 通常会用来定义一些函数或者回调函数，不过他也可以遍历以及操纵已经生成的 document tree。

3. 在 HTML 解析器遇到拥有 async 属性的 script 标签时，他会开始下载 script（或者这个 script 是一个 async 模块，它也会被递归下载其所用 dependencies），然后继续解析 document。script 会在下载完成后开始执行，但是解析器并不会再像上面一样挂机傻等着它下载了。这种异步加载的 script 不能拥有 document.write() 方法，但是也可以访问在其之前的 document 内容，也可能可以访问更多（若下载的慢）

4. 在 document 解析完毕后，document.readyState 属性会改为 "interacitive"。

5. 任何拥有 defer 的 script（或者没有 async 属性的模块）会以其在 HTML 中出现的顺序下载和执行。Async 的 script 也可能在这时候执行（若刚下载完成）。Deferred scripts 拥有对整个 document 的完全访问，且不能使用 document.write() 方法。

6. 接下来，浏览器会在 Document 对象上传出 DOMContentLoaded 事件。这标记了同步执行的结束，也是异步执行的开始。（不过还有可能有 async script 还未被执行，即未下载完成）

7. 在这个时候，document 已经解析完成了，不过浏览器可能依然在等待剩余内容的加载，如图片。在那些内容都加载完成，以及所有的 async 代码都加载和执行完成之后，document.readySState 属性会改为 “complete”，浏览器也会在这时候在 Window 对象上传出 load 事件。

8. 在这个时间点及以后，会进入异步阶段，callback 会在异步 event 发生时被调用。

#### 程序的输入和输出

客户端的 JavaScript 也拥有输入和输出的数据，输入的数据可以是：

- document 的内容，JavaScript 可以通过 DOM API 访问
- 用户输入，在鼠标点击按钮或者文字进入 textarea 标签等事件或发生 event
- document 展示的 URL 可以通过 document.URL 来访问。若在 URL() 构造函数中传入字符串，则可以轻松的访问器域名，path 等
- HTTP 请求头中 Cookie 的内容可以通过 document.cookie 访问
- 全局 navigator 属性给予了对浏览器信息，操作系统等信息的访问

客户端的 JavaScript 通常会在必要的时候产生输出，输出可能是用 DOM API 修改 HTML document，或使用高层框架如 Vue/React 来操纵 document，或者是使用 console.log() 来展示信息。

#### 程序错误

不同于直接运行在系统上的应用，浏览器中的 JavaScript 程序并不能真正的 crash。如果 JavaScript 运行时发生异常，并且没有异常捕获机制，则错误信息会在 console 显示。但已注册的 event handlers 依然可以继续工作。

如果想要定义一个最后的异常捕获装置，可以通过改写 Window 对象的 onerror 属性来实现（传入一个新的 error handler 函数）。若之前没有别的异常捕获机制，则异常会在此处被捕捉。

window.onerror 函数会接收三个 arguments，第一个是 error 信息，第二个是 URL 表示的错误发生处，第三个是错误出现的行数。

对于没有 catch 处理的 rejected Promise，我们也可以用 window.onunhandledrejection 捕获。

#### 浏览器安全机制

因为浏览器可以在个人的设备上执行 JavaScript，所以浏览器引擎需要对以下两点做出平衡：

- 定于强大的客户端 APIs 用于创建 web apps
- 阻止恶意代码改写数据，访问私人信息等恶意行为

浏览器对于防止恶意代码的最基础的防线便是不支持某些能力。比如客户端 JavaScript 不支持写入或者删除文件/文件夹，这使得 JavaScript 无法 删除数据或者植入病毒。

下面介绍了我们应该知道的一些 JavaScript 安全限制。

##### 同源政策

同源政策限制了哪些网络内容可以被 JavaScript 交互。script 只可能读取拥有同源的内容的 windows 和 documents 属性。

document 的起源是根据 URL 的网络协议（protocol），域名(host)，端口（port）来决定的。从不同的服务器加载的 documents 拥有不同的起源；对于同一服务器上的资源，如果网络协议不同（http: 和 https:），也不同源。浏览器通常也将本地文件定义为一个单独的起源。

同源政策同样会影响 HTTP 请求，级 JavaScript 代码只能对 document 加载的起源发送 HTTP 请求，而会阻止对于其他服务器的交互（除非其拥有特定的 CORS 选项，下面会被介绍）。

如果一个大型网站包含了多个子域（subdomains），且它们之间需要通信的话，则可以对 document.domain 进设定。

第二种用于松动同源政策的技术被称为 CORS（Cross-Origin Resource Sharing）。这允许服务器去确定它们可以服务那些起源。CORS 会在 HTTP 请求头加上 Origin，会在响应头加上 Access-Control-Allow-Origin。这使得服务器可以增加一个指定可以访问其的起源的集合。

##### XSS

XSS 也被称为 CROSS-SITE SCRIPTING，它代表了一类安全问题：黑客可以通过注入 HTML 标签，或者注入 script 的形式来恶意破坏一个网站。所以说，客户端的 JavaScript 必须注意，并且防御 XSS。

在拥有动态生成的文档时，比如用户输入的数据，网站很容易暴露于 XSS 的攻击下。所以我们要先对用户的输入进行 sanitizing，用于移除所有内嵌的 HTML 标签。

比如下面的例子会使用 JavaScript 来对用户名进行问候（此问候非彼问候）：

```js
let name = new URL(document.URL).searchParams.get('name');
document.querySelector('h1').innerHTML = 'Hello' + name;
```

这两行代码会从 document URL 中提取用户名，然后注入 h1 标签中进行展示，所以这两行代码会期待这样的 URL：

```html
http://www.example.com/greet.html?name=David
```

在这种情况下，h1 会展示 'Hello David'。但如果 name 的属性改成了如下会发生什么呢？

```html
http://www.example.com/greet.html?name=%3Cimg%20src=%22x.png%22%20onload=%22alert(%27hacked%27)
%22/%3E
```

在 URL 中的参数解析完成后，它就使得一下 HTML 被注入进了 document：

```html
Hello <img src="x.png" onload="alert('hacked')"/>
```

在图片加载完成后，onload 中的 JavaScript 就会被执行。这个例子简单的展示了用户输入是如何操纵 HTML document 的，所以对于未知的输入，一定要确保对其进行消毒。

XSS 攻击之所以被称为 CROSS-SITE SCRIPTING，是因为可能会有多个网站同时介入。假设恶意网站 B 拥有一个精心制作的链接指向了网站 A。如果网站 B 上的用户被吸引然后点击了那个链接，他们便会被带到网站 A，只不过网站 A 还是会运行网站 B 上的代码，网站 B 上的代码就可以使网站 A 不已正常方式运行。更为危险的是，恶意代码甚至可以读取网站 A 存储的 cookies（比如账号信息或个人身份信息等），然后发送回网站 B。

总的来说，防止 XSS 攻击最好的办法就是移除所有不可信输入的 HTML 标签，然后再进行动态 document 的生成：

```js
name = name
  .relplace(/&/g, "&amp;")
  .replace(/</g, "&lt;")
  .replace(/>/g, "&gt;")
  .replace(/"/g, "&quot;")
  .replace(/'/g, "&#x27;")
  .replace(/\//g, "&#x2F;");
```

另一种解决 XSS 的方法是将任何不信任的内容展示在拥有 sandbox 属性的 iframe 中。

### 事件（events）

客户端的 JavaScript 程序使用了异步的，事件驱使的编程模型。在这种模型之下，浏览器会在某些使其发生时，生成一个特定的事件。因为事件可以在任何 HTML document 中的元素上生成，所以浏览器中的事件会比 Node 中的复杂许多。下面是对事件的一些介绍：

**1. 事件类型（event type）**

event type 字符串指明了事件的种类。比如说类型 'mousemove' 表示了鼠标的移动；'keydown' 表示了用户按下键盘等。因为 event type 是一个字符串，我们通常也将其称为事件名（event name）。

**2. 事件目标（event target）**

这是事件发生在的目标对象。对于所有的事件，都需要定义其类型和目标，比如如 Window（目标）的 load（类型）事件等。若想要注册一个事件处理（event handler)函数，则需要指定其事件类型和目标。只有在指定的事件类型发生在指定的目标上，才会调用该事件处理函数。
    
**3. 事件对象（event object）**

这是一个于和一个事件以及其内容关联的对象，事件对象以 argument 的形式传入事件处理函数。事件对象拥有 tyep 和 target 属性用于指定类型和目标。事件类型又会为其事件对象定义一系列的属性。比如鼠标移动的事件对象会持有鼠标的坐标。而某些事件类型则不会未事件对象定义属性，使得事件对象只拥有 type 和 target 这两个属性。
    
**4. 事件传播（event propagation）**

这是浏览器决定由哪些对象来出发事件处理函数的过程。对于那种指定了单一对象的事件，比如 Window 对象的 'load' 事件，则不需要传播。但是对于某些元素的事件则会在 document tree 向上传播。比如用户将鼠标移动到超链接上，mousemove 事件会现在 \<a> 元素上触发，然后会在包含该链接的 \<p> 元素上触发，然后是包含 \<p> 的 \<div> 上触发等等，一直传递到 Document 对象。事件触发了某个定义的事件处理函数之后，便会停止传播。这是因为事件处理函数会调用事件对象上的一个方法使其停止传播。
	
某事件会拥有默认的与其绑定的 action，比如在点击超链接时会默认使浏览器跟随该链接并且加载新的页面。我们可以在事件处理函数中阻止该默认 action。

#### 事件分类

客户端 JavaScript 支持许许多多的事件类型，这里会对事件类型进行一下分类：

**基于设备的输入事件（Device-dependent input events）**

这一类事件会会直接和输入设备绑定，如鼠标或者键盘，比如：mousedown，mousemove，mouseup，touchstart，touchmove，touchend，keydown，keyup 等。

**不基于设备的输入事件（Device-independent input events）**

这一类输入时间则不会和设备绑定，比如 click 事件，虽然通常使用过鼠标点击，不过也可以通过键盘或者点击触摸屏来完成。比如：pointerdown，pointermove，pointerup 就不会和设备绑定，对于触摸屏，触摸板等也能生效。

**用户界面事件（User interface events）**

UI 事件是高层事件，通常会在 HTML 表单元素出现，包括了 focus，change，submit 等事件。

**状态改变事件（State-change events）**

某些事件则不会在用户交互式触发，而是在网络或者浏览器的状态改变时触发。比如前面提到的 load 和 DOMContentLoaded 事件，会分别在 Window 和 Document 对象上触发。浏览器会在网络连接改变时触发 online 和 offline 事件。浏览器历史管理系统会在按下浏览器 back 按钮后触发 popstate 事件等。

**API 指定事件（API-specific events）**

许多 HTML 中的 web APIs 也拥有其事件。如果 HTML 中的 video 和 audio 元素定义了一系列与它们关联的事件，如 waiting，playing，seeking，volumechange 等等。这使得我们可以自定义媒体的播放。在 Promise 之前，异步的 web APIs 都是事件驱使的，如 IndexedDB 和 XMLHttpRequest API 等。

#### 注册事件处理器

这一共有两种基础方法可以用于注册事件处理器（event handlers）。第一种是为 document 元素或其对应的对象上增加事件处理属性。第二种是使用 addEventListener() 方法来传入处理器。

##### 设置事件处理属性（JavaScript 中）

注册事件处理器最简单方法就是在目标事件上设置一个事件处理函数作为属性。事件处理器属性由前缀 on 加上 事件类型来表示，如 onclick，onmousedown，onload 等（注意全小写）：

```js
window.onload = function() {
  let form = document.querySelector('form#form1');
  form.onsubmit = function(event) {
    console.log(event);
  }
}
```

上面的例子就为 window 注册了 onload 时的事件处理函数，为 form 注册了 onsubmit 时的事件处理函数。
不过通常情况下，addEventListener() 会是一个更好的选择，一因为他不会覆写已注册的处理器。

##### 设置事件处理属性（HTML 中）

我们也可以在 HTML 直接为元素添加事件处理属性，不过现在一般不会这么做了：

```html
<button onclick="console.log('Thank you');">Please Click Here</button>
```

这样注册的事件处理函数函数会被浏览器转换成类似于以下的形式：

```js
function(event) {
  with(document) {
    with(this.form) {
      with(this) {
        console.log(event);
      }
    }
  }
}
```

因为 with 语句已经在严格模式下被遗弃，所以这样定义的事件处理器会出现不可预知的行为，所以说尽量不要在 HTML 中直接写入事件处理函数。

##### ADDEVENTLISTENER()

所有可能成为事件目标的对象（Window，Document 对象以及所有 document 元素）都拥有名为 addEventListener() 的方法。我们可以使用它来注册一个事件处理函数。addEventListener() 接收三个 arguments，第一个 argument 是不包含 on 前缀的，用于表示事件的字符串，如 'click', 'load' 等，第二个 argument 是回调函数，第三个 argument 则是可选的，会在下面被解释。


比如下面的对象为 button 注册了两个 click 事件处理器：

```html
<button id="mybutton">Click me</button>
<script>
  let b = document.querySelector("#mybutton");
  b.onclick = function() { console.log("Thanks for clicking me!"); };
  b.addEventListener("click", () => { console.log("Thanks again!"); });
</script>
```

使用 addEventListener() 新增的处理器不会对已注册的事件处理器造成影响，而是会一起被调用。所以我们可以使用 addEventListener() 对一个目标注册多个事件处理器，不过注册同样 argument 的 addEventListener() 超过一次是无用的，一样的 handler 只会被注册一次。

addEventListener() 方法是和 removeEventListener() 方法配对的，removeEventListener() 接收两个和 addEventListener() 同样的 arguments（以及可选的第三个），只不过会移除指定的事件处理函数。

addEventListener() 接收的第三个 argument 可以是一个布尔值或者一个对象，如果传入 true，则回调函数是以捕获事件处理器的形式注册的（会在等下介绍），而且会在事件触发的另一个阶段调用。想要删除同一个事件处理函数，则必须在 removeEventListener() 中传入同样的第三个 argument。

我们除了传入布尔值，也可以传入详细的 Options 对象：

```js
document.addEventListener('click', handleClick, {
  capture: true,
  once: true,
  passive: true
});
```

如果 Options 对象的 capture 属性为 true，则事件处理器会被注册成为捕获处理器，如果是 false 或被忽略，则是普通的事件处理器。

如果 Options 对象的 once 属性为 true，事件监听器会在触发一次后被移除，若为 false 或被忽略，则不会被自动移除。

如果 Options 对象的 passive 属性为 true，则表示事件处理函数不会调用 preventDefault() 方法，这对手机的触摸事件比较有用。如果 touchmove 事件可以 prevent 浏览器滚动的 default action，浏览器就无法实现 smooth 滚动。这个 passive 属性使得我们可以阻止这种可能的情况。正因为 smooth 滚动对于 UX 十分重要，所以浏览器的 touchmove 和 mousewheel 默认传入 passive 为 true。所以在需要对这些事件增加使用 preventDefault() 的函数，则需要显式的写下 passive 为 false。

##### 事件处理函数调用

在注册事件处理函数函数之后，浏览器就会在事件发生时自动调用它。这一节会深入解释背后发生了什么。

事件处理函数的 argument

没有给事件处理函数会接收一个事件对象作为其单一的 argument，这个事件对象包括以下属性：

- type：事件类型
- target：目标对象
- currentTarget：对于会传播的对象，这个属性是当前事件处理函数注册的对象
- timeStamp：毫秒表示的事件发生的时间戳
- isTrusted：true 代表浏览器生成的事件，false 代表 JavaScript 代码生成的事件
- 其他根据事件类型增肌的属性

##### 事件处理函数上下文（context）

通过写入属性的方式注册的事件处理函数看上去就像在目标对象上新增了一个方法一样：

```js
target.onclick = function() { /* handler code */ };
```

所以说，事件处理函数是以目标对象的方法的形式来调用的。这意味着在函数体中 this 指向了注册事件处理函数的对象，也就是目标对象。

对于 addEventListener() 注册的事件处理函数，其中的 this 的值也是相同的，不过对于箭头函数不管用，箭头函数的 this 依然指向其被定义的上下文。

##### 事件处理函数的返回值

理论上说，现在的 JavaScript 中的事件处理函数不应该返回任何东西，但是对于老旧的代码，可能会有返回值。这个返回值通常是一个用于表示浏览器不需要执行默认 action 的信号。就比如 submit 按钮的 onclick 返回 false，则浏览器不会继续默认的 submit 表单。

现在的阻止默认行为的方法是调用 preventDefault() 方法。

#### 事件传播（Event Propagation）

对于 Window 对象或者其他独立对象的事件，浏览器对其的处理就是简单地调用其处理函数。但是如果目标对象是 Document 对象，情况就会变得复杂起来。

在调用完目标对象上的事件处理函数之后，大多数事件（focus，blur，scroll 事件除外）会沿着 DOM tree 一路冒泡（bubble），所以目标对象的 parent 的事件处理函数会被调用，然后是 grandparent 的事件处理函数，以此类推一直到 Document 对象，然后是 Window 对象。load 事件会在 document 元素的 load 事件也会冒泡，不过会在到了 Document 对象时停止，不冒泡到 Window 对象（Window 对象的 load 事件处理函数只会在整个 document 加载完成时触发）。

事件冒泡是事件传播的第三个阶段，而调用目标对象上的事件处理函数是第二个阶段，而第一个阶段则是在调用事件处理函数之前发生的，被称为捕获（capturing）阶段。还记得 addEventListener() 会接受第三个可选的 argument，如果 argument 为 true 或者 {capture: true}，则事件处理函数会已捕获事件处理函数的形式被注册，然后会在事件传播的第一个阶段被调用。

这第一个捕获阶段就如同相反的冒泡阶段，捕获处理函数会从 Window 对象先开始调用，然后是 Document 对象，然后是 body 对象，然后一路沿着 DOM tree 传播直到目标对象的 parent。但是要注意的是，目标对象上的捕获函数不会被调用。

这一捕获机制是我们可以在事件传递到目标对象之前捕获到它，可以用于 debug 或者用于事件 cancel 技术，用于过滤掉一些事件。这个机制可以在 drag 事件中用到，因为 drag 时的鼠标移动事件只需要被目标对象处理。

#### 事件取消

浏览器会在许多用户事件时响应，即使那些事件没有被我们的代码所用到。比如点击超链接时，浏览器会自动跳转到新的链接。

取消与事件关联的默认 action是唯一一种事件取消技术。我们也可以通过调用事件对象上的 stopPropagation() 方法来取消事件的传播。对于同一目标对象上的事件处理函数，它们依然会被调用，
但是对于其他对象上的处理函数则不会再被调用。

stopPropagation() 方法会在捕获阶段以及冒泡阶段都可以生效，调用时要记得以 e.stopPropagation() 的形式，因为该方法在事件对象上。

还有一个类似的方法 stopImmediatePropagation()，不过它会阻止目标对象上剩余的事件处理函数的执行。

#### 生成自定义事件

客户端 JavaScript 事件的 API 可以使我们定义并且生成自定义事件。比如我们希望定期执行一次网络请求来确定页面是否要更新，其发生时我们希望用户知道我们正在加载，而非面对未响应的页面，我们可以在这时生成一个事件用于告知用户。

如果一个 JavaScript 对象拥有 addEventListener() 方法，则会说明这是一个可以作为事件对象的对象。它也拥有一个 dispatchEvent() 方法。我们可以通过 CustomEvent() 构造函数来创建一个自定义事件，然后传入 dispatchEvent()。CustomEvent() 的第一个 argument 是指定事件类型的字符串，第二个 argument 是定义事件对象属性的对象，我们可以将该对象的 detail 属性设置为字符串，对象，或者其他值，他会用于表示事件的内容：


```js
// 生成一个自定义事件，告知 UI 现在正在加载
document.dispatchEvent(new CustomEvent('loading',{detail: true;}));

// 发送网络请求
fetch(url)
  .then(handleNetworkResponse)
  .catch(handleNetworkError)
  .finally(() => {
    // 加载完成后再生成一个自定义事件，告知 UI 加载完成
    document.dispatchEvent(new CustomEvent("busy", {detail: false }));
});

// 程序的其他地方拥有一个事件处理函数的注册
document.addEventListener('loading',(e) => {
  if (e.detail) {
    showLoading();
  } else {
    hideLoading();
  }
});
```

### Scripting Documents

JavaScript 诞生的目的就是为了将静态的页面转换成 

每一个 Window 对象都拥有 document 属性，它指向了 Document 对象。Document 对象代表了网页中的内容。Document 对象并不会单独存在，而它是 DOM 的中心对象，用于表示和操纵 document 内容。

DOM 已经在前面介绍过了，这一章会解释一下它包含的 API：

- 如何查询以及选择 document 中的元素
- 如何遍历 document，并寻找指定元素的 ancestors/siblings/descendants 元素
- 如何查询以及改变 document 元素属性
- 如何查询以及改变 document 内容
- 如何通过增删改节点来改变 document tree 结构

#### 选择 Document 元素

客户端 JavaScript 中全局的 document 属性指向了 Document 对象，其中的 head 和 body 属性又分别指向了 head 和 body 标签所对应的 Element 对象。

##### 通过 CSS 选择器选择元素

CSS 拥有一个强大的句法，被称为选择器（selectors），可以用于描述单个或多个元素。DOM 中的方法 querySelector() 和 querySelectorAll() 可以使我们通过 CSS 选择器来查找元素。

CSS 选择器可以通过标签，id（#），class（.）来描述元素：

```scss
div // 任何 div 标签的元素 
#abc // 拥有 id='abc' 的元素 
.abc // 拥有 class='abc' 的元素 
```

或者可以通过更为指定的属性来选择：

```scss
p[lang='fr'] // 任何法语编写的 p 标签（<p lang="fr">）
*[name='abc'] // 任何拥有 name='x' 的元素
```

也可以是更为复杂的组合，或者指定了 document 中的位置：

```scss
span.fatal.error // 任何拥有 fatal 和 error 同时作为 class  的 span 标签的元素
span[lang='fr'].warning // 任何拥有 warning 作为 class，且是法语的 span 标签的元素

h1, h2 // 使用逗号分割代表符合其一即可

#log span // id='log' 元素下所有 span 标签的 descendant（所有后裔）元素
#log>span // id='log' 元素下所有 span 标签的 child（仅子元素，孙元素不行） 元素
body>h1:first-child // body 中第一个 h1 的子元素
img + p.caption // 在 img 之后紧跟的 拥有 class='caption' 的 p
h2 ~ p // 任何拥有 h2 作为 sibling（兄妹）的 p 元素
```

querySelector() 方法就可以接收用字符串表示的 CSS 选择器作为 argument，在找到第一个匹配元素时，返回它，或者在找不到任何匹配时返回 null；querySelectorAll() 则会返回所有匹配值：

``` js
let h1 = document.querySelector("h1"); // 返回第一个 h1 
let h2 = document.querySelectorAll("h2"); // 返回所有 h2
```

querySelectorAll() 的返回值不是包含 Element 对象的数组，而是一个类数组对象，被称为 NodeList。NodeList 对象也拥有 length 属性，也拥有和数组一样的索引，所以可以使用 for of 循环等遍历，我们可以通过 Array.from() 方法来将其转换成一个数组。

这两个方法在 Element 类和 Document 类中都被实现了。对于 Document 使用，会在整个 Document 中查找，而对 Element 使用，则只会查找它的 descendants

对于 CSS 中 ::first-line 等只截取 node 中的一部分的选择器无法在这两个方法中使用。许多浏览器页不允许 :link 和 :visited 选择器，因为可能会暴露用户隐私。

另一种 CSS 选择器的方法时 closest()，它只在 Element 类中被定义。它也会接收一个 CSS 选择器作为 argument，如果有匹配元素，返回它；若没有匹配元素，它会返回最近的匹配的 ancestor（祖先)，若还是找不到，则返回 null。

##### 其他的元素选择方法

除了通过 CSS 选择器来选择之外，还可以通过以下方法来选择：

```js
let a1 = document.getElementById("a1"); // 通过 id 属性选择
let a2 = document.getElementsByName("a2"); // 通过 name 属性选择
let a3 = document.getElementsByClassName("a3"); // 通过 class 属性选择
let h2 = document.getElementsByTagName("h2"); // 通过标签名选择
```

getElementById() 会返回一个 Element 对象，而剩余的方法则和 querySelectorAll() 一样，返回一个 NodeList。

##### 预设元素选择

出于历史原因，Document 类也定义了一些属性来直接访问特定的 nodes。比如 images，forms，links 属性会选择所有 img，form，a 标签的元素。这些属性指向了 HTMLCollection 对象，它和 NodeList 对象十分相似，不过 HTMLCollection 也可以通过 name 或者 id 查找：

```js
document.links[0]; // 第一个超链接
document.links.first // id 为 first 的超链接
```

还有一个过时的 API：document.all，它可以用 HTMLCollection 来返回 document 中的所有元素。不过目前已被遗弃，不应该再被使用。

#### Document 的结构和遍历

在选择了一个 Element 之后，我们可以调用其 traversal API 来遍历 document tree 中的 Element 对象，并且忽略 Text 节点:

- **parentNode：** Element 对象上的该属性指向了该元素的 parent 对象
- **children：** 该属性为包含了 Element 对象上的是所有子元素的 NodeList
- **childElementCount：** 该属性为子元素数量，等同于 children.length
- **firstElementChild：** 该属性指向了第一个子元素
- **lastElementChild：** 该属性指向了最后一个子元素
- **nextElementSibling：** 该属性指向了下一个兄妹元素
- **previousElementSibling：** 该属性指向了上一个兄妹元素

深度优先遍历 document：

```js
// 第一种
function dfs(element) {
  // 可以在这做些什么
  // ...
  for(let child of e.childern) {
    dfs(child); // 递归遍历
  }
}

// 第二种
function dfs(element) {
  // 可以在这做些什么
  // ...
  let child = element.firstElementChild; // 使用链表风格
  while(child !== null) {
    dfs(child); // 递归遍历
    child = child.nextElementSibling;
  }
}
```

而如果我们不想忽略 Text 节点时，我们可以使用以下属性，所有的 Node 对象都拥有它们：

- **parentNode：** 该属性指向了该元素的 parent 节点对象
- **childNodes：** 该属性为包含了该对象上的是所有子节点对象的 NodeList
- **firstChild：** 该属性指向了第一个子节点
- **lastChild：** 该属性指向了最后一个子节点
- **nextSibling：** 该属性指向了下一个兄妹节点
- **nextSibling：** 该属性指向了上一个兄妹节点
- **nodeType：** 表示节点种类的数字，9 为 Document 节点，1 为 Element 节点，3 为 Text 节点，8 为 Comment 节点
- **nodeValue：** Text 和 Comment 节点的文字内容
- **nodeName：** HTML 标签名，转换成大写

#### 元素属性

HTML 的 element 都拥有一个标签名，以及一系列表示属性的 name/value pairs。我们可以使用 getAttribute(), setAttribute(), hasAttribute(), removeAttribute() 来取得，改变，测试，移除特定的属性。但其实 HTMLElement 对象上拥有对应的属性作为属性名，所以用起来会更方便。

##### 以 ELement 对象属性表示 HTML 属性

Element 对象表示的 HTML 元素通常定义了用于读写的属性。所有的 Element 对象都定义了 id，title，lang，dir 以及事件处理函数。对于某些特定类型的 Element 还定义附加的属性，如 img 的 src 属性。

HTML 的属性不用区分大小写，但是 JavaScript 属性需要，使用 camelCase（驼峰命名法）来命名。某些 HTML 属性名是 JavaScript 中的保留字，对于他们的解决方案通常是加上 html 前缀。比如 HTML 的 for 属性，进入 JavaScript 属性就成为了 htmlFor。class 也是一个保留字，但是它比较特殊，所以改名为了 className。

虽然 Element 对象上的属性定义了读写方法，但没有定义移除的方法，所以再移除属性时调用 removeAttribute() 来实现。

##### class 属性

对于 HTML 元素来说，class 属性十分重要，它的值时一列通过空格分割的 CSS 类。JavaScript 中使用 classsName 属性表示。我们可以直接更改 className，不过更多的时候，我们只需要修改其中一个类。

处于这个原因，Element 对象也定义了 classList 属性，是我们可以将 HTML 的 class 属性当做一个 list 来处理。classList 的值是一个类数组对象，可以被迭代，也可以使用以下方法：

```js
let a = document.querySelector('#a1');
a.classList.remove(); // 删除指定类
a.classList.add(); // 增加指定类
a.classList.contains(); // 是否包含指定类
a.classList.toggle(); // 若该类存在，删除它；若不存在，添加它
```

##### dataset 属性

某些时候我们会需要为 HTML 元素添加更多的数据，然后让 JavaScript 来操纵或者选取它们。在 HTML 中，任何以 data- 前缀开头的属性都是合理的数据，它们并不会改变 HTML 元素展示的样子，而是一种标准的增加附加数据的方法。

在 DOM 中，Element 对象拥有一个 dataset 属性，它指向了一个包含对应的 data- 属性作为属性的对象，不过不再拥有前缀。比如 dataset.x 就指向了 HTML 标签中的 data-x 属性。对应时还会从连字符转到 camelCase，即 data-my-age 会变成 dataset.myAge：

```html
<h2 id="age" data-my-age="22">age</h2>
```

对应

```js
document.querySelector("#age").dataset.myAge; // '22'
```

#### 元素内容

假设 HTML 中有一个 p 元素：

```html
<p>This is a <i>simple</i> example</p>
```

那么它的内容是什么呢？

- 是 'This is a \<i>simple\</i> document' 嘛？
- 又或是 'This is a simple document' 呢？

其实它们都可以被称之为答案，并且拥有特定的用处

##### 以 HTML 作为元素内容

读取 Element 的 innerHTML 属性时，返回的就会是 'This is a \<i>simple\</i> document'，即包含了 HTML 标签的字符串。我们对其进行修改时，也可以加入 HTML 标签，而它们也会被正常的解析。

因为浏览器十分擅长解析 HTML，所以通常改变 innerHTML 是比较高效的，但如果对 innerHTML 使用 += 操作符就不再高效了，因为需要格外的序列化步骤，将 HTML 元素转换成字符串，然后再将新的字符串解析回去。

还有一点需要注意，不要让用户输入的数据进入 document，不然他可能再你的程序中恶意插入它的代码。

outerHTML 和 innerHTML 很像，不过会包含元素本身：'\<p>This is a \<i>simple\</i> example\</p>'。一个相关的方法是 insertAdjacentHTML()，它使我们可以再 HTML 中为指定元素的相邻位置插入字符串标识的 markup。markup 会作为第二个 argument 表示，而第一个 argument 定义了 adjacent 的位置，可以是 'beforebegin', 'afterbegin', 'beforeend', 'afterend' 中的一个：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ea7bb9607554ad1b3f67f325fbbfd68~tplv-k3u1fbpfcp-watermark.image)

##### 以纯文本作为元素内容

在我们只需要元素的内容的文字部分时，或者需要插入纯文本时（没有 markup 的标签等），我们可以使用 textContent 属性来实现，比如前面的例子就会返回 'This is a simple document'。

textContent 属性是在 Node 类上定义的，所以其子类 Element node 和 Text node 都拥有该属性。

#### 创建，插入，删除节点

除去查询和修改之外，我们也可以直接创建，插入或删除节点，然后直接改变 document 的结构。Document 类中顶一个用于创建 Element 对象的方法，Element 对象和 Text 对象又拥有了插入，删除，以及替换节点的方法。

想要创建一个新的 Element 对象并修改它十分的简单：

```js
let p = document.createElement('p'); // 创建新的 p 元素
let em = document.createElement('em'); // 创建新的 em 元素
em.append('world'); // 为 em 从右增加字符串
p.append(em, '!'); // 将 em 加入 p 中
p.prepend('Hello '); // 为 p 从左增加字符串
p.innertHTML // "Hello <em>World</em>!"
```

append() 和 preend() 方法接收任意数量的 arguments，且可以是字符串或者 Node 对象，字符串会自动转换成 Text 节点。（或使用 document.createTextNode() 来显式创建，但是没必要）

如果要在 Element 或者 Text 节点的中间插入，则需要先得到对于某个 sibling 节点的引用，然后调用 before() 或 after() 方法：

```js
// 找到拥有 class='greetings' 的 h2 元素
let greetings = document.querySelector("h2.greetings"); 
// 在它后面添加 p 和一个 horizontal rule（hr）
greetings.after(p, document.createElement("hr"));
```

如果想要复制一个节点，可以使用 cloneNode() 方法，若传入 true 作为 argument，则会将其中所有内容复制；若传入 false 或不传 argument，则只会复制标签和属性。若想要移除一个节点，我们可以使用 document 的 remove() 方法，或者 replaceWith() 用于替换：

```js
let p2 = p.cloneNode(true); // 复制 p
greetings.replaceWith(p2); // 删除 greetings，并用 p2 替代
p2.remove(); // 删除 p2
```

### Scripting CSS

除了操纵 HTML document 的内容和结构之外，JavaScript 也可以操纵 CSS 来改变其展示风格。以下属性是最常在 JavaScript 中被用到的 CSS 风格：

- **display 属性：** 通过将其设置为 none 来隐藏元素
- **position 属性：** 通过将其设置为 absolute，relative 或者 fixed，然后修改元素展示位置
- **transform 属性：** 用于移动，旋转，拉伸元素
- **transition 属性：** 为元素添加动画，这些动画会通过浏览器自动处理，而不需要额外的 JavaScript

#### CSS 类

最基础的操纵 CSS 的用法就是从 HTML 的 class 标签新增或删除 CSS 类名。使用 classList 属性来操作会很简单。比如我们可以定义一个 CSS 类：

```scss
.hidden { // 该类元素会被隐藏
  display: none;
}
```

然后通过 JavaScript 修改 class 名来隐藏/显示元素：

```js
document.querySelector("#x").classList.add("hidden"); // 隐藏 id 为 x 的元素
document.querySelector("#x").classList.remove("hidden"); // 展示它
```

#### Inline Styles

我们可以通过 JavaScript 来修改 HTML 元素的 style 属性来改变它的风格。DOM 为每一个 Element 对象定义了一个 style 属性，指向了对应的 HTML style 属性。style 和大多属性不同，他不是一个字符串，而是一个 CSSStyleDeclaration 对象：一个被解析的，拥有以字符串表示的 CSS 风格的对象。比如我们可以这样定义一个修改元素位置的函数：

```js
function displayAt(element, x, y) {
  element.style.display = 'block';
  element.style.position = 'absolute';
  element.style.left = `${x}px`;
  element.style.top = `${y}px`;
}
```

CSSStyleDeclaration 的命名风格和 HTML 中的不同，因为连字符在 JavaScript 会被当成减号，所以会将 kebab-case 转换成 camelCase，比如 border-left-width 会变成 borderLeftWidth。

在和 CSSStyleDeclaration 打交道时，要记住所有的属性值都是字符串：

```scss
.e { // CSS 中不用字符串来表示
  display: block;
  font-family: sans-serif; 
  background-color: #ffffff;
  margin-left: 300px;
}
```

对应：

```js
// JavaScript 中则需要是字符串
e.style.display = "block"; // 注意分号在字符串外，它们是 JavaScript 的分号
e.style.fontFamily = "sans-serif"; // 我们不需要为 CSS 传入分号
e.style.backgroundColor = "#ffffff";
e.style.marginLeft = "300px"; // 注意是字符串而非数字，也要注意单位
```

我们也可以使用 getAttribute 和 setAttribute，或者 cssText 属性来查询 inline style，而且会简单一些：

```js
// 将 e 的 inline styles 复制到 f 上，下面两个方法都可以
f.setAttribute("style", e.getAttribute("style"));
f.style.cssText = e.style.cssText;
```
在通过 style 属性查询元素的 CSS 时，我们只能得到其 inline style，而无法取得外部 CSS 的风格。

#### Computed Styles

一个元素的 Computed Styles 就是其所有风格的属性和值的集合（包括 inline 的，浏览器定义的，CSS 定义的）：这也是展示元素时用到的属性。Computed Styles 也是通过 CSSStyleDeclaration 对象来代表的，不过和 inline style 不同，它是一个只读对象，我们不能修改它。

我们可以通用 Window 对象上的 getComputedStyle() 方法来取得元素的风格，第一个 argument 是需要查询的元素，第二个可选的 argument 是用于指定 CSS 中的伪代码（类似 ::before 等）：

```js
let title = document.querySelector("#section1title");
let styles = window.getComputedStyle(title);
let beforeStyles = window.getComputedStyle(title, "::before");
```
getComputedStyle() 方法的返回值是一个 CSSStyleDeclaration，它代表了指定元素上所有的风格。虽然都是 CSSStyleDeclaration 对象，但是 inline style 表示的和 computed style 表示的还是有许多区别的：

- computed style 的属性是只读的
- computed style 的属性是绝对的：所以百分比等非绝对值都会转换成绝对值，任何需要 size 的属性（如 font-size 和 margin size）都会转换成 pixels（px 后缀）；颜色则会被转换成 rgb() 或者 rgba() 形式
- computed style 不包含简写属性：比如 margin 不被包含，只会包含其中的 marginLeft，marginTop 等
- computed style 的 cssText 属性是 undefined

通常来说，getComputedStyle() 返回的 CSSStyleDeclaration 对象会拥有更多的信息。但是某些时候它包含的信息是很狡猾的，比如 top 和 left 的属性通常会返回 auto 作为值，虽然一点都不 make sense，不过这是一个合理的 CSS 值。

虽然 CSS 可以用于精确的指定元素的 position 和 size，但是通过查询 computed style 并不是最完美的查询 position 和 size 的方法。等下会介绍更好的方法。

#### Scripting Stylesheets

除去操纵 inline style 之外，JavaScript 也可以操纵 stylesheets（即以 sstyle 标签的元素，或 rel='stylesheet' 的 link）。因为它们都是 HTML 标签，所以可以通过 doncument.querySelector() 等方法查找。style 和 link 元素对应的 Element 对象拥有 disabled 属性，我们可以用它来 disable 整个 stylesheet：

```js
function switchTheme () { // 切换主题
  let light = document.querySelector('#light-theme');
  let dark = document.querySelector('#-theme');
  if (dark.disabled) {
    light.disable = true;
    dark.disable = false;
  } else { 
    light.disable = false;
    dark.disable = true;
  }
}
```

或者我们可以为 DOM 添加 link 来加入 stylesheet：

```js
let link = document.createElement('link'); // 创建 link 对象
link.id = "style"; // 设置 id
link.rel = "stylesheet"; // 设置 rel
link.href = `style.css`; // 设置 href
document.head.append(link); // 加入 document 中
```

#### CSS 动画和事件

假设这有两个 CSS 类：

```scss
.transparent { opacity: 0; }
.fade { transition: opacity .5s ease-in }
```

第一个会使元素完全透明化，而第二个为其增加了 fade 效果。假设我们已经有了一个拥有 fade 类的元素，我们在 JavaScript 中为其增加 transparent 类时，它会自动触发动画；在删除时也会触发。我们不需要在 JavaScript 中额外的定义什么，它们都是纯粹的 CSS 特效，JavaScript 仅仅使用来触发它们的。

JavaScript 也可以被用来监听 CSS transition 的进度，因为浏览器会在 transition 开始和结束时触发事件。transitionrun 事件会在 transition 效果触发时触发，它会在视觉效果变化前触发（如果设有 trainsition-delay）；transitionstart 事件会在视觉效果开始时触发；transitionend 会在动画结束时触发。传入该类事件的处理函数的 argument 会是 TransitionEvent 对象，拥有额外的 propertyName 属性，指定了用于动画的 CSS 属性，以及 elapsedTime 属性指定了从 transitionstart 到 transitionend 花了多少时间。

除去 transition，CSS 还支持 animation。它包含比如 animation-name 和 animation-duration 属性，和一个指定的 @keyframes rule 用于指定其细节。我们也可以通过 JavaScript 增删 CSS 类来触发它。