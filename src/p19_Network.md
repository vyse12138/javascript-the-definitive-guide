### 网络（Networking）

每当浏览器加载页面时，它便会发送 HTTP 请求，用于加载 HTML 文件，以及于其关联的图片，字体，CSS，JavaScript 等文件。除此之外，JavaScript 中也暴露了关于网络的 APIs。

#### fetch() 

对于基础的 HTTP 请求来说，我们可以使用 fetch() 三部曲：

1. 调用 fetch()，传入 URL 作为 argument
2. 异步的取得响应对象，并在响应开始抵达时，调用该对象上的方法去得到响应体
3. 异步的取得上一步的取得响应体对象，并且进行操作

fetch() API 是完全基于 Promises 的，并且其中有两个异步的步骤。所以在使用 fetch() 时，通常会有两个 then() 或两个 await。

下面的例子就是最基本的 fetch() 用法，期待服务器返回一个 JSON 格式的响应：

```js
fetch('/api/data') // 发送 HTTP 请求
  .then(response => response.json()) // 将响应解析成为 JSON 对象
  .then((data)=> { // 对解析完的对象进行操作
    console.log(data);
  }
```

下面则是是由 async 和 await 关键字来实现：

```js
async function getData() {
  let response = await fetch('/api/data');
  let data = await response.json();
  return data;
}
```

fetch() API 的问世替代了 XMLHttpRequest（XHR） API。虽然我们依然可以在已有的代码中见到它，但如今并没有必要再去使用它了。

##### HTTP 状态码，响应头以及网络异常

上面展示的 fetch() 的用法并没有包含异常处理机制，以下是更为真实的例子：

```js
fetch('/api/data/') // 发送 HTTP 请求
  .then(response => {
    // 判断响应状态和类型是否符合
    if (response.ok && response.headers.get('Content-Type') === 'application/json') {
      return response.json(); // 将响应解析成为 JSON 对象
    } else {
      // 若不符合则抛出异常
      throw new Error('Unexpected response stateus of content type');
    }
  })
  .then(data => { // 对解析完的对象进行操作
    /* 进行操作... */
  })
  .catch(error => { // 捕获 Promises chain 中的任何异常
    console.log('There is an error: ',error);
  })
```

fetch() 返回的 Promise 是一个 Response 对象，所以他包含了 status 属性指定了 HTTP 状态码，如成功的 200 和 Not Found 的 404。或者我们可以直接使用 ok 属性，它会在 status 值为 200 到 299 之间为 true。

fetch() 会在响应开始抵达时，即在状态码和响应头抵达时，就将 Promise resolves。Response 对象上的 headers 属性指向了 Header 对象。我们可以使用其 get() 或者 has() 方法等用于取得其响应头的内容。它其实也是一个可遍历对象，可以用 for of 来取得其 \[name, value\]。

只要 fetch() 取得了响应，哪怕其状态码是 404 Not Found Error 或者 500 Internal Server Error 等，它都会返回一个 fulfilled Promise，持有 Response 对象。只有在完全联系不到服务器时，才会返回 rejected Promise（比如服务器下线，hostname 不存在等）。

##### 设置请求头

我们可以用过在 URL 后的 ? 后增加 name/value pair 来传入额外参数。而某些时候，我们则需要自定义请求头。我们可以在 fetch() 中传入第二个 argument 来实现。

fetch() 的第一个 argument 还是字符串或者 URL 对象表示的 URL，而第二个 argument 则是一个用于提供额外信息的 Options 对象，我们可以在该对象上添加 headers 属性用于自定义 header：

```js
let customHeaders = new Headers(); // 创建 Header 对象
customHeaders.set("Authorization", "Basic ABCDEF12345"); // 增加请求头内容
fetch("/api/data/", { headers: customHeaders }) // 自定义请求头
  .then() // .....
```

或者可以创建自定义 Request 对象来实现：

``` js
let request = new Request(url, { headers });
fetch(request).then(response => ...);
```

除了设置请求头之外，第二个对象还可以用于设置许多其他内容，它们会在接下来被介绍。

##### 解析响应体

还记得前面的例子中用到的 json() 和 text() 方法吗，它们就是用于解析响应体的方法，分别解析成为 Json 对象和字符串。但其实除了它之外还有以下用于解析响应体的方法：

**arrayBuffer()**

这个方法会返回一个在 resolved 后会成为 ArrayBuffer 的 Promise。我们可以用它来解析二进制数据，然后用 ArrayBuffer 来创建 typed array 或者 DataView 对象。

**blob()**

这个方法会返回一个在 resolved 后会成为 Blob 对象的 Promise。Blob（Binary Large Object）通常会用于处理许多二进制数据。如果我们想要将响应体转换成 Blob，浏览器会将响应体 stream 到一个暂时的文件中，然后返回表示该文件的 Blob 对象。

