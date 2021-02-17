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