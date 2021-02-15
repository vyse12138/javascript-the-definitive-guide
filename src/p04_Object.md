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