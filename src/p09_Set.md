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
