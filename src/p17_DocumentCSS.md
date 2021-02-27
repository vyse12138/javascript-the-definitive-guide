### Scripting Documents

JavaScript 诞生的目的就是为了将静态的页面转换成 

每一个 Window 对象都拥有 document 属性，它指向了 Document 对象。Document 对象代表了网页中的内容。Document 对象并不会单独存在，而它是 DOM 的中心对象，用于表示和操纵 document 内容。

DOM 已经在前面介绍过了，这一章会解释一下它包含的 API：

- 如何查询以及选择 document 中的元素
- 如何遍历 document，并寻找指定元素的 ancestors/siblings/descendants 元素
- 如何查询以及改变 document 元素属性
- 如何查询以及改变 document 内容
- 如何通过增删改节点来改变 document tree 结构

#### 选择 Document 元素

客户端 JavaScript 中全局的 document 属性指向了 Document 对象，其中的 head 和 body 属性又分别指向了 head 和 body 标签所对应的 Element 对象。

##### 通过 CSS 选择器选择元素

CSS 拥有一个强大的句法，被称为选择器（selectors），可以用于描述单个或多个元素。DOM 中的方法 querySelector() 和 querySelectorAll() 可以使我们通过 CSS 选择器来查找元素。

CSS 选择器可以通过标签，id（#），class（.）来描述元素：

```scss
div // 任何 div 标签的元素 
#abc // 拥有 id='abc' 的元素 
.abc // 拥有 class='abc' 的元素 
```

或者可以通过更为指定的属性来选择：

```scss
p[lang='fr'] // 任何法语编写的 p 标签（<p lang="fr">）
*[name='abc'] // 任何拥有 name='x' 的元素
```

也可以是更为复杂的组合，或者指定了 document 中的位置：

```scss
span.fatal.error // 任何拥有 fatal 和 error 同时作为 class  的 span 标签的元素
span[lang='fr'].warning // 任何拥有 warning 作为 class，且是法语的 span 标签的元素

h1, h2 // 使用逗号分割代表符合其一即可

#log span // id='log' 元素下所有 span 标签的 descendant（所有后裔）元素
#log>span // id='log' 元素下所有 span 标签的 child（仅子元素，孙元素不行） 元素
body>h1:first-child // body 中第一个 h1 的子元素
img + p.caption // 在 img 之后紧跟的 拥有 class='caption' 的 p
h2 ~ p // 任何拥有 h2 作为 sibling（兄妹）的 p 元素
```

querySelector() 方法就可以接收用字符串表示的 CSS 选择器作为 argument，在找到第一个匹配元素时，返回它，或者在找不到任何匹配时返回 null；querySelectorAll() 则会返回所有匹配值：

``` js
let h1 = document.querySelector("h1"); // 返回第一个 h1 
let h2 = document.querySelectorAll("h2"); // 返回所有 h2
```

querySelectorAll() 的返回值不是包含 Element 对象的数组，而是一个类数组对象，被称为 NodeList。NodeList 对象也拥有 length 属性，也拥有和数组一样的索引，所以可以使用 for of 循环等遍历，我们可以通过 Array.from() 方法来将其转换成一个数组。

这两个方法在 Element 类和 Document 类中都被实现了。对于 Document 使用，会在整个 Document 中查找，而对 Element 使用，则只会查找它的 descendants

对于 CSS 中 ::first-line 等只截取 node 中的一部分的选择器无法在这两个方法中使用。许多浏览器页不允许 :link 和 :visited 选择器，因为可能会暴露用户隐私。

另一种 CSS 选择器的方法时 closest()，它只在 Element 类中被定义。它也会接收一个 CSS 选择器作为 argument，如果有匹配元素，返回它；若没有匹配元素，它会返回最近的匹配的 ancestor（祖先)，若还是找不到，则返回 null。

##### 其他的元素选择方法

除了通过 CSS 选择器来选择之外，还可以通过以下方法来选择：

```js
let a1 = document.getElementById("a1"); // 通过 id 属性选择
let a2 = document.getElementsByName("a2"); // 通过 name 属性选择
let a3 = document.getElementsByClassName("a3"); // 通过 class 属性选择
let h2 = document.getElementsByTagName("h2"); // 通过标签名选择
```

