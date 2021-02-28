### Document 的滚动和几何学（Document Geometry and Scrolling）

我们已经介绍了 document 是如何以树形结构进入 DOM 的，不过在将其渲染到页面上时，我们需要对其中的每一个元素定义位置（position）和大小（size）。虽然大多数情况下这都是由 HTML 和 CSS 完成的，不过某些时候也会用到 JavaScript 来进行动态的渲染。

#### Document 坐标系和视口坐标系

document 中元素的位置是通过 CSS 像素来测量并以坐标系的格式定义的，x 坐标值增加会使元素向右移，y 坐标值增加会使元素向下移。但其实对于该坐标系而言，其原点一共有两种可能：
- 第一个是相对于 document 的左上角
- 第二个则是相对于 document 展示的视口（viewport）的左上角

对于 top level 的窗口和页面来说，视口就是浏览器用于展示 document 内容的部分：即不包含菜单，标签，导航栏等信息。对于展示在 iframe 标签中的 documents，则由 DOM 中的 iframe 元素来定义内嵌 document 的视口。不论是哪一种情况下，我们在用到位置时，一定要确定是使用 document 坐标系还是视口坐标系。（视口坐标系（Viewport Coordinates）有时也被称为窗口坐标系（window coordinates）） 

如果一个 document 比视口小，或者 document 还没有被滚动过，则 document 的左上角就是视口的左上角，而在这种情况下，document 坐标系和视口坐标系是相同的。而通常情况下，想要在两个坐标系之间转换，则需要加减 scroll offsets。

在多数情况下，document 坐标系的效果并不是很好，主要是因为 CSS 的 overflow 属性使得元素可以拥有更多的内容，并且可以使其拥有独自的滚动条。这使得我们很难只通过单一的 (x, y) 坐标来描述位置。因为这个原因，客户端的 JavaScript 会更偏向于使用视口坐标系。

在使用 CSS position:fixed 来显式的指定元素的位置时，top 和 left 属性会在视口坐标系中被编译。如果使用 position:relative 则是默认位置。如果使用 position:absolute，则 top 和 left 会是相对于 document 中最近的容器的位置。

#### 查询元素的几何属性

我们可以通过 getBoundingClientRect() 方法来查询一个元素的大小（包含 border 和 padding，但不包含 margin）和位置（在视口坐标系中）。该方法不接收任何 argument，然后返回一个拥有属性 left，right，top，bottom，width，height 的 DOMRect 对象。left 和 top 是元素左上角的坐标， right 和 bottom 是元素右下角的坐标，而 width 和 height 是它们的差值。

Block 元素，如 img，p，div 等元素会一直是长方形的，而 inline 元素，如 span，code，b 则可能跨度多行，所以拥有多个长方形。对于这种 inline 元素，getBoundingClientRect() 的 width 是所有长方形的长度和；或者我们可以调用 getClientRects() 方法，用于返回一个类数组对象，其元素是 inline 元素中的所有长方形。

#### 查询一个点对应的元素

某些时候，我们想要通过视口中的一个点来查询元素，我们可以使用 Document 对象上的 elementFromPoint() 方法来实现。我们在调用时传入表示视口坐标系中的点的 x 和 y 值，作为其两个 arguments。（比如传入 mouse 事件的 clientX 和 clientY）

然后该方法会返回在那个点上的最深处的 Element 对象。

#### 滚动

Window 对象上的 scrollTo() 方法接收 x 和 y 坐标作为 arguments，然后将它们作为滚动条的 offsets。这意味着它会将窗口滚动到指定的点作为视口的左上角：

```js
// 得到 document 和 视口的高度
let documentHeight = document.documentElement.offsetHeight;
let viewportHeight = window.innerHeight;
// 滚动到最后一页
window.scrollTo(0, documentHeight - viewportHeight);
```

Window 对象的 scrollBy() 方法和 scrollTo() 方法很像，不过它的 arguments 是相对的，会增加到当前滚动位置之上：

```js
// 每一秒向下滚动 50px
setInterval(() => { scrollBy(0,50); }, 100);
```

