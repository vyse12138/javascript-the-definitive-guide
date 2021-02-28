## 浏览器中的 JavaScript（JavaScript in Web Browsers）

JavaScript 在 1994 诞生时，最初的目的就是为了给浏览器加上动态的行为。从那以后，JavaScript 发生了很大的进化，它的范围和能力也在飞速增长。如今，网页可以成为用 JavaScript 进行开发的完整平台了。我们可以使用 JavaScript 为网页进行：

- 操作文本的内容和风格
- 判断文本元素的在屏幕上的位置
- 创建可复用的展示组件
- 画图
- 生成以及播放音乐
- 管理浏览器的导航栏和历史记录
- 在网络上交换数据
- 在用户本地存储数据
- 进行多线程运算

### 网页编程基础（Web Programming Basics）

这一章介绍了对于网页的 JavaScript 代码的结构是如何的，是如何引入浏览器的，如何操作输入和输出，以及如何执行异步代码的。

#### HTML \<script\> 标签中的 JavaScript

浏览器可以显示 HTML 文本，但是若想要让浏览器执行 JavaScript 代码，则必须将 JavaScript 的代码（或代码的引用）加入 \<script\> 标签中。

JavaScript 可以以 inline 的形式插入 HTML 的 \<script\> 和 \</script\> 标签中，但是我们通常会使用 \<script\> 的 src 属性（attribute）来指定包含 JavaScript 的 URL：

```html
<script src="scripts/code.js"></script>
```

注意不要忘了结束标签。使用 src 属性代替 inline script 拥有以下的好处：

- 使 HTML 文件更为整洁，即分割内容和表现行为
- 当多个网页公用同一段 JavaScript 代码使，使用 src 只用维护一段代码
- 即使一个 JavaScript 文件被多个网页使用，它也只有在第一次会被下载，剩余的网页会从浏览器缓存（cache）中取得它
- 因为 src 使用 URL 作为值，网页也可以从别的服务器上获取 JavaScript 文件

**模块**

在 JavaScript 模块的部分介绍了用 import 和 export 来将代码分割成模块，如果要加载模块，则在 top-level 模块加载时使用的 \<script\> 中需要加入 type="module" 属性，这样该模块以及其子模块都会被递归加载：

```html
<script src="scripts/mainModule.js" type="module"></script>
```

**指定 script 的 type 属性**

在从前，浏览器可能拥有除 JavaScript 外的别的语言，所以有时需要加上 language="javascript" 和 type="application/javascript" 的属性。但是现在大可不必，因为 JavaScript 是默认且唯一的浏览器语言。要修改 script 标签中的 type 属性只会是因为以下两种情况：

- 指定该 script 为模块
- 嵌入数据的同时不进行展示（会在之后介绍）

**script 中的 async 和 deferred**

因为在原来，JavaScript 中唯一用来改变网页展示文本的方法就是使用 document.write()，而该方法必须在 HTML 加载前执行。

所以这一方法使得 script 标签会默认先运行 JavaScript 代码，再进行剩余 HTML 的渲染。而这种默认方法会阻塞王爷的渲染，大大减缓渲染速度。

为了解决这一情况，script 标签可以用有 defer 或者 async 属性，用于改写其默认运行方式。这两个属性都是布尔值，所以只需要再 script 标签中出现即可，且只有于 src 属性同时出现才有意义：

```html
<script defer src="deferred.js"></script>
<script async src="async.js"></script>
```

这两个属性告诉浏览器加载的 JavaScript 代码不适用 document.write()，可以放心的继续加载页面，并在后台继续下载 script。

defer 属性使浏览器推迟 script 的执行到文本已经被加载和解析完成之后，即可被操纵之后；async 属性使浏览器在不阻塞文本解析的情况下，尽可能早的执行 script（须在 script 下载完成之后）。如果两个属性都出现在了同一个 script 标签下，async 优先级更高。

若有多个 script 标签，defer 依然会以 HTML 中的顺序执行，而 async 则会在加载完之后直接执行，所以顺序可能会被打乱。

type="module" 的 script 会默认在 document 加载后执行，即和 defer 一样。不过我们也可以用 async 改写它，使其模块加载完成后便直接执行。

或者说我们可以把代码放在 HTML 文件的最后，这样它们也会在 document 解析完成之后开始运行。

#### DOM（Document Object Model）

JavaScript 客户端程序中最重要的对象之一就是 Document 对象，它表示了页面展示的 HTML 文本。操作 HTML 文本的对象被成为 DOM（Document Object Model）。这里会做简单的介绍，下面的章节会有更详细的内容。