getElementById() 会返回一个 Element 对象，而剩余的方法则和 querySelectorAll() 一样，返回一个 NodeList。

##### 预设元素选择

出于历史原因，Document 类也定义了一些属性来直接访问特定的 nodes。比如 images，forms，links 属性会选择所有 img，form，a 标签的元素。这些属性指向了 HTMLCollection 对象，它和 NodeList 对象十分相似，不过 HTMLCollection 也可以通过 name 或者 id 查找：

```js
document.links[0]; // 第一个超链接
document.links.first // id 为 first 的超链接
```

还有一个过时的 API：document.all，它可以用 HTMLCollection 来返回 document 中的所有元素。不过目前已被遗弃，不应该再被使用。

#### Document 的结构和遍历

在选择了一个 Element 之后，我们可以调用其 traversal API 来遍历 document tree 中的 Element 对象，并且忽略 Text 节点:

- **parentNode：** Element 对象上的该属性指向了该元素的 parent 对象
- **children：** 该属性为包含了 Element 对象上的是所有子元素的 NodeList
- **childElementCount：** 该属性为子元素数量，等同于 children.length
- **firstElementChild：** 该属性指向了第一个子元素
- **lastElementChild：** 该属性指向了最后一个子元素
- **nextElementSibling：** 该属性指向了下一个兄妹元素
- **previousElementSibling：** 该属性指向了上一个兄妹元素

深度优先遍历 document：

```js
// 第一种
function dfs(element) {
  // 可以在这做些什么
  // ...
  for(let child of e.childern) {
    dfs(child); // 递归遍历
  }
}

// 第二种
function dfs(element) {
  // 可以在这做些什么
  // ...
  let child = element.firstElementChild; // 使用链表风格
  while(child !== null) {
    dfs(child); // 递归遍历
    child = child.nextElementSibling;
  }
}
```

而如果我们不想忽略 Text 节点时，我们可以使用以下属性，所有的 Node 对象都拥有它们：

- **parentNode：** 该属性指向了该元素的 parent 节点对象
- **childNodes：** 该属性为包含了该对象上的是所有子节点对象的 NodeList
- **firstChild：** 该属性指向了第一个子节点
- **lastChild：** 该属性指向了最后一个子节点
- **nextSibling：** 该属性指向了下一个兄妹节点
- **nextSibling：** 该属性指向了上一个兄妹节点
- **nodeType：** 表示节点种类的数字，9 为 Document 节点，1 为 Element 节点，3 为 Text 节点，8 为 Comment 节点
- **nodeValue：** Text 和 Comment 节点的文字内容
- **nodeName：** HTML 标签名，转换成大写

#### 元素属性

HTML 的 element 都拥有一个标签名，以及一系列表示属性的 name/value pairs。我们可以使用 getAttribute(), setAttribute(), hasAttribute(), removeAttribute() 来取得，改变，测试，移除特定的属性。但其实 HTMLElement 对象上拥有对应的属性作为属性名，所以用起来会更方便。

##### 以 ELement 对象属性表示 HTML 属性

Element 对象表示的 HTML 元素通常定义了用于读写的属性。所有的 Element 对象都定义了 id，title，lang，dir 以及事件处理函数。对于某些特定类型的 Element 还定义附加的属性，如 img 的 src 属性。

HTML 的属性不用区分大小写，但是 JavaScript 属性需要，使用 camelCase（驼峰命名法）来命名。某些 HTML 属性名是 JavaScript 中的保留字，对于他们的解决方案通常是加上 html 前缀。比如 HTML 的 for 属性，进入 JavaScript 属性就成为了 htmlFor。class 也是一个保留字，但是它比较特殊，所以改名为了 className。

虽然 Element 对象上的属性定义了读写方法，但没有定义移除的方法，所以再移除属性时调用 removeAttribute() 来实现。

##### class 属性

对于 HTML 元素来说，class 属性十分重要，它的值时一列通过空格分割的 CSS 类。JavaScript 中使用 classsName 属性表示。我们可以直接更改 className，不过更多的时候，我们只需要修改其中一个类。

