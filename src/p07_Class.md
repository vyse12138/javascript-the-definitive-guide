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