HTML 中的文本包含了 HTML 的元素，元素也会嵌套在别的元素之中：

```html
<html>
  <head>
    <title>Sample Document</title>
  </head>
  <body>
    <h1>An HTML Document</h1>
    <p>This is a <i>simple</i> document.
  </body>
</html>
```

top-level 的 html 标签包含了 head 和 body 标签，head 又包含了 title 标签，body 包含了 h1 和 p 标签，p 又包含了 i 标签。

DOM API 会以树形结构来映射出 HTML document。对于每一个 HTML 标签中，这都会对应一个 JavaScript Element 对象；每一个 document 中的文本都会有一个对应的 Text 对象。Element 和 Text 类，以及 Document 类，它们都是 Node 类的子类。Node 对象被组织成为树形结构，这使得 JavaScript 可以用 DOM API 对其进行查询以及遍历。下图为上面例子的树形结构：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a86897a71a4445f886e6e5cc9e355bb3~tplv-k3u1fbpfcp-watermark.image)

DOM API 包含了用于创建新的 Element 以及 Text 节点的方法，以及移动或者移除它们的方法。

对于每一个 HTML 的标签类型，JavaScript 中都有一个对应的类。每次 HTML 中该标签的出现，都会以该标签对应的类的实例形式在 JavaScript 中表示。比如 body 标签会以 HTMLBodyElement 类的实例表示。每一个 Element 对象都拥有对应 HTML 标签属性的属性。比如 HTMLImageElement 类的实例，即 img 标签，都有一个 src 属性表示了 HTML 中 img 标签中对应的 src 属性，且值也会从 img 标签中继承，在 JavaScript 中改变该对象的值，也会改变 HTML 中 img 标签的中 src 的值，并使得浏览器重新渲染。

#### 浏览器中的全局对象

对于浏览器的每一个页面，这都有一个全局对象，所有的 JavaScript 代码（除了在 worker 线程运行的），都运行在这个全局对象上。

全局对象就是 JavaScript 标准版被定义的地方，比如 parseInt() 函数，Math 对象，Set 类等等。在浏览器中，全局对象也包含了各种 web APIs。比如 document 属性标示了当前展示的 document，fetch() 方法可以进行 HTTP 请求等。

在浏览器中，请求对象还有一个任务。除了定义内嵌的类型和函数外，它还表示了当前浏览器窗口定义的属性如 history，innerWidth 等。

全局对象还拥有一个名为 window 的属性，它的值指向了全局对象本身。在使用窗口指定的特性时，加上 window 前缀会使得表达更清晰。

**共享命名空间的 scripts**

对于 module script 来说，它们拥有其私用的命名空间，即不进行 export 的话，是无法被访问到的。但是对于非 module script，情况则不是这样，所有的 scripts 都会共有一个命名空间，即在一个 script 中的 top-level 定义的变量，在另一个 script 中也可以被访问。

#### JavaScript 程序的执行

JavaScript 程序可以认为是一个 HTML 文件中所有的 scripts。程序的执行可以分为两个阶段：

- 第一阶段是在 document 内容加载完成时，scripts 中的代码会开始执行，默认是情况下或根据其出现的顺序来执行，但可以通过前面提到的 defer 和 async 来改变。对于每一个 script，其都会被从上到下执行。某些 scripts 可能在一开始并不会做任何事，而只是定义了一些函数和类，以便于在第二阶段使用。而某些 scripts 可能在这第一阶段执行了许多工作，而在第二阶段并没有什么事情需要做。比如某一个 script 的作用是提取 document 中所有的 h1 和 h2 来生成并且渲染 table of content，这个 script 就全部在第一阶段执行。

- 第二阶段是在 document 内容加载完，并且所有的代码都执行过一次之后进入的。这个阶段是异步，事件驱使的。如果一个 script 想要参与第二阶段，她至少需要在第一阶段中注册 event handler 或者其他的异步 callback 函数。在这第二阶段中，浏览器会在事件发生时，调用对应的 callback 函数。例如用户点击，按下键盘，加载资源等。

在第二阶段中最先执行的一些事件被称为 DOMContentLoaded 以及 load 事件。DOMContentLoaded 会在 HTML document 加载和解析完毕后触发，load 事件是当所有的 document 中的外部资源（比如图片）也加载完毕后触发。JavaScript 程序通常会调用其中一个来进行一些初始化的设置。

JavaScript 的 loading 阶段相较之下是非常短暂的，理想状态下使少于一秒的。在 loading 阶段结束后，第二阶段，也就是 event-driven 阶段会一直持续，直到浏览器窗口不再显示。