或者使用 Element 对象上的 scrollIntoView() 方法来滚动到指定的 HTML 元素的位置上。默认情况下，滚动之后的 HTML 元素会出现在视口顶部，如果传入 false 则会出现在底部。

我们也可以只传入一个对象作为 scrollBy()，scrollTo() 和 scrollIntoView() 的 argument，用于描述该滚动：

```js
window.scrollTo({
  left: 0, // x 
  top: documentHeight - viewportHeight, // y
  behavior: "smooth" // 滚动时的行为
});
```

#### 视口大小，内容大小以及滚动位置

对于浏览器窗口来说，视口的大小是通过 window.innerWidth 和 window.innerHeight 属性来显示的。我们可以通过在 head 中加入 \<meta name="viewport"\> 的标签来指定视口宽度，这常常会在对于手机设备的优化时用到。

document 的大小和 html 元素中的大小其实是一样的，所以我们可以调用 document.documentElement（html 元素）上的 getBoundingClientRect() 方法来取得 document 的大小，或者是 offsetWidth 和 offssetHeight 属性来取得宽或高。

document 位于视口中的 scroll offsets 也可以通过 window.scrollX 和 window.scrollY 来查询（它们是只读属性）。

对于 Element 对象来说，事情就会复杂一点，每一个 Element 对象都拥有以下三组属性：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d55905b933204f1ba620040bde750010~tplv-k3u1fbpfcp-watermark.image)

offsetWidth 和 offsetHeight 代表了元素在屏幕上的大小（包括内容，padding，border），通过 CSS 像素表示；offsetLeft 和 offsetTop 则会返回元素的 x 和 y 坐标；对于某些特定的元素，如 table cells 等，它们返回的属性是相对于其 ancestor（祖先）元素的坐标，而非 document，offsetParent 就可以返回其是相对于哪一个元素的。

clientWidth 和 clientHeight 十分类似于 offsetWidth 和 offsetHeight系列很像，只不过不包括 border 的大小，只有内容和 padding；clientLeft 和 clientTop 通常则没什么用，它们只会返回元素外 padding 到外 boarder 的距离。

scrollWidth 和 scrollHeight 会返回元素的内容，加上它的 padding，再加上它的 overflow 内容。在内容没有 overflow 时，值等于 clientWidth 和 clientHeight；scrollLeft 和 scrollTop 会返回元素内容相对于该元素视口的 scroll offsets，我们可以通过改变它们的值来在元素的内容中滚动。（或者使用 Element 对象上的 scrollTo() 和 scrollBy() 方法，不过这一特性还未被所有浏览器支持）

### 网页组件（Web Components）

对于如今的网页开发来说，很少有人会使用 raw HTML 来编写，而是更倾向于使用框架如 Vue/React/Angular 等。它们使我们可以创建可复用的组件。

而网页组件（Web Components）则是浏览器原生的，使用 JavaScript 来 extend HTML，并创建新标签的方法。我们有时可以使用它们来代替框架开发的组件。

#### 使用网页组件

网页组件是在 JavaScript 中定义的，所以想要使用使用它们，我们只需要将定义了组件的 JavaScript 的文件引入即可。通常情况下是以模块的形式使用的：

```html
<script type="module" src="components/search-box.js">
```

网页组件会定义新的 HTML 标签名，不过这些标签必须包含连字符（为了不和以后可能会新增的 HTML 标签弄混）：

```html
<search-box placeholder="Search..."></search-box>
```

网页组件可以和普通 HTML 标签一样持有属性，但是无法使用 self-closing tags（即无法使用 \<search-box/\>）。

某些网页组件的定义中指定了它可以接收特定子元素，然后出现在 slots 中。在使用时，我们只要传入拥有 slot 属性的元素作为其子元素即可：

```html
<search-box>
  <img src="images/search-icon.png" slot="left"/>
  <img src="images/cancel-icon.png" slot="right"/>
</search-box>
```

