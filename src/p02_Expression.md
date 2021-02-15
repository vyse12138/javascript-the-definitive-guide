---
# 贡献主题：https://github.com/xitu/juejin-markdown-themes
theme: juejin
highlight: vs2015
---

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