##### 客户端 JavaScript 线程 model

JavaScript 是一个单线程的语言，编写单线程会比较简单：我们可以认定两个 event handlers 不会同时运行；我们也可以知道在修改 document 时，没有其他的线程会对其同时进行修改；所以也不需要考虑 locks，deadlock 或者 race conditions 等。

但单线程执行也意味着在执行 event handlers 时，浏览器会停止对于用户输入的响应，这使 JavaScript 也拥有了弊端：如果一个 event handlers 执行了太久，则在那一段时间内浏览器是完全无响应的，这会使得用户认为浏览器崩溃了，并且会减少用户体验。

为了解决这一情况，浏览器平台定义了 web worker。它是一个后台线程，即使在其中执行长时间的任务也不会使浏览器不响应。web worker 会在接下来被介绍。

##### 客户端 JavaScript 时间轴

我们已经提到了 JavaScript 程序的两个阶段，它们其实可以被细分为以下的步骤：

1. 浏览器创建 Document 对象，并开始解析 HTML 文件，将 HTML 的元素以 Element 对象和 Text 节点的形式加入 Document 对象。在这一状态下，document.readyState 拥有 'loading' 作为值。

2. 在 HTML 解析器遇到 script 标签时，并且其不具有 async，defer 或者 type="module" 属性时，它会将那个 script 标签加入 document，然后执行那段 script。script 是同步执行的，所以 HTML 解析器会先暂停，等待 script 的下载和执行。那样的 script 可以使用 document.write() 来在 input stream 中插入文本，那些文本在解析前重新开始时，就会成为 document 的一部分。虽然这样的 script 通常会用来定义一些函数或者回调函数，不过他也可以遍历以及操纵已经生成的 document tree。

3. 在 HTML 解析器遇到拥有 async 属性的 script 标签时，他会开始下载 script（或者这个 script 是一个 async 模块，它也会被递归下载其所用 dependencies），然后继续解析 document。script 会在下载完成后开始执行，但是解析器并不会再像上面一样挂机傻等着它下载了。这种异步加载的 script 不能拥有 document.write() 方法，但是也可以访问在其之前的 document 内容，也可能可以访问更多（若下载的慢）

4. 在 document 解析完毕后，document.readyState 属性会改为 "interacitive"。

5. 任何拥有 defer 的 script（或者没有 async 属性的模块）会以其在 HTML 中出现的顺序下载和执行。Async 的 script 也可能在这时候执行（若刚下载完成）。Deferred scripts 拥有对整个 document 的完全访问，且不能使用 document.write() 方法。

6. 接下来，浏览器会在 Document 对象上传出 DOMContentLoaded 事件。这标记了同步执行的结束，也是异步执行的开始。（不过还有可能有 async script 还未被执行，即未下载完成）

7. 在这个时候，document 已经解析完成了，不过浏览器可能依然在等待剩余内容的加载，如图片。在那些内容都加载完成，以及所有的 async 代码都加载和执行完成之后，document.readySState 属性会改为 “complete”，浏览器也会在这时候在 Window 对象上传出 load 事件。

8. 在这个时间点及以后，会进入异步阶段，callback 会在异步 event 发生时被调用。

#### 程序的输入和输出

客户端的 JavaScript 也拥有输入和输出的数据，输入的数据可以是：

- document 的内容，JavaScript 可以通过 DOM API 访问
- 用户输入，在鼠标点击按钮或者文字进入 textarea 标签等事件或发生 event
- document 展示的 URL 可以通过 document.URL 来访问。若在 URL() 构造函数中传入字符串，则可以轻松的访问器域名，path 等
- HTTP 请求头中 Cookie 的内容可以通过 document.cookie 访问
- 全局 navigator 属性给予了对浏览器信息，操作系统等信息的访问

客户端的 JavaScript 通常会在必要的时候产生输出，输出可能是用 DOM API 修改 HTML document，或使用高层框架如 Vue/React 来操纵 document，或者是使用 console.log() 来展示信息。

#### 程序错误

不同于直接运行在系统上的应用，浏览器中的 JavaScript 程序并不能真正的 crash。如果 JavaScript 运行时发生异常，并且没有异常捕获机制，则错误信息会在 console 显示。但已注册的 event handlers 依然可以继续工作。

如果想要定义一个最后的异常捕获装置，可以通过改写 Window 对象的 onerror 属性来实现（传入一个新的 error handler 函数）。若之前没有别的异常捕获机制，则异常会在此处被捕捉。

