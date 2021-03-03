### 本地存储

我们编写的 Web App 可以使用浏览器的 APIs 用于进行本地存储。本地存储通常是由源头进行分割的，即两个不同的网站无法访问同一本地数据，不过不同页面下的同一网站则可以。本地储存的内容是不会进行加密的，所以可以用于存储主题风格等，但不要用来存储私密信息如密码。

#### localStorage 和 sessionStorage

localStorage 和 sessionStorage 都是 Window 对象上的属性，它们都指向了一个 Storage 对象。该对象和普通的 JavaScript 对象一样，除了：

- Storage 对象的属性名必须是字符串
- Storage 对象的属性会 persist，即它不会在刷新页面时消失

我们可以通过 delete 操作符，或者 clear() 方法来删除 localStorage 和 sessionStorage 的属性，也可以使用 for in，或者 Object.keys() 来遍历 Storage 对象的属性。或者也可以使用 getItem(), setItem(), deleteItem() 方法来访问属性。

##### 存储的寿命和作用域

localStorage 和 sessionStorage 的区别是他们的寿命和作用域。

localStorage 中存储的数据是永久的，除非我们主动去删除，不然它会一直存在。localStorage 的作用域有以下两个：

- 第一个是 document 的源头，即必须符合同源政策。同源的 documents 都会共享同一 localStorage，即使是在不同页面下也可以
- 第二个是浏览器实现，即使访问同一网站，不同的浏览器也拥有不同的 localStorage

sessionStorage 的寿命则不同，它拥有和当前窗口（当前浏览器页面）一样的寿命。当窗口关闭时，sessionStorage 的数据就会被删除（不过可以通过重新打开最近关闭页面的功能来复原 sessionStorage）。sessionStorage 的作用域也有两个：

- 第一个是 document 的源头，即必须符合同源政策。同源的 documents 都会共享同一 sessionStorage，即使是在不同页面下也可以
- 第二个是窗口，不同窗口中的同一页面也拥有独自的 sessionStorage

##### 存储事件

在 localStorage 的数据改变时，浏览器会触发 storage 事件，与其关联的 Event 对象拥有以下额外属性：

**key**

被删除或更改的属性名。若调用的是 clear() 方法，则该值为 null

**newValue**

持有新的值，如果调用的是 removeItem() 方法，则该属性不存在

**oldValue**

持有旧的值，若是新建的属性，则该属性不存在

**storageArea**

指向改变的 Storage 对象，通常是 localStorage 对象

**url**

持有 Storage 对象的 document 的 URL

#### Cookies

cookie 浏览器存储的于某一个页面/网站关联的少量数据。cookies 的初衷是为了便于服务器端的开发，它们也是通过 HTTP 协议的 extension 实现的。cookie 的数据会自动在浏览器和服务器之间传输，所以服务器可以读写浏览器中保存的 cookie 数据。不过客户端 JavaScript 也可以在 Document 对象上操作 cookie。

对于读写 cookie 来说，这并没有方法参与其中，而是操纵以特殊格式存在的字符串。cookie 的寿命和作用域可以通过特性来自定义。

##### 读取 cookies

我们可以通过读取 document.cookie 属性来读取 cookies。他返回了一个包含 cookies 的字符串，使用分号和空格来分开每一个 name/value pairs。这样读取的 cookie 值就是值本身，不包含任何于其关联的特性。在得到 cookie 的值之后，我们还需要根据其加密方法来翻译它。

##### cookies 的特性

除了 name 和 value 之外，每一个 cookie 还拥有可选的特性用于修改其寿命和作用域。默认情况下，cookies 的寿命是等同于当前浏览器的寿命，所以它会在退出浏览器后丢失。不过我们可以为其指定 max-age 特性用于表示其持续时间，这样的话 cookies 会被浏览器保存到文件中，然后在它过期时才会被删除。

cookies 的作用域也必须符合同源政策，不过默认情况下它还与 URL 的路径关联，即对于某一个页面而言，只有于其在同一文件夹下（或者子文件）时，才可以访问到同样的 cookies。不过我们可以通过 path 和 domain 特性来改写其作用域。比如将 path 设置为 '/' 就能使得同域名下的不同路径都可以访问该 cookie；将 domian 设置为 '.example.com'，则类似 data.example.com, orders.example.com 都可以访问该 cookie。