前面提到过使用 type='module' 定义的 script 会在 document 内容解析完之后再加载，就如同拥有 defer 属性一样。这意味着浏览器会先解析并渲染这类网页组件（如例子中的 \<search-box\>），然后才能从 JavaScript 代码中知道这个组件究竟是什么。使用网页组件时，这一现象是十分合理的，因为浏览器的 HTML 解析器是十分 flexible 的，他在遇到这类还未被定义的网页组件的标签时，它会在 DOM tree 中先加入一个通用的 HTMLElement，然后在解析到定义该网页组件的 JavaScript 时，再去将通用的 HTMLElement 升级为一个更为精确的 Element。

如果一个网页组件拥有子元素的话，它们通常在加载到定义该组件的 JavaScript 之前是无法正确显示的，所以我们可以使用这样的 CSS 来先将它们隐藏起来：

```css
search-box: not(:defined) {
  opacity:0;
  display: inline-block;
  width: 300px;
  height: 50px;
}
```

和普通的 HTML 标签一样，自定义的网页组件也可以通过 querySelector() 来选择。

**DocumentFragment**

在更深入的介绍网页组件之间，要回到 DOM API 来解释一下其中的 DocumentFragment 是什么。DOM API 会将 document 转换成一个包含了许多节点的树形结构，每一个节点可以是 Documet，Element，Text 甚至是 Comment 节点，但是它们都无法在不包含 parent 节点的情况下表示其下的所有 sibling 节点。这就是 DocumentFragment 的出现的原因：他是另一种节点，可以作为一组 sibling 节点的临时 parent 节点。我们可以通过 document.createDocumentFragment() 来创建。虽然它可以像 Element 节点一样 append 内容，但是它不拥有 parent。所以在将 DocumentFragment 节点插入 DOM tree 时，DocumentFragment 本身并不会被插入，只有其子元素会被插入。

#### HTML Templates 

HTML 中的 template 标签其实和网页组件的关系不是很紧密，但是它对常出现的组件进行了优化。template 标签中的元素不会被浏览器渲染，而只有在使用 JavaScript 时它们才有用。其背后的思想时：当一个网页包含许多重复的基本结构时，我们可以通过 template 定义它一次，然后通过 JavaScript 去多次渲染它们。

在 JavaScript 中，template 标签是通过 HTMLTemplateElement 对象表示的。该对象只拥有一个 content 属性，代表了一个包含所有子元素的 DocumentFragment。我们可以对其进行复制，然后插入 document 中来使用。比如我们可以用它来渲染表格：

```js
let table = document.querySelector('tbody'); // 表格
let template = document.querySelector('#row'); // template 标签中的 row
let i = 10;
while(i--) {
  let clone = template.content.cloneNode(true); // 深拷贝
  table.append(clone); // 将 template 插入 table 
} 
```

template 标签页不一定需要在 HTML 中以字面量的形式出现，我们也可以在 JavaScript 中创建它们，然后通过 innerHTML 为其增加元素。

#### 自定义 Elements

第二个用于网页组件的特性就是自定义元素：将一个 HTML 标签与一个 JavaScript 类关联，然后任何该 HTML 标签在进入 DOM tree 时都会自动转换成该 JavaScript 类。我们可以使用 customElements.define() 方法，传入一个网页组件的标签名作为第一个 argument，传入 HTMLElement 的子类作为其第二个 argument。所有网页组件标签会升级成为新的子类的实例，任何以后新建的网页组件则会直接创建该子类的实例。

浏览器会自动调用自定义元素类上的特定的生命周期方法，比如 connectedCallback() 方法会在自定义元素插入 document 时调用；disconnectedCallback() 方法会在自定义元素移除时调用；若自定义元素类上还定义了 observedAttributes 属性（它的值是一个包含 HTML 属性名的数组），只要任何其中的属性发生改变，就会调用 attributeChangedCallback() 方法，并传入属性名，旧值和新值作为 arguments。

#### Shadow DOM

