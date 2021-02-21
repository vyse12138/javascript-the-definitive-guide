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