### 事件（events）

客户端的 JavaScript 程序使用了异步的，事件驱使的编程模型。在这种模型之下，浏览器会在某些使其发生时，生成一个特定的事件。因为事件可以在任何 HTML document 中的元素上生成，所以浏览器中的事件会比 Node 中的复杂许多。下面是对事件的一些介绍：

**1. 事件类型（event type）**

event type 字符串指明了事件的种类。比如说类型 'mousemove' 表示了鼠标的移动；'keydown' 表示了用户按下键盘等。因为 event type 是一个字符串，我们通常也将其称为事件名（event name）。

**2. 事件目标（event target）**

这是事件发生在的目标对象。对于所有的事件，都需要定义其类型和目标，比如如 Window（目标）的 load（类型）事件等。若想要注册一个事件处理（event handler)函数，则需要指定其事件类型和目标。只有在指定的事件类型发生在指定的目标上，才会调用该事件处理函数。
    
**3. 事件对象（event object）**

这是一个于和一个事件以及其内容关联的对象，事件对象以 argument 的形式传入事件处理函数。事件对象拥有 tyep 和 target 属性用于指定类型和目标。事件类型又会为其事件对象定义一系列的属性。比如鼠标移动的事件对象会持有鼠标的坐标。而某些事件类型则不会未事件对象定义属性，使得事件对象只拥有 type 和 target 这两个属性。
    
**4. 事件传播（event propagation）**

这是浏览器决定由哪些对象来出发事件处理函数的过程。对于那种指定了单一对象的事件，比如 Window 对象的 'load' 事件，则不需要传播。但是对于某些元素的事件则会在 document tree 向上传播。比如用户将鼠标移动到超链接上，mousemove 事件会现在 \<a> 元素上触发，然后会在包含该链接的 \<p> 元素上触发，然后是包含 \<p> 的 \<div> 上触发等等，一直传递到 Document 对象。事件触发了某个定义的事件处理函数之后，便会停止传播。这是因为事件处理函数会调用事件对象上的一个方法使其停止传播。
	
某事件会拥有默认的与其绑定的 action，比如在点击超链接时会默认使浏览器跟随该链接并且加载新的页面。我们可以在事件处理函数中阻止该默认 action。

#### 事件分类

客户端 JavaScript 支持许许多多的事件类型，这里会对事件类型进行一下分类：

**基于设备的输入事件（Device-dependent input events）**

这一类事件会会直接和输入设备绑定，如鼠标或者键盘，比如：mousedown，mousemove，mouseup，touchstart，touchmove，touchend，keydown，keyup 等。

**不基于设备的输入事件（Device-independent input events）**

这一类输入时间则不会和设备绑定，比如 click 事件，虽然通常使用过鼠标点击，不过也可以通过键盘或者点击触摸屏来完成。比如：pointerdown，pointermove，pointerup 就不会和设备绑定，对于触摸屏，触摸板等也能生效。

**用户界面事件（User interface events）**

UI 事件是高层事件，通常会在 HTML 表单元素出现，包括了 focus，change，submit 等事件。

**状态改变事件（State-change events）**

某些事件则不会在用户交互式触发，而是在网络或者浏览器的状态改变时触发。比如前面提到的 load 和 DOMContentLoaded 事件，会分别在 Window 和 Document 对象上触发。浏览器会在网络连接改变时触发 online 和 offline 事件。浏览器历史管理系统会在按下浏览器 back 按钮后触发 popstate 事件等。

**API 指定事件（API-specific events）**

许多 HTML 中的 web APIs 也拥有其事件。如果 HTML 中的 video 和 audio 元素定义了一系列与它们关联的事件，如 waiting，playing，seeking，volumechange 等等。这使得我们可以自定义媒体的播放。在 Promise 之前，异步的 web APIs 都是事件驱使的，如 IndexedDB 和 XMLHttpRequest API 等。

#### 注册事件处理器

这一共有两种基础方法可以用于注册事件处理器（event handlers）。第一种是为 document 元素或其对应的对象上增加事件处理属性。第二种是使用 addEventListener() 方法来传入处理器。

##### 设置事件处理属性（JavaScript 中）

注册事件处理器最简单方法就是在目标事件上设置一个事件处理函数作为属性。事件处理器属性由前缀 on 加上 事件类型来表示，如 onclick，onmousedown，onload 等（注意全小写）：

```js
window.onload = function() {
  let form = document.querySelector('form#form1');
  form.onsubmit = function(event) {
    console.log(event);
  }
}
```

