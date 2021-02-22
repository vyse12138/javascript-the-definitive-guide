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