处于这个原因，Element 对象也定义了 classList 属性，是我们可以将 HTML 的 class 属性当做一个 list 来处理。classList 的值是一个类数组对象，可以被迭代，也可以使用以下方法：

```js
let a = document.querySelector('#a1');
a.classList.remove(); // 删除指定类
a.classList.add(); // 增加指定类
a.classList.contains(); // 是否包含指定类
a.classList.toggle(); // 若该类存在，删除它；若不存在，添加它
```

##### dataset 属性

某些时候我们会需要为 HTML 元素添加更多的数据，然后让 JavaScript 来操纵或者选取它们。在 HTML 中，任何以 data- 前缀开头的属性都是合理的数据，它们并不会改变 HTML 元素展示的样子，而是一种标准的增加附加数据的方法。

在 DOM 中，Element 对象拥有一个 dataset 属性，它指向了一个包含对应的 data- 属性作为属性的对象，不过不再拥有前缀。比如 dataset.x 就指向了 HTML 标签中的 data-x 属性。对应时还会从连字符转到 camelCase，即 data-my-age 会变成 dataset.myAge：

```html
<h2 id="age" data-my-age="22">age</h2>
```

对应

```js
document.querySelector("#age").dataset.myAge; // '22'
```

#### 元素内容

假设 HTML 中有一个 p 元素：

```html
<p>This is a <i>simple</i> example</p>
```

那么它的内容是什么呢？

- 是 'This is a \<i>simple\</i> document' 嘛？
- 又或是 'This is a simple document' 呢？

其实它们都可以被称之为答案，并且拥有特定的用处

##### 以 HTML 作为元素内容

读取 Element 的 innerHTML 属性时，返回的就会是 'This is a \<i>simple\</i> document'，即包含了 HTML 标签的字符串。我们对其进行修改时，也可以加入 HTML 标签，而它们也会被正常的解析。

因为浏览器十分擅长解析 HTML，所以通常改变 innerHTML 是比较高效的，但如果对 innerHTML 使用 += 操作符就不再高效了，因为需要格外的序列化步骤，将 HTML 元素转换成字符串，然后再将新的字符串解析回去。

还有一点需要注意，不要让用户输入的数据进入 document，不然他可能再你的程序中恶意插入它的代码。

outerHTML 和 innerHTML 很像，不过会包含元素本身：'\<p>This is a \<i>simple\</i> example\</p>'。一个相关的方法是 insertAdjacentHTML()，它使我们可以再 HTML 中为指定元素的相邻位置插入字符串标识的 markup。markup 会作为第二个 argument 表示，而第一个 argument 定义了 adjacent 的位置，可以是 'beforebegin', 'afterbegin', 'beforeend', 'afterend' 中的一个：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ea7bb9607554ad1b3f67f325fbbfd68~tplv-k3u1fbpfcp-watermark.image)

##### 以纯文本作为元素内容

在我们只需要元素的内容的文字部分时，或者需要插入纯文本时（没有 markup 的标签等），我们可以使用 textContent 属性来实现，比如前面的例子就会返回 'This is a simple document'。

textContent 属性是在 Node 类上定义的，所以其子类 Element node 和 Text node 都拥有该属性。

#### 创建，插入，删除节点

除去查询和修改之外，我们也可以直接创建，插入或删除节点，然后直接改变 document 的结构。Document 类中顶一个用于创建 Element 对象的方法，Element 对象和 Text 对象又拥有了插入，删除，以及替换节点的方法。

想要创建一个新的 Element 对象并修改它十分的简单：

```js
let p = document.createElement('p'); // 创建新的 p 元素
let em = document.createElement('em'); // 创建新的 em 元素
em.append('world'); // 为 em 从右增加字符串
p.append(em, '!'); // 将 em 加入 p 中
p.prepend('Hello '); // 为 p 从左增加字符串
p.innertHTML // "Hello <em>World</em>!"
```

append() 和 preend() 方法接收任意数量的 arguments，且可以是字符串或者 Node 对象，字符串会自动转换成 Text 节点。（或使用 document.createTextNode() 来显式创建，但是没必要）

