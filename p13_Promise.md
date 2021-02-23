## 异步 JavaScript（Asynchronous JavaScript）

虽然说大部分 JavaScript 的代码都是同步执行的，但其实也会有许多需要异步执行的地方。这一章会介绍 JavaScript 对于异步编程的简化的一些特性。如在 ES6 中新增的 Promises，ES2017 中新增的 Async 和 await 关键字，以及 ES2018 中新增得 for await 循环等。

### 使用回调函数进行异步编程（Asynchronous Programming with Callbacks）

从最基本的角度考虑，JavaScript 的异步编程是通过回调函数（callbaks）完成的。一个回调函数是一个被传入另一个函数的函数，那另一个函数会在某个异步事件发生时调用我们传入的回调函数。

#### 计时器（Timers）

最简单的异步就是定义一个我们希望在一段时间以后再运行的代码，我们可以使用 setTimeout() 或者 setInterval() 函数来实现：

```js
setTimeout(f1, 1000);
setInterval(f1, 1000);
```

就比如上面的例子，第一个 argument 是一个函数，而第二个则是以毫秒计时的时间。所以 f1 会在一秒以后被调用（或重复调用）。

#### 事件（Events）

客户端 JavaScript 的程序基本上都是通过事件驱使（event driven）的：

相较于运行提前指定的运算，JavaScript 更多情况下会等到用户进行了操作以后再运行。浏览器会在用户按下键盘，移动鼠标或点击时生成一个事件，然后每当事件发生时，我们就可以调用其对应的回调函数。这些回调函数被称为 event handlers/listeners，可以通过 addEventListener() 来注册：

```js
let button1 = document.querySelector('#button1'); // 返回一个代表了网页中的一个元素的对象
button1.addEventListener('click', f1); // 为其添加 click 事件下的回调函数
```

上面的例子红，回调函数 f1 会在每次 button1 被点击时调用。

#### 网络事件（Network Events）

另一个异步编程经常出现的时机就是网络请求了，浏览器中的 JavaScript 可以这样来异步 fetch 数据：

```js
function getName(nameCallback) { // 传入回调函数
  // 定义一个异步的 http request
  let request = new XMLHttpRequest();
  request.open("GET", "http://www.example.com/api/name");
  request.send();
  
  注册一个回调函数，在接收到 response 时调用
  request.onload = function() {
    if (request.status === 200 ) {
      let name = request.responseText; // 取得 response 数据
      nameCallback(name); // 调用回调函数
    }
  }
}
```

客户端可以使用 XMLHttpRequest 类和回调函数来发送 HTTP requests，然后异步的处理 response（当其抵达后）。

虽然上面的例子没有使用 addEventListener() 来注册回调函数，但其实我们也可以这么做。只不过这次我们直接定义了其 onload 属性的回调函数，即等同于 request.addEventListener('load'，f1)。

### Promises

在介绍完最基础的异步编程之后，我们要介绍一下 Promises 了。 Promise 是一个表示了异步运算的结果的对象。这个结果可能是准备好的，也可能不是。特意模糊该结果的原因是不希望 JavaScript 通过同步的方法来取得 Promise 的值。我们只能定义一个在值准备好之后调用的回调函数。

虽然说 Promise 只不过是另一种使用回调函数的方法，但它其实还有一些优势：对于嵌套回调函数时，它会不停的缩进，使代码难以阅读，而使用 Promise 可以被链式调用，这使得代码更具有可读性；Promise 也标准化了异步编程的异常处理机制，也使得 error 可以正确在链式调用的 Promise 中传播。

注意 Promise 表示了单一异步运算的结果，它们不能表示重复的异步运算的结果。对于前面的 setTimeout() 和网络事件部分，我们都可以用 Promise 来替代，但是对于按钮的点击事件和 setInterval()，我们则不能用 Promise 来替代，因外它们会被多次调用。

虽然 Primises 看上去不难，对于它的基本使用也不难，但它其实很容易变得复杂。

#### 使用 Promises

在 Promises 被加入 JavaScript 后，浏览器就开始使用 Promise-based APIs 了。比如：

```js
getData(url).then(data => {
  // 这是回调函数
});
```
getData() 会发送一个异步的 HTTP request，在 request 在待定时，返回一个 Promise 对象。Promise 对象上拥有一个 then() 方法，我们在其中传入一个回调函数，在 HTTP response 抵达时，该回调函数会被调用。 

then() 方法虽然类似于 event listener，不过每个 then() 中的回调函数之会被调用一次。该回调函数也是异步调用的。

通常情况下，我们会以动词的形式命名会返回 Promise 的函数，以及使用 Promise 结果的函数。这使得代码更容易阅读：

```js
function showData(data) {...}; // 回调函数
getData（'www.example.com').then(showData); // 几乎等同于英文，易于阅读
```