上面的例子就为 window 注册了 onload 时的事件处理函数，为 form 注册了 onsubmit 时的事件处理函数。
不过通常情况下，addEventListener() 会是一个更好的选择，一因为他不会覆写已注册的处理器。

##### 设置事件处理属性（HTML 中）

我们也可以在 HTML 直接为元素添加事件处理属性，不过现在一般不会这么做了：

```html
<button onclick="console.log('Thank you');">Please Click Here</button>
```

这样注册的事件处理函数函数会被浏览器转换成类似于以下的形式：

```js
function(event) {
  with(document) {
    with(this.form) {
      with(this) {
        console.log(event);
      }
    }
  }
}
```

因为 with 语句已经在严格模式下被遗弃，所以这样定义的事件处理器会出现不可预知的行为，所以说尽量不要在 HTML 中直接写入事件处理函数。

##### ADDEVENTLISTENER()

所有可能成为事件目标的对象（Window，Document 对象以及所有 document 元素）都拥有名为 addEventListener() 的方法。我们可以使用它来注册一个事件处理函数。addEventListener() 接收三个 arguments，第一个 argument 是不包含 on 前缀的，用于表示事件的字符串，如 'click', 'load' 等，第二个 argument 是回调函数，第三个 argument 则是可选的，会在下面被解释。


比如下面的对象为 button 注册了两个 click 事件处理器：

```html
<button id="mybutton">Click me</button>
<script>
  let b = document.querySelector("#mybutton");
  b.onclick = function() { console.log("Thanks for clicking me!"); };
  b.addEventListener("click", () => { console.log("Thanks again!"); });
</script>
```

使用 addEventListener() 新增的处理器不会对已注册的事件处理器造成影响，而是会一起被调用。所以我们可以使用 addEventListener() 对一个目标注册多个事件处理器，不过注册同样 argument 的 addEventListener() 超过一次是无用的，一样的 handler 只会被注册一次。

addEventListener() 方法是和 removeEventListener() 方法配对的，removeEventListener() 接收两个和 addEventListener() 同样的 arguments（以及可选的第三个），只不过会移除指定的事件处理函数。

addEventListener() 接收的第三个 argument 可以是一个布尔值或者一个对象，如果传入 true，则回调函数是以捕获事件处理器的形式注册的（会在等下介绍），而且会在事件触发的另一个阶段调用。想要删除同一个事件处理函数，则必须在 removeEventListener() 中传入同样的第三个 argument。

我们除了传入布尔值，也可以传入详细的 Options 对象：

```js
document.addEventListener('click', handleClick, {
  capture: true,
  once: true,
  passive: true
});
```

如果 Options 对象的 capture 属性为 true，则事件处理器会被注册成为捕获处理器，如果是 false 或被忽略，则是普通的事件处理器。

如果 Options 对象的 once 属性为 true，事件监听器会在触发一次后被移除，若为 false 或被忽略，则不会被自动移除。

如果 Options 对象的 passive 属性为 true，则表示事件处理函数不会调用 preventDefault() 方法，这对手机的触摸事件比较有用。如果 touchmove 事件可以 prevent 浏览器滚动的 default action，浏览器就无法实现 smooth 滚动。这个 passive 属性使得我们可以阻止这种可能的情况。正因为 smooth 滚动对于 UX 十分重要，所以浏览器的 touchmove 和 mousewheel 默认传入 passive 为 true。所以在需要对这些事件增加使用 preventDefault() 的函数，则需要显式的写下 passive 为 false。

##### 事件处理函数调用

在注册事件处理函数函数之后，浏览器就会在事件发生时自动调用它。这一节会深入解释背后发生了什么。

事件处理函数的 argument

没有给事件处理函数会接收一个事件对象作为其单一的 argument，这个事件对象包括以下属性：

- type：事件类型
- target：目标对象
- currentTarget：对于会传播的对象，这个属性是当前事件处理函数注册的对象
- timeStamp：毫秒表示的事件发生的时间戳
- isTrusted：true 代表浏览器生成的事件，false 代表 JavaScript 代码生成的事件
- 其他根据事件类型增肌的属性

##### 事件处理函数上下文（context）

通过写入属性的方式注册的事件处理函数看上去就像在目标对象上新增了一个方法一样：

```js
target.onclick = function() { /* handler code */ };
```

所以说，事件处理函数是以目标对象的方法的形式来调用的。这意味着在函数体中 this 指向了注册事件处理函数的对象，也就是目标对象。

