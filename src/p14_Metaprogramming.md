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