#### 使用 Promises 处理异常

异步函数，尤其是包含网络的部分，理论上来说有很多种失败的情况，我们可以在 Promise 的 then() 方法中传入第二个函数用于处理错误：

```js
getData（'www.example.com').then(showData, handleDataError);
```

在 Promise-based 的异步运算中，如果运算成功完成了，它会将其 result 传入 then() 的第一个函数作为 argument；但如果运算失败了，它会抛出一个异常，并传入 then() 的第二个函数作为 argument。

但在大多数情况下，我们不会为 then() 传入两个函数，而是：

```js
getData（'www.example.com').then(showData).catch(handleDataError);
```

catch() 其实等同于调用了 then()，不过传入了 null 作为 then() 的第一个 argument。在此之上，catch() 也可以捕获 showData 函数中抛出的异常。


一个 Promise 必然处于一下状态之一：
- 待定（pending）
- 已兑现（fulfilled）
- 已拒绝（rejected）

假设我们已经调用了一个 Promise 的 then() 方法并且传入了两个回调函数时：
- 若第一个回调函数被调用，该 Promise 的状态便会改成 fulfilled
- 若第二个回调函数被调用，该 Promise 的状态便会改成 rejected
- 若一个 Promise 既不是 fulfilled 或者 rejected，则称其为待定
- 只要 Promise 进入 fulfilled 或者 rejected 状态，我们称它为已敲定（settled）
- settled 的 Promise 不会再改变其状态
- 一个 Promise 不会同时在 fulfilled 和 rejected 状态之下
- Promise 也可能处于已决议（resolved）的状态下，这个状态和 fulfilled 或者 rejected 都不同


#### 链式调用 Promise

Promise 带来的另一个好处就是使得表达一列异步操作更为自然，使它们不用内嵌在上一个回调函数中。

在调用一链 then() 方法时，我们不再同一个 Promise 对象上定义多个回调函数，而是每次调用 then() 时返回一个新的 Promise 对象。

#### Resolving Promise

正如前面提到的，Promise 也可能处在 resolved 的状态。我们来看下面的例子：

```js
function c1(response) { // 回调函数 1
  let p4 = response.json();
  retun p4; // 返回 promise 4
}

function c2(data) { // 回调函数 2
  showData(data);
}

let p1 = fetch('/api/data'); // promise 1, task 1
let p2 = p1.then(c1); // promise 2, task 2
let p3 = p2.then(c2); // promise 3, task 3
```

如果想要让链式调用正确的执行，task 2 的输出就必须成为 task 3 的输入（即一个 Promise 对象），而这个例子中 task 3 的输入时 fetched 到的 URL body，并被解析成了 JSON 对象。但是回调函数 c1 的返回值并不是一个 JSON 对象，而是 JSON 对象的 Promise p4。这看上去很矛盾，但其实不然：

在 p1 兑现时，c1 被调用，然后 task 2 开始执行。在 p2 兑现时，c2 被调用，然后 task 3 开始执行。虽然 task 在 c1 被调用时就开始执行了，但这并不意味着 task 2 在 c1 返回时必须结束。这意味着如果 task 2 是异步执行的，这个任务并不会在回调函数返回之前就执行完毕。

在看完上面的例子后，我们来讲一下 Promise 的 resolve 状态：

在传入回调函数 c 进入 then() 方法时，then() 返回了一个 Promise p 并且安排 c 在一段时间以后被异步调用。然后 c 执行了运算并返回了一个值 v，在回调函数返回时，p 就进入了 resolved 的状态并且持有值 v。当一个 Promise 进入 resolved 的状态并且持有一个值，且该值 v 不是 Promise 时，它会立刻进入 fulfilled 的状态，并且持有那个值。

所以说，如果 c 返回了的值 v 是 non-Promise，那个返回值就是 p 的值，且 p 立刻成为 fulfilled，对于这个 Promise 的处理就结束了；但是如果 c 返回的值 v 是一个 Promise 的话，p 就被称为 resolved 而非 fulfilled。在这个状态时，除非 Promise v settles（落定），否则 p 就不能 settles。当 Promise v fulfilled 时，p 也会成为 fulfilled 并且持有和 Promise v 一样的值；如果 Promise v rejected，p 也会因为同样的原因 rejected。

上面就解释了 Promise 的 resolve 状态是什么：即一个 Promise 与另一个 Promise 关联的状态。我们不知道 p 会成为 fulfilled 还是 rejected，不过回调函数 c 对其已经没有操纵权了，p 的命运已经全部交给了 Promise v。

这时候我们再回到刚刚的 URL fetch 的例子中，当 c1 返回 p4 时，p2 就会 resolved，但是 resolved 不等于 fulfilled，所以 task 3 还并没有开始。只有等到 p4 fulfilled 时，即 HTTP body 中的 .json() 方法也被 fetch 到然后解析出 p4 的值时，p2 才会自动变成 fulfilled。只有这个时候 task 3 才会开始。