**formData()**

这个方法会返回一个在 resolved 后会成为 FormData 对象的 Promise。如果响应体是以 multipart/form-data 的形式 encoded 的话，我们可以使用这个方法。通常该形式是 post 到服务器的格式，在响应的情况下该格式很少见。

##### Streaming 响应体

除去以上五种异步解析方法之外，我们也可以 stream（流传输）响应体。这个方法通常会在处理一个很大块的 response body 中用到。stream 也可以在我们希望展示进度条时被用到。

Response 对象上的 body 属性就是一个 ReadableStream 对象。如果我们已经调用过如 json() 的解析方法的话，Response 对象上的 bodyUsed 属性就会被设置为 true。我们只有在其为 false 时，才能使用 response.body 对象上的 getReader() 用于取得 stream reader 对象，然后使用 read() 方法来异步的读取其中的数据。read() 的返回值是一个会被 resolved 成为一个对象的 Promise，该对象拥有 done 和 value 属性，done 会在整个 body 都被读取完，或者 stream 被关闭时返回 true。value 则是表示下一块代码的 Unit8Array，或者在没有更多代码时返回 undefined。  

##### 指定请求方法

还记得前面提到的 Options 对象吗？我们可以用它来定义除了 GET以 外不同的请求方法（如 POST，PUT，DELETE）。

```js
fetch(url, { method: "POST" }) // 定义为 POST 方法
  .then( ... );
```

而 POST 和 PUT 请求通常还会包含数据作为请求体，我们也可以在 Options 对象设置：

```js
fetch(url, { 
  method: "POST"，
  body： data // 设定请求体
}) 
  .then( ... );
```

在我们设定了请求体知乎，浏览器会自动增加 Content-Length 请求头。若请求头为字符串，则浏览器还会自动增加 "Content-Type" 请求头，值为 "text/plain; charset=UTF-8"。或者说我们也可以在 Options 中手动覆写它们。

对于 POST 请求时，通常会为请求体传入 name/value pairs（而非使用 URL 的查询部分），这可以通过两种方法来实现：

- 我们可以 URLSearchParams 构造函数来定义定义 name/value pairs，然后直接传入 Options 对象的 body 属性中。如果我们这么做，body 看上去就会和 URL 的查询部分一样，并且会自动将 "Content-Type" 设置成 "application/x-www-formurlencoded; charset=UTF-8"。

- 或者使用 FormData 构造函数来定义，这样的话 "Content-Type" 就会被设置为 "multipart/formdata;  boundary=..."，boundary 字符串也会指向请求体对应的 boundary。

##### 跨域请求

大多数情况下 fetch() 都是用于请求同源的数据。出于安全原因，浏览器通常不允许进行跨域请求（图片和 scripts 除外）。但是 CORS（Cross-Origin Resource Sharing）可以使我们安全的进行跨域请求。

在使用 fetch() 进行跨域请求时，浏览器会自动为请求加上 "Origin" 请求头来告诉服务器该请求的源头（Origin 属性无法被手动通过 headers 属性修改）。如果浏览器的响应拥有对应的 "Access-Control-Allow-Origin" 请求头的话，该请求依然可以继续正常返回。

##### 终止请求

我们可以通过 AbortController 和 AbortSignal 类来实现提前终止请求的功能。如果想要添加提前终止的选项，我们可以在请求前先定义 AbortController 对象。然后为其定义 signal 属性，指向 AbortSignal 对象。然后只要将 Opinions 对象下的 signal 属性设置为 AbortSignal 对象即可。

这样的话，我们就可以通过 AbortController 对象的 abort() 方法来提前终止请求，使得任何于 fetch 相关的 Promises 对象进入 rejected 状态：

```js
// 如果 options 中有 timeout 属性，则在 timeout 时间之后终止请求
function fetchWithTimeout(url, options={}) {
  if (options.timeout) {
    let controller = new AbourtController();
    options.signal = controller.signal;
    setTimeout(() => { controller.abort(); }, options.timeout)；
  }
  return fetch(url, options);
}
```

##### 其他的请求选项

Options 除去上述的属性外，还支持以下属性：

**cache**

可以使用该方法覆写浏览器默认的缓存机制，它可以拥有以下的字符串作为值：

- default：浏览器默认缓存机制。缓存中新的响应会被直接取得，旧的响应则会在取得前先进行验证
- no-store：使浏览器在请求时忽略缓存，在响应抵达时也不会进行更新
- reload：使浏览器在请求时忽略缓存，不过在响应抵达时，依然会被存于缓存
- no-cache：名字具有误导性，它的实际作用是使浏览器在取得缓存前一定会进行验证
- force-cache：只要有匹配的缓存就直接读取，不论有多陈旧

**redirct**

