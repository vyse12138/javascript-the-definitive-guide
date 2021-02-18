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