另一个 cookies 的特性是 secure，它是一个布尔值，用于表示 cookie 是如何在网络上传输的。默认值为 false，即通过普通 HTTP 连接传输。如果该值为 true，则通过 HTTPS 或者其他安全协议传输。

##### cookies 的限制

cookies 的目的是为了储存被服务器使用的少量数据，在每一次对应的 URL 请求发送时，cookies 都会被上传到服务器。

虽然标准希望浏览器可以支持无限多以及无限大的 cookies，不过不需要浏览器持有超过 300 个 cookies，或者每个服务器 20 个 cookies，或者超过 4kb 的 cookies。通常情况下，浏览器允许超过 300 的 cookies，不过 4kb 的大小还是被某些浏览器限制了。

##### 保存 cookies

若想要保存一个短暂的 cookie 值，我们可以直接为当前 document 设置 cookie 属性为一个 name=value 的字符串：

```js
document.cookie = `version=${encodeURIComponent(document.lastModified)}`;
```

这样一来，下一次访问 cookie 属性时，该 name=value pair 就会出现。cookie 的值不能包含分号，逗号，空格，所以我们通常会调用 JavaScript 全局函数来加密 encodeURIComponent()，并在读取时用 decodeURIComponent() 解密。

这样的 cookie 由于没有特性的加持，所以其寿命和浏览器一样。不过我们可以这样设置其特性：

```js
let cookie = `${name}=${encodeURIComponent(value)}; max-age=${3600}`; // 持续一小时的 cookie
```

对于设置其他的特性，方法也是一样的。若想要修改 cookie 值，我们只需要为同名的 cookie 重新赋值即可（path 和 domain 特性也需要相同才行）。想要删除 cookie 只需要在赋值时加上 max-age=0 特性即可。

##### IndexedDB

IndexedDB 是一个可以储存对象的数据库，通过 JavaScript 的 API，我们可以将 JavaScript 对象储存长久地到用户的电脑中。

和 SQL 不同，IndexedDB 是一个对象数据库，而非关系型数据库，所以查询语句会简单很多。相较于只能储存 key/value pairs 的 localStorage 来说，IndexedDB 也强大了许多。

IndexedDB 的作用域也是当前 document 的源头，每一个源头都可以拥有任意数量的 IndexedDB 数据库，只要它们的名字都是不同的即可。在 IndexedDB API 中，数据库是 object stores 的集合，而 object stores 正如其名，可以储存对象。对象会通过 structured clone algorithm（DOM 的部分介绍过）解析进入 object stores，所以也可以储存 Maps，Sets，typed arrays 等。

每一个储存的对象必须拥有一个 key，而且该 key 必须具有课排序的特性（key 可以是数字，字符串，Date 对象），IndexedDB 也可以为每一个对象自动生成独特的 key，不过通常我们传入的对象都会拥有一个用于表示 key 的属性，我们可以为该属性指定 key path。key apth 是一个用于告诉数据库从哪里取得 key 值的一个值。

除去通过主要 key 值来查询对象外，我们也可以定义次要 key 值用于查询。我们可以在 object store 上定义任意数量的索引，每一个索引都为储存的对象定义了次要的 key 值。这些索引并不一定是独特的，所以可能有多个对象匹配同一个 key。

理论上来说，IndexedDB 的 API 都是十分简单的。若想要查询数据库，我们只需要：

- 首先，通过名字来打开指定数据库
- 其次，创建一个 transaction 对象，并用它来查询数据库中 - 通过名字指定的 object store 
- 最后，通过 object store 上的 get(), put(), add() 方法来查询，修改对象

如果想要通过一个区间的主要 key 值来查询的话，我们可以创建一个 IDBRange 对象并且指定区间的上下限，然后传入 object store 上的 getAll() 或者 openCursor() 方法中。

如果想要通过次要 key 来查找的话，我们可以在 get(), getAll(), openCursor() 方法中传入该 key 或者 IDBRange 对象。

IndexedDB API 是异步的，不过它出现在 Promise 之前，所以无法对其使用 async 和 await 关键字。这些异步方法会立即返回一个 request 对象，在 request 成功或失败时会触发对应的 success 和 error 事件。

### Worker 线程

JavaScript 最基本的特性之一就是单线程：它不会在同一时间运行两个事件处理函数。所以对于状态的 concurrent 更新是不可能的。不过单线程的弊端就是客户端的 JavaScript 代码不能运行太久，不然它会阻止页面响应。这也是 fetch() 之类的 API 被设计为异步的原因。