仅靠自定义元素来实现的网页组件还是没有被完全封装（encapsulated）的。若想要将一个自定义元素升级成为真正的网页组件的话，我们必须使用强大的封装机制，该机制被称为 shadow DOM。

shadow DOM 可以让我们在一个自定义元素上 attache 一个 shadow root，然后该自定义元素节点会被称为 shadow host。shadow host 的元素，其实和 HTML 元素一样，已经是其后裔组成的树的根节点了。而一个 shadow root 则是另一个更为私密的树的节点，该树也由 shadow host 作为根节点展开（可以想象成一个独立的 minidocument）。

shadow DOM 中的 shadow 意味着 shadow root 的后裔元素都是 hiding in the shadows 的，即不是普通 DOM tree 的一部分，也无法被普通的 DOM 遍历（如 querySelector()）查找到。所以某些时候，shadow host 下的普通 DOM 元素也被称为 light DOM 用于区分。

为了理解 shadow DOM，我们可以想象一下 HTML 中的 video 和 audio 元素：它们是一个独立的 UI，用于操纵媒体的播放。但是其中的播放，暂停，音量调节以及其他的 UI 元素都不是 DOM tree 的一部分，所以也无法被 JavaScript 所操纵。它们就遵循的 shadow DOM 的标准。

##### Shadow DOM 封装

shadow DOM 最重要的特性就是封装。shadow root 的子元素可以从 DOM tree 中独立出来并且被隐藏。shadow DOM 拥有三种很重要的封装特性：

- 在创建一个 shadow root 并将其 attache 到 shadow host 时，我们可以用 open mode 或者 closed mode 来创建它。一个 closed 的 shadow DOM 是完全密封，且无法访问的。但大多数情况下，我们都会创建 open shadow DOM。这意味着 shadow host 拥有 shadowRoot 属性，然后我们可以通过该属性来访问 shadow root 下的元素。

- 在 shadow root 下定义的风格是私用的，即不会影响 light DOM 的元素，同样的，shadow host 下 light DOM 的风格也不会影响 shadow root 下的风格。shadow root 可以为 shadow host 元素定义默认风格，不过会被 light DOM 的风格覆盖。shadow DOM 中的元素依然会从 light DOM 中继承 font-size，background-color，但除此之外，大多数风格则是独立的。所以在使用网页组件时，我们不应该担心外在的 CSS 会影响它。所以 scope CSS 也是 shadow DOM 的特性之一。

- 某些在 shadow DOM 中触发的事件，如 load，是被封闭在 shadow DOM 之中的。而另一些则也会冒泡，如 focus，mouse，keyboard 事件等。在 shadom DOM 中的事件冒泡到其边界之外，准备进入 light DOM 时，它的 target 属性就会变成 shadow host 元素。

##### Shadow DOM 的 Slots 和 Light DOM 的 Children

被称为 shadow host 的 HTML 元素维护了两个子元素的树，第一个是 children 数组，即普通 light DOM 的子元素；第二个是 shawdow root 以及其子元素：

- shadow root 中的子元素会显示在 shadow host 下。

- 如果 shadow root 中的子元素拥有 slot 元素，则 light DOM 中的元素会以 slot 的子元素的形式被显示，并替换 slot 中的 shadow DOM 内容。反之，若 shadow DOM 没有 slot 的元素，则 light DOM 的内容不会被显示。若 shadow DOM 拥有 slot 元素但没有 light DOM，则默认展示 shadow DOM 的内容。

- 当 light DOM 内容显示在 shadow DOM 的 slot 中时，我们称那些元素被 distributed，但并没有真正的成为 shadow DOM 的一部分，它们依然时 DOM 中 children 的一部分。

##### SHADOW DOM API

我们可以在 light DOM 元素上调用其 attachShadow() 方法，并传入 {mode：'open'} 作为 argument，来将其转换成一个 shadow host。该方法会返回一个 shadow root 对象，并且将 shadow host 的 shadowRoot 属性指定为该对象。、

shadow root 对象是一个 DocumentFragment，我们可以使用常规的 DOM 方法来为其增加内容（如 innerHTML）。