对于 addEventListener() 注册的事件处理函数，其中的 this 的值也是相同的，不过对于箭头函数不管用，箭头函数的 this 依然指向其被定义的上下文。

##### 事件处理函数的返回值

理论上说，现在的 JavaScript 中的事件处理函数不应该返回任何东西，但是对于老旧的代码，可能会有返回值。这个返回值通常是一个用于表示浏览器不需要执行默认 action 的信号。就比如 submit 按钮的 onclick 返回 false，则浏览器不会继续默认的 submit 表单。

现在的阻止默认行为的方法是调用 preventDefault() 方法。

#### 事件传播（Event Propagation）

对于 Window 对象或者其他独立对象的事件，浏览器对其的处理就是简单地调用其处理函数。但是如果目标对象是 Document 对象，情况就会变得复杂起来。

在调用完目标对象上的事件处理函数之后，大多数事件（focus，blur，scroll 事件除外）会沿着 DOM tree 一路冒泡（bubble），所以目标对象的 parent 的事件处理函数会被调用，然后是 grandparent 的事件处理函数，以此类推一直到 Document 对象，然后是 Window 对象。load 事件会在 document 元素的 load 事件也会冒泡，不过会在到了 Document 对象时停止，不冒泡到 Window 对象（Window 对象的 load 事件处理函数只会在整个 document 加载完成时触发）。

事件冒泡是事件传播的第三个阶段，而调用目标对象上的事件处理函数是第二个阶段，而第一个阶段则是在调用事件处理函数之前发生的，被称为捕获（capturing）阶段。还记得 addEventListener() 会接受第三个可选的 argument，如果 argument 为 true 或者 {capture: true}，则事件处理函数会已捕获事件处理函数的形式被注册，然后会在事件传播的第一个阶段被调用。

这第一个捕获阶段就如同相反的冒泡阶段，捕获处理函数会从 Window 对象先开始调用，然后是 Document 对象，然后是 body 对象，然后一路沿着 DOM tree 传播直到目标对象的 parent。但是要注意的是，目标对象上的捕获函数不会被调用。

这一捕获机制是我们可以在事件传递到目标对象之前捕获到它，可以用于 debug 或者用于事件 cancel 技术，用于过滤掉一些事件。这个机制可以在 drag 事件中用到，因为 drag 时的鼠标移动事件只需要被目标对象处理。

#### 事件取消

浏览器会在许多用户事件时响应，即使那些事件没有被我们的代码所用到。比如点击超链接时，浏览器会自动跳转到新的链接。

取消与事件关联的默认 action是唯一一种事件取消技术。我们也可以通过调用事件对象上的 stopPropagation() 方法来取消事件的传播。对于同一目标对象上的事件处理函数，它们依然会被调用，
但是对于其他对象上的处理函数则不会再被调用。

stopPropagation() 方法会在捕获阶段以及冒泡阶段都可以生效，调用时要记得以 e.stopPropagation() 的形式，因为该方法在事件对象上。

还有一个类似的方法 stopImmediatePropagation()，不过它会阻止目标对象上剩余的事件处理函数的执行。

#### 生成自定义事件

客户端 JavaScript 事件的 API 可以使我们定义并且生成自定义事件。比如我们希望定期执行一次网络请求来确定页面是否要更新，其发生时我们希望用户知道我们正在加载，而非面对未响应的页面，我们可以在这时生成一个事件用于告知用户。

如果一个 JavaScript 对象拥有 addEventListener() 方法，则会说明这是一个可以作为事件对象的对象。它也拥有一个 dispatchEvent() 方法。我们可以通过 CustomEvent() 构造函数来创建一个自定义事件，然后传入 dispatchEvent()。CustomEvent() 的第一个 argument 是指定事件类型的字符串，第二个 argument 是定义事件对象属性的对象，我们可以将该对象的 detail 属性设置为字符串，对象，或者其他值，他会用于表示事件的内容：


```js
// 生成一个自定义事件，告知 UI 现在正在加载
document.dispatchEvent(new CustomEvent('loading',{detail: true;}));

// 发送网络请求
fetch(url)
  .then(handleNetworkResponse)
  .catch(handleNetworkError)
  .finally(() => {
    // 加载完成后再生成一个自定义事件，告知 UI 加载完成
    document.dispatchEvent(new CustomEvent("busy", {detail: false }));
});

// 程序的其他地方拥有一个事件处理函数的注册
document.addEventListener('loading',(e) => {
  if (e.detail) {
    showLoading();
  } else {
    hideLoading();
  }
});
```