浏览器可以通过 Worker 类来稍稍放松该单线程的机制：该类的实例表示了一个可以和主线程同时运行的线程。Workers 会存在于一个 self-contained 的执行环境下，它的全局对象是完全独立的，所以无法访问 Window 和 Document 对象。Worrkerss 只能通过异步的信息传输来和主线程进行交流，这意味着对 DOM 的异步修改依然时不可能的，不过我们可以通过它来执行长时间的函数。

虽然创建新的 worker 并不如创建新的浏览器创建一样复杂，但是也不轻松。所以对于不重要的事件，我们也没理由创建新的 workers。对于复杂的 web app，我们可能会为其创建几十个 workers，但通常不会创建几百到几千个。

比如我们想要创建一个线上的 code editor 并实现语法高亮功能，我们就会希望将语法高亮放入 worker 线程中，因为若其在主线程中，很有可能在解析代码时会阻塞按键输入的事件处理函数。

对于任何线程 API 而言，它由两部分组成，第一个是 Worker 对象，第二个是 WorkerGlobalScope，即新线程的全局对象。

#### Worker 对象

我们可以使用 Worker() 构造函数来创建一个新的 worker，我们只需要传入希望它执行的代码的 URL 即可，除此之外，我们也可以传入一个对象作为第二个 argument 用于描述该 Worker 对象：

```js
let timer = new Worker("utils/timer.js");
```

在创建完 Worker 对象之后，我们可以使用 postMessage() 方法来向其发送数据：

```js
timer.postMessage(time);
```

数据传输是通过 structured clone algorithm 实现的，所以 postMessage 的 argument 也可以是对象，Map，typed arrays 等。

我们可以创建一个 Worker 对象上 message 事件的处理函数用于接收数据：

```js
timer.onmessage = function(e) { // 或使用 addEventListener() 
  console.log(e.data); // 事件的 data 属性就是接收到的信息
}
```

Worker 对象上还有一个方法 terminate()，可以用于提前终止该 worker 线程。

#### Workers 中的全局对象

在创建完 worker 之后，它会在一个新的，且独立的 JavaScript 执行环境下执行代码。而该环境下的全局对象为 WorkerGlobalScope 对象。WorkerGlobalScope 对象上也拥有全局函数 postMessage() 以及全局变量 onmessage 事件处理属性：

- 在 worker 中调用 postMessage() 会向外发送信息
- 在 worker 中的 onmessage 会在接收到信息时触发

WorkerGlobalScope 对象上的 close() 函数可以在 worker 中终止它自己。

因为 WorkerGlobalScope 是 workers 的全局对象，他拥有所有 JavaScript core 的属性，比如 JSON 对象，Date() 构造函数，isNaN() 函数等。除此之外，它还拥有以下 Window 对象的属性：

- self：指向全局对象本身，即 WorkerGlobalScope 对象，而非 Window 对象
- setTimeout(), clearTimeout(), setInterval(), clearInterval() 计数方法
- location：是一个只读 location 对象，它描述了 Worker() 构造函数接收的 URL
- navigator：指向了一个类似于 window 中 Navigator 对象的对象
- addEventListener() 和 removeEventListener() 事件处理函数
- Console 对象
- fetch() 函数
- IndexedDB API

除此之外，它还拥有 Worker() 构造函数，这意味着我们可以在 worker 线程中创建新的 workers。

#### 在 Worker 中 import 代码

因为 worker 是在模块系统引入前定义的，所以它拥有它独特的 import 系统：

```js
// 在 worker 中 import 其他代码，同步且顺序的执行。若某一代码抛出异常，则之后的的代码不会继续执行
importScripts("utils/Histogram.js", "utils/BitSet.js");
```

#### Worker 的信息传输

Worker 对象上的 postMessage() 以及全局的 postMessage() 函数，它们在被调用时会自动创建一对 MessagePort 对象。客户端 JavaScript 无法直接访问自动创建的 MessagePort，不过我们
可以使用构造函数 MessageChannel() 来手动创建一对 MessagePort 对象：

```js
let channel = new MessageChannel; // 创建 MessageChannel
let port1 = channel.port1; // 它的属性指向了 MessagePort 对象
let port2 = channel.port2;
// 两个 port 可以互相沟通
port1.postMessage("Can you hear me?"); 
port2.onmessage = ((e) => console.log(e.data));
```