如果要在 Element 或者 Text 节点的中间插入，则需要先得到对于某个 sibling 节点的引用，然后调用 before() 或 after() 方法：

```js
// 找到拥有 class='greetings' 的 h2 元素
let greetings = document.querySelector("h2.greetings"); 
// 在它后面添加 p 和一个 horizontal rule（hr）
greetings.after(p, document.createElement("hr"));
```

如果想要复制一个节点，可以使用 cloneNode() 方法，若传入 true 作为 argument，则会将其中所有内容复制；若传入 false 或不传 argument，则只会复制标签和属性。若想要移除一个节点，我们可以使用 document 的 remove() 方法，或者 replaceWith() 用于替换：

```js
let p2 = p.cloneNode(true); // 复制 p
greetings.replaceWith(p2); // 删除 greetings，并用 p2 替代
p2.remove(); // 删除 p2
```

### Scripting CSS

除了操纵 HTML document 的内容和结构之外，JavaScript 也可以操纵 CSS 来改变其展示风格。以下属性是最常在 JavaScript 中被用到的 CSS 风格：

- **display 属性：** 通过将其设置为 none 来隐藏元素
- **position 属性：** 通过将其设置为 absolute，relative 或者 fixed，然后修改元素展示位置
- **transform 属性：** 用于移动，旋转，拉伸元素
- **transition 属性：** 为元素添加动画，这些动画会通过浏览器自动处理，而不需要额外的 JavaScript

#### CSS 类

最基础的操纵 CSS 的用法就是从 HTML 的 class 标签新增或删除 CSS 类名。使用 classList 属性来操作会很简单。比如我们可以定义一个 CSS 类：

```scss
.hidden { // 该类元素会被隐藏
  display: none;
}
```

然后通过 JavaScript 修改 class 名来隐藏/显示元素：

```js
document.querySelector("#x").classList.add("hidden"); // 隐藏 id 为 x 的元素
document.querySelector("#x").classList.remove("hidden"); // 展示它
```

#### Inline Styles

我们可以通过 JavaScript 来修改 HTML 元素的 style 属性来改变它的风格。DOM 为每一个 Element 对象定义了一个 style 属性，指向了对应的 HTML style 属性。style 和大多属性不同，他不是一个字符串，而是一个 CSSStyleDeclaration 对象：一个被解析的，拥有以字符串表示的 CSS 风格的对象。比如我们可以这样定义一个修改元素位置的函数：

```js
function displayAt(element, x, y) {
  element.style.display = 'block';
  element.style.position = 'absolute';
  element.style.left = `${x}px`;
  element.style.top = `${y}px`;
}
```

CSSStyleDeclaration 的命名风格和 HTML 中的不同，因为连字符在 JavaScript 会被当成减号，所以会将 kebab-case 转换成 camelCase，比如 border-left-width 会变成 borderLeftWidth。

在和 CSSStyleDeclaration 打交道时，要记住所有的属性值都是字符串：

```scss
.e { // CSS 中不用字符串来表示
  display: block;
  font-family: sans-serif; 
  background-color: #ffffff;
  margin-left: 300px;
}
```

对应：

```js
// JavaScript 中则需要是字符串
e.style.display = "block"; // 注意分号在字符串外，它们是 JavaScript 的分号
e.style.fontFamily = "sans-serif"; // 我们不需要为 CSS 传入分号
e.style.backgroundColor = "#ffffff";
e.style.marginLeft = "300px"; // 注意是字符串而非数字，也要注意单位
```

我们也可以使用 getAttribute 和 setAttribute，或者 cssText 属性来查询 inline style，而且会简单一些：

```js
// 将 e 的 inline styles 复制到 f 上，下面两个方法都可以
f.setAttribute("style", e.getAttribute("style"));
f.style.cssText = e.style.cssText;
```
在通过 style 属性查询元素的 CSS 时，我们只能得到其 inline style，而无法取得外部 CSS 的风格。

#### Computed Styles