window.onerror 函数会接收三个 arguments，第一个是 error 信息，第二个是 URL 表示的错误发生处，第三个是错误出现的行数。

对于没有 catch 处理的 rejected Promise，我们也可以用 window.onunhandledrejection 捕获。

#### 浏览器安全机制

因为浏览器可以在个人的设备上执行 JavaScript，所以浏览器引擎需要对以下两点做出平衡：

- 定于强大的客户端 APIs 用于创建 web apps
- 阻止恶意代码改写数据，访问私人信息等恶意行为

浏览器对于防止恶意代码的最基础的防线便是不支持某些能力。比如客户端 JavaScript 不支持写入或者删除文件/文件夹，这使得 JavaScript 无法 删除数据或者植入病毒。

下面介绍了我们应该知道的一些 JavaScript 安全限制。

##### 同源政策

同源政策限制了哪些网络内容可以被 JavaScript 交互。script 只可能读取拥有同源的内容的 windows 和 documents 属性。

document 的起源是根据 URL 的网络协议（protocol），域名(host)，端口（port）来决定的。从不同的服务器加载的 documents 拥有不同的起源；对于同一服务器上的资源，如果网络协议不同（http: 和 https:），也不同源。浏览器通常也将本地文件定义为一个单独的起源。

同源政策同样会影响 HTTP 请求，级 JavaScript 代码只能对 document 加载的起源发送 HTTP 请求，而会阻止对于其他服务器的交互（除非其拥有特定的 CORS 选项，下面会被介绍）。

如果一个大型网站包含了多个子域（subdomains），且它们之间需要通信的话，则可以对 document.domain 进设定。

第二种用于松动同源政策的技术被称为 CORS（Cross-Origin Resource Sharing）。这允许服务器去确定它们可以服务那些起源。CORS 会在 HTTP 请求头加上 Origin，会在响应头加上 Access-Control-Allow-Origin。这使得服务器可以增加一个指定可以访问其的起源的集合。

##### XSS

XSS 也被称为 CROSS-SITE SCRIPTING，它代表了一类安全问题：黑客可以通过注入 HTML 标签，或者注入 script 的形式来恶意破坏一个网站。所以说，客户端的 JavaScript 必须注意，并且防御 XSS。

在拥有动态生成的文档时，比如用户输入的数据，网站很容易暴露于 XSS 的攻击下。所以我们要先对用户的输入进行 sanitizing，用于移除所有内嵌的 HTML 标签。

比如下面的例子会使用 JavaScript 来对用户名进行问候（此问候非彼问候）：

```js
let name = new URL(document.URL).searchParams.get('name');
document.querySelector('h1').innerHTML = 'Hello' + name;
```

这两行代码会从 document URL 中提取用户名，然后注入 h1 标签中进行展示，所以这两行代码会期待这样的 URL：

```html
http://www.example.com/greet.html?name=David
```

在这种情况下，h1 会展示 'Hello David'。但如果 name 的属性改成了如下会发生什么呢？

```html
http://www.example.com/greet.html?name=%3Cimg%20src=%22x.png%22%20onload=%22alert(%27hacked%27)
%22/%3E
```

在 URL 中的参数解析完成后，它就使得一下 HTML 被注入进了 document：

```html
Hello <img src="x.png" onload="alert('hacked')"/>
```

在图片加载完成后，onload 中的 JavaScript 就会被执行。这个例子简单的展示了用户输入是如何操纵 HTML document 的，所以对于未知的输入，一定要确保对其进行消毒。

XSS 攻击之所以被称为 CROSS-SITE SCRIPTING，是因为可能会有多个网站同时介入。假设恶意网站 B 拥有一个精心制作的链接指向了网站 A。如果网站 B 上的用户被吸引然后点击了那个链接，他们便会被带到网站 A，只不过网站 A 还是会运行网站 B 上的代码，网站 B 上的代码就可以使网站 A 不已正常方式运行。更为危险的是，恶意代码甚至可以读取网站 A 存储的 cookies（比如账号信息或个人身份信息等），然后发送回网站 B。

总的来说，防止 XSS 攻击最好的办法就是移除所有不可信输入的 HTML 标签，然后再进行动态 document 的生成：

```js
name = name
  .relplace(/&/g, "&amp;")
  .replace(/</g, "&lt;")
  .replace(/>/g, "&gt;")
  .replace(/"/g, "&quot;")
  .replace(/'/g, "&#x27;")
  .replace(/\//g, "&#x2F;");
```

另一种解决 XSS 的方法是将任何不信任的内容展示在拥有 sandbox 属性的 iframe 中。