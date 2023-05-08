# DOM

## 什么是 DOM？

DOM，全称：文档对象模型（Document Object Model），就是可以通过 js 来操作 HTML 的，一般 DOM 我们称为 DOM树，因为把整个 HTML 网页的文档看作一棵树，而 HTML 网页中的各个元素就是各个节点，对应 DOM树 中的节点。



## 什么是节点（Node）？

**节点**是构成网页的最基本的组成部分。是构成 HTML 文档最基本的单元，**网页中的每一个部分都可以称为是一个节点**，例如：

```html
<div>
    <span>Hello world</span>
    <p>
        这是一个兄弟节点
    </p>
</div>
```

在这里，`div` 就是 `span` 的父节点，而 `span` 是 `div` 的子节点，`span` 是 `p` 的兄弟节点。

而在一个完整的 HTML 网页中，包含 `head` 和 `body` 两部分，而都称为节点，整个文档是一个文档节点。

由于网页中的每一个部分都可以称为是一个节点，所以在上面的代码中，`p` 标签中的文本”这是一个兄弟节点“，其实也是一个节点，叫做文本节点。



**常用的节点**

- 文档节点：整个 HTML 文档
- 元素节点：HTML 文档中的 HTML 标签
- 属性节点：元素的属性
- 文本节点：HTML 标签中的文本内容



### 如何找到节点？

浏览器已经为我们提供了文档节点对象，这个对象是 window 对象属性，可以在页面中直接使用，文档节点代表的是整个网页。





## 事件

文档或浏览器窗口发生的一些特定的交互瞬间，就是用户和浏览器之间的交互行为。比如：点击按钮、鼠标移动、关闭窗口等等。

JavaScript 与 HTML 之间的交互是通过事件实现的。

我们可以为按钮的对应事件绑定处理函数的形式来对事件进行响应，而被响应的事件内的函数叫做响应函数，这样当事件被触发时，其对应的函数将会被调用。

拿 `button` 按钮的点击事件来举个简单的例子。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>button点击事件</title>
</head>
<body>
    <!-- 这里定义了一个 button 按钮，为了能在 js 中找到它，给了它一个 id 属性 -->
    <button id="btn">点我</button>
</body>
<script>
    // 首先我们通过 id 来获取到该元素，我们将获取到的元素，赋值给一个变量 -> btn
    // 这个后面会讲如何获取元素
    let btn = document.getElementById('btn');
    // 拿到了这个元素之后我们就可以对它赋予事件了
    btn.onclick = function() {
        alert('hello world');
    }
</script>
</html>
```

1. 上面的代码是通过 document 的内置方法，找到了 id 为 btn 的元素，将找到的元素赋值给 btn 这个变量
2. btn 这个变量包含了 `button` 这个元素后，它有个 onclick 属性，就是鼠标单击事件。当按钮被点击后，这个事件就会被触发，有点抽象，可以将事件理解为饮水机，当没有水放在挂载在饮水机上面的时候，去打开饮水机阀门是不会有水流出来的，只有当有水放上去了，去打开饮水机阀门才会有水流出来，也就是所谓的事件被触发了。所以在这个例子中，onclick 属性是饮水机，只有当我们给这个属性添加一个 function()，就相当于给饮水机上放了水，而这个事件什么时候触发，就取决于用户啥时候打开阀门了。



> 我们一般不会在 HTML 元素中编写事件，例如 `<button onclick="alert('hello world');">点我</button>`，这个方式是不被推荐的
>
> **那我们如何编写事件呢？**
>
>  在 HTML 中的任意位置编写 script 代码，我们一般在 body 后面编写 script ，为什么不在前面编写后面会讲到。





## 文档的加载

我们的 HTML 文档加载出来是有顺序的，与基本的编程一样，是从上往下的顺序（异步编程除外），读取到一行就运行一行，如果这时候我们将 script 标签写到页面的上边，在 script 代码执行时，页面还没有加载出来，DOM 对象也就没有加载，会导致 DOM 无法获取到。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>button点击事件</title>
	<script>   // 例如这里，把 script 标签写在了 body 的上面，运行页面代码时会报错。
    let btn = document.getElementById('btn');
    btn.onclick = function() {
        alert('hello world');
    }
	</script>
</head>
<body>
    <button id="btn">点我</button>
</body>
</html>
```