一个元素的 Computed Styles 就是其所有风格的属性和值的集合（包括 inline 的，浏览器定义的，CSS 定义的）：这也是展示元素时用到的属性。Computed Styles 也是通过 CSSStyleDeclaration 对象来代表的，不过和 inline style 不同，它是一个只读对象，我们不能修改它。

我们可以通用 Window 对象上的 getComputedStyle() 方法来取得元素的风格，第一个 argument 是需要查询的元素，第二个可选的 argument 是用于指定 CSS 中的伪代码（类似 ::before 等）：

```js
let title = document.querySelector("#section1title");
let styles = window.getComputedStyle(title);
let beforeStyles = window.getComputedStyle(title, "::before");
```
getComputedStyle() 方法的返回值是一个 CSSStyleDeclaration，它代表了指定元素上所有的风格。虽然都是 CSSStyleDeclaration 对象，但是 inline style 表示的和 computed style 表示的还是有许多区别的：

- computed style 的属性是只读的
- computed style 的属性是绝对的：所以百分比等非绝对值都会转换成绝对值，任何需要 size 的属性（如 font-size 和 margin size）都会转换成 pixels（px 后缀）；颜色则会被转换成 rgb() 或者 rgba() 形式
- computed style 不包含简写属性：比如 margin 不被包含，只会包含其中的 marginLeft，marginTop 等
- computed style 的 cssText 属性是 undefined

通常来说，getComputedStyle() 返回的 CSSStyleDeclaration 对象会拥有更多的信息。但是某些时候它包含的信息是很狡猾的，比如 top 和 left 的属性通常会返回 auto 作为值，虽然一点都不 make sense，不过这是一个合理的 CSS 值。

虽然 CSS 可以用于精确的指定元素的 position 和 size，但是通过查询 computed style 并不是最完美的查询 position 和 size 的方法。等下会介绍更好的方法。

#### Scripting Stylesheets

除去操纵 inline style 之外，JavaScript 也可以操纵 stylesheets（即以 sstyle 标签的元素，或 rel='stylesheet' 的 link）。因为它们都是 HTML 标签，所以可以通过 doncument.querySelector() 等方法查找。style 和 link 元素对应的 Element 对象拥有 disabled 属性，我们可以用它来 disable 整个 stylesheet：

```js
function switchTheme () { // 切换主题
  let light = document.querySelector('#light-theme');
  let dark = document.querySelector('#-theme');
  if (dark.disabled) {
    light.disable = true;
    dark.disable = false;
  } else { 
    light.disable = false;
    dark.disable = true;
  }
}
```

或者我们可以为 DOM 添加 link 来加入 stylesheet：

```js
let link = document.createElement('link'); // 创建 link 对象
link.id = "style"; // 设置 id
link.rel = "stylesheet"; // 设置 rel
link.href = `style.css`; // 设置 href
document.head.append(link); // 加入 document 中
```

#### CSS 动画和事件

假设这有两个 CSS 类：

```scss
.transparent { opacity: 0; }
.fade { transition: opacity .5s ease-in }
```

第一个会使元素完全透明化，而第二个为其增加了 fade 效果。假设我们已经有了一个拥有 fade 类的元素，我们在 JavaScript 中为其增加 transparent 类时，它会自动触发动画；在删除时也会触发。我们不需要在 JavaScript 中额外的定义什么，它们都是纯粹的 CSS 特效，JavaScript 仅仅使用来触发它们的。

JavaScript 也可以被用来监听 CSS transition 的进度，因为浏览器会在 transition 开始和结束时触发事件。transitionrun 事件会在 transition 效果触发时触发，它会在视觉效果变化前触发（如果设有 trainsition-delay）；transitionstart 事件会在视觉效果开始时触发；transitionend 会在动画结束时触发。传入该类事件的处理函数的 argument 会是 TransitionEvent 对象，拥有额外的 propertyName 属性，指定了用于动画的 CSS 属性，以及 elapsedTime 属性指定了从 transitionstart 到 transitionend 花了多少时间。

除去 transition，CSS 还支持 animation。它包含比如 animation-name 和 animation-duration 属性，和一个指定的 @keyframes rule 用于指定其细节。我们也可以通过 JavaScript 增删 CSS 类来触发它。