#### Promises 和 Error

正如在前面提到的，我们可以在 then() 中传入第二个函数，用于在 Promise rejected 时调用。在第二个调用时，其 argument 会是一个表示错误发生的原因的值（通常是一个 Error 对象）。也提到了使用 catch() 来代替第二个函数。在了解了链式调用之后，我们可以更进一步的了解错误处理机制了。

对于异步编程的异常处理会变得更为复杂，因为不像同步变成在错误发生时会提供详细的 stack trace。而在很多时候都是静默发生的错误。

**catch() 和 finally() 方法**

正如前面提到的，以下两种方法是一样的：

```js
p.then(null, c); // c 为回调函数
p.catch(c);
```
在链式 Promise 中，catch() 不仅可以被加载最后，其实也可以被加在中间：

```js
doAsyncThing()
  .then(do1)
  .catch(handleDo1Error) // 若 doAsyncThing 或 do1 中抛出 error 则会处理它，否则这一行被忽略
  .then(do2)
  .then(do3)
  .catch(cleanUp);
```

除去 catch() 方法，在 ES2018 中，新增了 finally() 方法，其使用方法类似于 try/catch/finally 语句。如果在链式 Promise 中增加了 finally() 的调用，则 finally() 中的回调函数会在调用其的 Promise settle 时调用。即无论 rejected 或者 fulfilled 都会调用，且不会传入 argument。

#### 同时执行多个 Promises

如果我们想要同时执行多个异步操作时，函数 Promise.all() 可以帮我们做到。Promise.all() 接受一个数组作为 argument，数组中应当包含为 Promise 对象，然后该函数会返回一个 Promise。返回的 Promise 会在任意一个输入的 Promises rejected 错误 rejected；不然的话，即所有输入的 Promises 都 fulfilled，返回的 Promise 也会成为 fulfilled。

比如我们要从多个 URLs 同时 fetch 数据时，我们可以：

```js
let urls = [/*URLs组成的数组*/];
urlsPromises = urls.map(url => fetch(url).then(r => r.text())); // 转换成 array of Promises
Promise.all(urlsPromises).then(/*所有 URLs fetch 成功*/).catch(/*任意一个 fetch 失败*/);
```

其实 Promise.all() 还可以更简单一些，其输入的数组中也可以包括非 Promise 对象的值，它们会被当做已经 fulfilled 的 Promise 持有的值，并且会不改变它们，直接复制到输出数组中。

Promise.all() 在 rejected 时的返回值就是任何 rejected 的输入 Promise。这会在第一个 rejected Promise 出现时发生，即使还有别的 Promise 依然处于 pending 的状态下。

在 ES2020 中，新增了 Promise.addSettled()，他和 Promise.all() 的输入和输出格式一样，只不过它的返回值绝对不会是 rejected，但是在所有的 Promises 都 settled 之前，不会进入 fulfilled 状态。这个返回值 Promise 会先进入 array of objects 的 resolved 状态，数组中的每一个对象对应一个输入的 Promise。每一个返回的对象拥有一个 status 属性。若该属性为 fulfilled，则这个对象还会拥有一个 value 属性表示其对应的 Promise 的值；若为 rejected，则这个对象回有一个 reason 属性，即其对应的 Promise rejected 的原因。

```js
Promise.allSettled([Promise.resolve(1), Promise.reject(2),3])
  .then(results => {
  results[0] // { status: "fulfilled", value: 1 }
  results[1] // { status: "rejected", reason: 2 }
  results[2] // { status: "fulfilled", value: 3 }
});
```

我们还可以使用 Promise.race() 来代替 Promise.all()，它会返回其输入数组中第一个 settled 的 Promise（若输入数组有非 Promise 值，则其中第一个会被返回）。

#### 创建 Promises

##### 基于其他 Promises 的 Promises

在拥有一个 Promise 时，创建另一个新的 Promise 会十分简单。因为调用 Promise 的 then() 方法会创建并返回一个新的 Promise。

##### 基于同步值的 Promises

使用 Promise.resolve() 和 Promise.reject() 可以直接通过传入的 argument 来创建新的 Promise。Promise.resolve() 接收一个值作为 argument，并且立刻（不过是异步的）返回一个 fulfilled 的 Promise，并持有传入的那个值；Promise.reject() 接收一个 argument，然后返回一个 rejected 的 Promise，argument 成为 rejected 的原因。（它们被返回时并不是 fulfilled 或者 rejected，而是处在 pending 的状态，然后等待当前代码执行完毕，才进入 settled 状态，通常只有几毫秒)

我们在前面提到过，resolved 的 Promise 并不是 fulfilled 的 Promise。所以我们在 Promise.resolve() 中传入一个 Promise 作为 argument 时，其返回的 Promise 只有等到传入的 Promise settled 之后才会 settled