例如上面的代码，运行时会报错。



### 那我们如何解决呢？

在 window 对象中，有一个 onload 事件，该事件会在整个页面加载完成之后才触发。

那么因为这个事件会在整个页面加载完成以后触发，所以我们可以对它绑定一个函数，在这个函数内部添加我们要执行的 script 代码，这样，我们需要的代码就会在整个页面加载完成之后才触发。

例如：

```html
<script>
    window.onload = function() {
        // 在这内部就可以添加我们需要执行的代码了
    };
</script>
```

在 onload 函数内部，你可以把上面的 onclick 事件的代码复制进来，这样的话 script 标签随便写在哪里都不会报错了。





## DOM 查询

### 什么是 DOM 查询？

简单来说就是通过 window 对象中的方法，来获取 HTML 文档中的各个元素节点。



### 如何获取节点？

我们在原生 js 中，一般都是调用 document 中的各个方法来对元素的节点进行获取，jquery 中简化了这个操作，但是我们必须得打好基础。



通过 document 对象调用

调用方式例如：document.getElementById();

- getElementById()
  - 通过元素的 `ID` 获取 **一个** 元素节点对象
- getElementsByTagName()
  - 通过元素的 `标签名` 获取 **一组** 元素节点对象
- getElementsByName()
  - 通过元素的 `name属性` 获取 **一组** 元素节点对象
- querySelect()
  - 根据一个 `CSS选择器` 获取 **一个** 元素节点对象
  - 如果找到的有多个，那么只会返回第一个
- querySelectorAll()
  - 根据一个 `CSS选择器` 获取 **一组** 元素节点对象

`document.all` 代表获取页面中所有的元素，还有 `document.body` 是对 body 的引用。

通过上面的几种方法，我们可以对元素进行获取了。

在调用上述方法时，传入对应的参数即可得到节点对象。例如：

`document.getElementById('btn')` 这一段代码就是获取 id 为 btn 的元素，在方法中传入 id 的字符串参数。

这里要提到的是，获取的节点对象为 **一组** 时，会使用数组来保存对应的节点（即使符合条件的元素只有一个，它也会返回数组），这时候就需要通过索引（数组下标）来取到相应的元素。

我们得到了节点对象，可以通过一个变量来接收，然后对变量进行对应的操作即可。

例如我们使用 btn 来接收了查询到的 **一个** 元素，我们可以通过 innerHTML 这个属性来获取到内部的 HTML 节点。

例如：`console.log(btn.innerHTML);` ，通过这段代码，我们就可以在控制台得到 btn 内部的 HTML **节点**了。

我们也可以通过 `console.log(btn.innerText);` 来获取 btn 内部的**文本**。





> 提示

首先，得对一点概念进行强调，文本也算节点，也就是说，标签名中间的文字内容也算是一个节点，叫做文本节点。

所以，如果有节点嵌套（例如一个 div 中嵌套一个 span），那么我假如这个 div 中还有文本内容，

例如 `<div>这是div中的文本<span></span></div>` 这种形式，那么通过 innerHTML 获取，不仅会获取到 span 标签，还会获取到 “这是div中的文本” 这一段文本，只有通过 innerText，才会获取到 “这是div中的文本” 这段内容。

还要对一点强调的就是，一个对象的属性是不带括号的，方法是带括号的。

例如：`document.innerHTML`  这是属性

`document.getElementById()`  这是方法

它们的差别就在于有没有括号

**获取元素节点的子节点**

- childNodes

  获取当前节点的所有子节点

- firstChild

  获取当前节点的第一个子节点

- lastChild

  获取当前节点的最后一个子节点



**获取元素节点的兄弟节点**

- parentNode

  获取当前节点的父节点

- perviousSibling

  获取当前节点的前一个兄弟节点

- nextSibling

  获取当前节点的后一个兄弟节点



当然具体的可以通过 w3schoold 进行查找，这里不多概述了。





## 使用 DOM 操作 CSS