该属性指定了浏览器如何处理服务器上重定向（redirect）的响应，它可以拥有以下的字符串作为值：

- follow - 这是默认值，使浏览器自动跟随重定向的响应。在这种情况下，fetch() 的响应不会拥有 300 到 399 的状态码
- error：这使得 fetch() 在服务器返回重定向响应时返回 reject Promise
- manual：手动处理响应

**referrer**

该属性可以设置 HTTP Referer 请求头的值（历史遗留拼写错误，少了一个 r）。可以是包含了相对 URL 的字符串，或者为空字符串用于忽略 Referer 请求头。

#### Server-Sent 事件

在 HTTP 协议中，最为基本的功能就是由客户端向服务器发送请求，然后由服务器响应该请求。不过某些时候我们可能会需要在某些事件下，让服务器来为客户端发送数据。但这并不是在原生 HTTP 协议中的，不过我们可以通过以下技术来实现：

- 客户端向服务器发送一个请求，不过不关闭该连接（connection）
- 在服务器想要传回数据时，它便为连接写入数据，不过依然不会关闭该连接
- 通常这样的连接也不会一直持续，不过客户端在察觉到连接断开时可以重新发送一个请求然后建立连接

该技术对于从服务器发送数据到客户端而言是时十分高效的，不过对于服务器端的代价会比较大，这是因为需要为所有活跃的客户端建立连接。

JavaScript 中拥有 EventSource API 来创建该类连接。我们只需要在 EventSource() 构造函数中传入 URL 即可。在服务器向连接中写入数据时，会触发一个在 EventSoucrce 对象上可以被监听到的事件：

```js
let ticker = new EventSource("/data");
ticker.addEventListener("tick", (event) => {
  console.log(event.data);
}
```

对于该类事件而言，于其关联的 Event 对象拥有一个 data 属性，持有了该事件下服务器传回的字符串；也拥有一个 type 属性指定了事件类型（事件名），默认为 message；事件也可以拥有 ID，这使得重新连接的客户端可以通过 ID 来告知服务器上一次其接收的事件。

Server-Sent 事件的协议十分简单，在客户端建立连接后，即创建 EventSource 对象之后，服务器会保持该连接。在事件触发时，服务器会为连接写入文本数据。

Server-Sent 事件常用于多人聊天软件之类的功能。客户端可以通过 fetch() 来在聊天室中发送消息以及建立于服务器的连接。

#### WebSockets

WebSocket API 是一个用法更为简单的网络协议接口。它使我们可以通过 JavaScript 来与服务器交换文本和二进制数据。和 Server-Sent 事件不同，它也可以传输二进制文件，并且信息可以双向传输。

支持 WebSocket 的网络协议更像是一个 HTTP 协议的 extension。WebSocket API 的接口是一个 URL，不过是使用 WebSocket 协议的，即用 wss:\/\/ 开头而非 https:\/\/。

建立 WebSocket 连接的第一步就是建立一个 HTTP 连接，然后向服务器发送一个 Upgrade: wbesocket 的请求头用于将 HTTP 协议转换成 WebSocket 协议。但如果服务器不支持 WebSocket 协议的话，我们也可以继续使用 Server-Sent 事件的。

##### 与 WebSocket 建立连接

我们可以通过调用 WebSocket 构造函数并以 wss:\/\/URL 的格式传入 argument 来建立与 WebSocket-enabled 服务器的连接：

```js
let socket = new WebSocket('wss://example.com');
```

WebSocket 对象的 readyState 表示了当前连接状态，它的值可能是：

- **WebSocket.CONNECTING：** 该 WebSocket 正在连接
- **WebSocket.OPEN：** 该 WebSocket 已经连接并且可以开始通信
- **WebSocket.CLOSING：** 该 WebSocket 连接正在被断开
- **WebSocket.CLOSED：** 该 WebSocket 连接已断开，无法建立更多通信。在初始化连接异常时也会进入该状态

在 WebSocket 从 CONNECTING 状态进入 OPEN 状态时，会触发 open 事件；在任何错误发生时，会触发 error 事件；进入 CLOSED 状态时会触发 close 事件。我们。可以对这些事件进行监听。


##### 通过 WebSocket 收发消息

若想要向 WebSocket 连接的服务器发送消息的话，我们只需要调用 WebSocket 对象身上的 send() 方即可法。该方法可以接收以下的某一个 argument 作为其消息：String，Blob，ArrayBuffer，typed array 或者 DataView。

若想要从 WebSocket 连接的服务器接收消息的话，我们则需要为 message 事件增加一个处理函数即可。与 message 事件关联的 Event 对象是一个 MessageEvent 实例，拥有 data 属性指向了服务器返回的消息。如果服务器返回了 UTF-8 编码的文本，则 event.data 就会是字符串；若返回了二进制数据，则 data 属性默认会是一个表示数据的 Blob 对象。