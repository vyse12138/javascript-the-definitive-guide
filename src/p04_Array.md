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