##### 从头开始创建 Promises

在不适用另一个 Promise 的时候，我们可以使用 Promise() 构造函数来创建 Promise。它接收一个函数作为其唯一的 argument，这个函数应该接收两个 arguments，通常将第一个 argument 命名为 resolve，将第二个命名为 reject。构造函数会同步的调用我们定义的函数，然后返回一个新建的 Promise。返回的 Promise 可以通过我们定义的函数来操纵，那个函数应当执行一些异步的操作，然后通过 resolve 函数来 resolve 或 fulfill 返回的 Promise，或者 reject 函数来 reject 返回的 Promise。

```js
function wait(duration) { 
  return new Promise((resolve, reject) => {  // 自定义 Promise
    if (duration < 0) { // 若 argument 小于零，Promise 成为 rejected
      reject(new Error());
    }
    setTimeout(resolve(duration), duration); // 否则等待一段时间后 Promise 被 resolve 成 fulfilled
  });
}
```

注意，Promise 构造函数中函数的 argument 名为 resolve() 和 reject() 而非 fulfill() 和 reject()。所以对 resolve() 传入一个 Promise 的情况下，它会被 resolve 成为新的 Promise。


### aysnc 和 await

ES2017 新增了两个新的关键字 async 和 await。它们代表了对于异步 JavaScript 编程的模式转换。这两个新的关键字大大减少了 Promises 的复杂程度，使我们可以如同写同步代码一样写异步基于 Promises 的代码。

#### await 表达式

await 关键字会接受一个 Promise，并将其转换成一个返回值或者抛出的异常。比如说有一个 Promise 对象 p，表达式 await p 会等待 p settles。若 p 为 fulfilled，则 await p 的值就是 p fulfilled 的值；若 p 为 rejected，则 await p 会抛出 p rejected 的原因。我们通常会在调用一个会返回 Promise 对象的函数前加上 await：

```js
let response = await fetch('/api/data');
```

await 关键字并不会阻塞程序，而且在它的 Promise settled 之前不会做任何事。

#### async 函数

因为任何使用 await 关键字的代码都是异步的，所以这有一个很重要的规则：我们只能在异步函数中使用 await 关键字，即拥有 async 关键字的函数中：

```js
async function getData() {
  let response = await fetch('/api/data');
  return response;
}
```

用 async 关键字来声明的函数意味着它的返回值会成为一个 Promise，即使在函数体中并没有直接出现关于 Promise 的代码。

### 异步迭代（ Asynchronous Iteration）

在前面我们提到过，Promise 无法解决 setInterval()，或者 click event 等会多次触发的异步操作，所以在 ES2018 中，加入了异步迭代器，它类似于前面介绍的迭代器，只不过是基于 Promise，并且使用 for await 改变了 for of 的形式。

#### for await 循环

如同于一个普通的 await 表达式，for await 循环也是基于 Promise 的，总的来说，for await 循环会等到所有的 Promises 都成为 fulfilled 之后，会为为循环变量定义值，最后再进行循环：

```js
for await(const response of responses) {
  handle(response);
}
```

#### 异步迭代器

异步迭代器和迭代器有许多相似之处，不过拥有两个很大的不同：

- 异步迭代器通过定义 Symbol.asyncIterator 方法而非 Symbol.iterator 方法来实现
- 异步迭代器对象的 next() 方法会返回一个 Promise，它会 resolve 成为迭代结果对象

#### 异步生成器

异步生成器可以通过再生成器函数前加上 async 关键字来定义。我们可以在异步生成器中使用 yield，只不过 yield 的值会被封装成一个对象。下面的例子实现了一个自定义的 setInterval()：

```js
function elapsedTime(ms) { // 用于返回新 Promise 的函数
  return new Promise(resolve => setTimeout(resolve, ms));
}
async function* clock(interval, max) { // 异步生成器
  for (let count = 1; count <= max; count++ ) {
    await elapsedTime(interval);
    yield count;
  }
}
async function tickClock() { // 只能在 async 函数中调用 await
  for await (let tick of clock(1000,60)) { // 每秒显示一次，一共六十次
    console.log(tick);
  }
}
tickClock(); 
```

### 小结

这一章的要点：

- 大多真实生活中的 JavaScript 程序都是异步的
- 原来，异步函数是通过 event 和回调实函数现的
- 在 ES6 新增的 Promises 给予了回调函数的新的结构，使其更为简洁
- 在 ES2017 新增的 async 和 await 关键字使 Promises-based 异步代码可以以同步代码的外貌编写
- 在 ES2018 新增的 for await 循环使我们可以迭代可异步迭代的对象。我们可以实现对象的 \[Symbol.asyncIterator\] 方法来使其成为可迭代对象