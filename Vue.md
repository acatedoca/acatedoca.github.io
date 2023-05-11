# Vue

## 介绍

### Vue.js 是什么

是一套用于构建用户界面的**渐进式框架**。与其他大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层。

> 什么是渐进式框架？

渐进式框架指的是：你不想用的部分可以先不用，Vue 不强求你一次性接受并使用它的全部功能特性。（意思就是不需要用了一点就必须用所有的部分）

> Vue 核心功能

基础功能：页面渲染、表单处理提交、帮我们管理 DOM 节点。

组件化开发：增强代码的复用能力，复杂系统代码维护更简单。

前端路由：更流畅的用户体验、灵活的在页面切换已渲染组件的显示，不需与后端做多余的交互。

前端工程化：结合 webpack 等前端打包工具，管理多种静态资源，代码，测试，发布等，整合前端大型项目。

> MVVM 模型

MVVM 是 Model-View-ViewModel 的缩写，它是一种基于前端开发的架构模式，其核心是提供对 View 和 ViewModel 的双向数据绑定，这使得 ViewModel 的状态改变可以自动传递给 View，即所谓的数据双向绑定。

Vue.js 是一个提供了 MVVM 风格的双向数据绑定的 JavaScript 库，专注于 View 层。它的核心是 MVVM 中的 VM，也就是 ViewModel。 ViewModel 负责连接 View 和 Model，保证视图和数据的一致性，这种轻量级的架构让前端开发更加高效、便捷。

> MVVM 模型扩展

**为什么会出现 MVVM 呢？**

MVC 即 Model-View-Controller 的缩写，就是 模型—视图—控制器，也就是说一个标准的 Web 应用程式是由这三部分组成的：

- View： 用来把数据以某种方式呈现给用户
- Model： 其实就是数据
- Controller： 接收并处理来自用户的请求，并将 Model 返回给用户

在 HTML5 还未火起来的年代，MVC 作为 Web 应用的最佳实践是可以的，这是因为 Web 应用的 View 层相对来说比较简单，前端所需要的数据在后端基本上都可以处理好，View 层主要是做一下展示，那时候提倡的是 Controller 来处理复杂的业务逻辑，所以 View 层相对来说比较轻量，就是所谓的**瘦客户端思想**。

**为什么前端要工程化，要是使用 MVC ？**

相对 HTML4，HTML5 最大的亮点就是它为移动设备提供了一些非常有用的功能，使得 HTML5 具备了开发 APP 的能力，HTML5 开发 APP 最大的好处就是跨平台、快速迭代和上线，节省人力成本和提升效率，因此很多企业开始对传统的 APP 进行改造，逐渐用 H5 代替 Native，到 2015年 的时候，市面上大多数 APP 或多或少都嵌入了 H5 的页面。既然要用 H5 来构建 APP ，那 View 层所做的事，就不仅仅是简单的数据展示了，它不仅要管理复杂的数据状态，还要处理移动设备上各种操作行为等等。因此，前端也需要工程化，也需要一个类似于 MVC 的框架来管理这些复杂的逻辑，使开发更加高效。但这里的 MVC 又稍微发生了点变化：

- View： UI布局，展示数据
- Model： 管理数据
- Controller： 响应用户操作，并将 Model 更新到 View 上

这种 MVC 架构模式对于简单的应用看起来是OK的，也符合软件架构的分层思想。但实际上，随着 H5 的不断发展，人们更希望使用 H5 开发的应用能和 Native 媲美，或者接近于原生 APP 的体验效果，于是前端应用的复杂程度已不同往日，今非昔比。这时前端开发就暴露出了三个痛点问题：

1. 开发者在代码中大量调用相同的 DOM API ，处理繁琐，操作冗余，使得代码难以维护。
2. 大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。
3. 当 Model 频繁发生变化，开发者需要主动更新到 View，当用户的操作导致 Model 发生变化，开发者同样需要将变化的数据同步到 Model 中，这样的工作不仅繁琐，而且很难维护复杂多变的数据状态。

其实，早期 jQuery 的出现就是为了前端能更简洁的操作 DOM 而设计的，但它只解决了第一个问题，另外两个问题始终伴随前端一直存在。

**MVVM 的出现，完美解决了以上三个问题**

MVVM 由 Model、View、ViewModel 三部分构成，Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI组件，它负责将数据模型转化成 UI 展示出来，ViewModel 是一个同步 View 和 Model 的对象。

**在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的，因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上**

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需要关注业务逻辑，不需要手动操作 DOM，不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

> 理解

MVVM 模型，是对数据的监听，对数据的处理，将处理后的数据显示出来。而监听和数据的处理都由 Vue 来完成，而所需要的只有 Model 和 View （数据模型和视图），通过对数据模型的输入，在 ViewModel （视图模型）里设定对应的处理，最后处理结果会自动显示在 View 上。

View 对应的是 DOM 

ViewModel 对应的是 Vue

Model 对应的是 plain JavaScript Objects



### 起步

> 步骤

1. 引入 script 资源
   - CDN 引入
   - 本地文件引入
2. 



### 声明式渲染

> Vue.js 的核心是一个允许采用简介的模板语法来声明式地将数据渲染进 DOM 系统

```html
<div id="app">
    {{ message }}
</div>
```

```javascript
var app = new Vue({
    el: '#app',   // 将 Vue 实例挂载到 #app 中
    data:{
        message: 'Hello Vue!',   // 将数据显示到 #app 的模板语法里
    }
})
```



> 指令

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

指令带有前缀 `v-` ，以表示它们是 Vue 中特殊的 attribute，例如 `v-bind`，它们会在渲染的 DOM 上应用特殊的响应行为。在这里，该指令的意思是：“将这个元素节点的 `title` attribute 和 Vue 实例的 `message` property 保持一致”



### 条件与循环

> v-if

```html
<div id="app">
    <p v-if="seen">现在可以看到该元素</p>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        seen: true,
    }
});
```

在控制台输入 `app.seen = false` 可以发现显示的消息消失了。



> v-for

```html
<div id="app">
    <ul>
        <li v-for="todo in todos">
            {{ todo.text }}
        </li>
    </ul>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data:{
        todos: [
            {text: '学习 JavaScript'},
            {text: '学习 Vue'},
            {text: '学习 Java'},
        ]
    }
})
```

在控制台输入 `app.todos.push({text: '这是一个新项目'})` ，会发现列表最后添加了一个新项目。



> 提示

在控制台输入 `app.seen` 或 `app.todo.push` 本质上是通过原型来访问 app 的属性，所以会通过原型链一直查找属性进行修改。



### 处理用户输入

> v-on 事件监听

```html
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">反转消息</button>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!',
    },
    methods: {
        reverseMessage: function(){
            this.message = this.message.split('').reverse().join('');
        }
    }
});
```

在这里，给 button 绑定了事件监听，监听点击事件，而点击事件触发的是 Vue 中的 methods 里的 reversMessage 方法，而这个方法操作的是 this ，而 this 指向的就是当前该对象（app），也就是 this.message 改变的是 data 中的 message 。



> v-model 双向绑定

```html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
});
```

在上面的代码中，input 中的 `v-model` 实现双向绑定，将数据(Model)绑定至视图模型(ViewModel)上，再将视图模型和视图(View)绑定，最后达到数据的改变从而影响视图。



### 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树。

> 可以把 Vue 实例挂载到的元素认为是根节点，一棵树只有一个根节点，也就是一个 Vue 实例。

在 Vue 里，一个组件本质上是一个拥有预定义选项的 Vue 实例。（注册组件）

```javascript
Vue.component('my-component', {
    template: `<li>这是一个待办项</li>`,
})
```

在 html 中，插入预定义的组件模板即可。

```html
<ul>
    <!-- 在此插入预定义的模板 -->
    <my-component></my-component>
</ul>
```

**注意：注册组件一定要在 Vue 实例化之前**

但是上面的组件渲染出来的内容都是相同的，数据并不能随我们想要的改变。所以需要将父作用域的数据传到子组件。

> 父作用域向子组件传递数据，需要子组件有一个 property 来接收它，所以在自定义组件中，需要定义一个 props 来接收传递进来的数据。可以理解为：父作用域就是等号右边的，子组件就是等号左边的。

子单元通过 prop 接口与父单元进行了良好的解耦。





## Vue 实例

### 创建一个 Vue 实例

每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的。

当创建一个 Vue 实例时，可以传入一个**选项对象**。

一个 Vue 应用由一个通过 `new Vue` 创建的**根 Vue 实例**，以及可选的嵌套的、可复用的组件树组成。

```javascript
var vm = new Vue({
    // 每个 Vue 应用都是通过 Vue 函数的实例化对象，创建时可以在括号内传入选项对象
    // 选项
});
```



**所有的 Vue 组件都是 Vue 实例。**



### 数据与方法

当一个 Vue 实例被创建的时候，将 `data` 对象中添加 property ，当这些 property 的值发生改变的时候，视图将会产生”响应“，即匹配更新为新的值。

当这些数据发生改变时，视图会进行重渲染。但是只有当实例被创建时就已经存在于 `data` 中的 property 才是响应式的。也就是说你在创建实例后，在外部添加一个新的 property，就不是响应式的了，例如：

```javascript
var vm = new Vue({
    data: {
        a: 0,
    }
});
vm.b = 'Hello Vue!';   // 这里的 b 不是响应式的
```

所以，对 `b` 的改动，不会触发任何视图的更新。如果你知道你会在后面的操作中添加一个 property，但是一开始它不存在，你就需要对该 property 赋初值。

唯一例外是使用 `Object.freeze()` ，这回阻止修改现有的 property，也就是说通过该方法会使响应系统无法再追踪变化。

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$` ，以便于用户定义的 property 区分开来，相当于对象固有的属性。例如：

```javascript
var data = { a: 1 };
var vm = new Vue({
    el: '#app',
    data: data,
});

vm.$data === data;   // ==> true
vm.$el === document.getElementById('app');   // ==> true

// $watch 是一个实例方法
vm.$watch('a', function(newValue, oldValue) {
    // 这个回调函数将会在 vm.a 的值改变后调用
})
```



### 实例生命周期钩子

> 什么是生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程，例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己代码的机会。

比如 `created` 钩子可以用来在一个实例被创建之后执行代码：

```javascript
new Vue({
    data: {
        a: 1
    },
    created: function(){
        // `this` 指向 vm 实例
        console.log('a is:' + this.a);
    }
})
// => 'a is: 1'
```

**注意：**不要在选项 property 或回调上使用**箭头函数**。因为箭头函数没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直到找到为止。



#### 什么是钩子函数

钩子函数是一个事件触发的时候，在系统级捕获到了它，然后做一些操作。一段用以处理系统消息的程序。“钩子”就是在某个阶段给你一个做某些处理的机会。

特点：1、是个函数，在系统消息触发时被系统调用。2、不是用户自己触发的。3、使用时直接编写函数体。

钩子函数的名称是确定的，当系统消息触发，自动会调用。例如 react 的 componentWillUpdate 函数，用户只需要编写 componentWillUpdate 的函数体，当组件状态改变要更新时，系统就会调用 componentWillUpdate。



#### 生命周期过程

- beforeCreate()

  在实例初始化之后，数据观测和事件配置之前被调用，此时组件的选项对象还未创建，el 和 data 并未初始化，因为无法访问 methods，data，computed 等上的方法和数据。

  在这个钩子函数里，只是刚开始初始化实例，你拿不到实例里的任何东西，比如 data 和 methods 和事件监听等。

- created()

  实例已经创建完成之后被调用，在这一步，实例已完成以下配置：数据观测、属性和方法的运算，watch / event 事件回调，完成了 data 数据的初始化，el 没有。然而，挂载阶段还没有开始，$el 属性目前不可见，这是一个常用的生命周期，因为你可以调用 methods 中的方法，改变 data 中的数据，并且修改可以通过 vue 的响应式绑定体现在页面上，获取 computed 中的计算属性等等，通常我们可以在这里对实例进行预处理，也可以在这里发送 ajax 请求，值得注意的是，这个周期中没有什么方法来对实例化过程进行拦截的，因此假如有某些数据必须获取才允许进入页面的话，并不适合在这个方法发请求，建议在组件路由钩子 beforeRouterEnter 中完成。

  在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测（data observer），属性和方法的运算，watch / event 事件回调。然而，挂载阶段还没开始， `$el` 属性目前不可见。

  这是最早能拿到实例里面的数据和方法的一个钩子函数。**应用场景：异步数据的获取和对实例数据的初始化操作都在这里面进行。**

- beforeMount()

  挂载开始之前被调用，相关的 render 函数首次被调用（虚拟 DOM ），实例已完成以下的配置：编译模板，把 data 里面的数据和模板生成 html，完成了 el 和 data 初始化，注意此时还没有挂载 html 到页面上。

  在挂载开始之前被调用：相关的 `render` 函数首次被调用。

  不论是 created 还是 beforeMount 在它们里面都拿不到真实的 DOM 元素，如果我们需要拿到 DOM 元素就需要在 mounted 里操作。

- mounted()

  挂载完成，也就是模板中的 html 渲染到 html 页面中，此时一般可以做一些 ajax 操作，mounted 只会执行一次。

- beforeUpdate()

  在数据更新之前被调用，发生在虚拟 DOM 重新渲染和打补丁之前，可以在该钩子中进一步的更改状态，不会触发附加的重渲染过程。

  当数据更新后触发的钩子函数，这个钩子函数里拿到的是更改之前的数据，**虚拟 DOM 重新渲染**和打补丁之前被调用。

  你可以在这个钩子中进一步地修改 data，这不会触发附加的重渲染过程。

- updated()

  在由于数据更改导致的虚拟 DOM 重新渲染和打补丁只会调用，调用时，组件 DOM 已经更新，所以可以执行依赖于 DOM 的操作，然后再大多数情况下，应该避免在此期间更改状态，因为可能会导致更新无限循环，该钩子在服务器端渲染期间不被调用。

  注意：updated 是指 mouted 钩子后（包括 mounted ）的数据更改，在 created 里的数据更改不叫更改叫做初始化，所以在 created 里修改数据是通过一个异步来确保 updated 可以执行的。我们一般都是在事件方法里更改数据，然后通过 updated 对其操作。应用场景：如果 DOM 操作依赖的数据是在异步操作中获取，并且只有一次数据的更改 ，也可以说是数据更新完毕：如果对数据更新做一些统一处理在 updated 钩子中处理即可。

  > updated、watch 和 nextTick 区别
  >
  > updated 对所有数据的变化进行统一处理
  >
  > watch 对具体某个数据变化做统一处理
  >
  > nextTick 是对某个数据的某一次变化进行处理
  >
  > 如果实例里面没写 el 挂载点你就需要在实例后面通过 .$mount('#app') 来手动触发

- beforeDestroy

  在实例销毁之前调用，实例仍然完全可用。

  1. 这一步还可以用 this 来获取实例
  2. 一般在这一步做一些重置的操作，比如清除掉组件中的定时器和监听的 DOM 事件

- destroyed

  在实例销毁之后调用，调用后，所有的事件监听器会被移除（Vue 实例指示的所有东西都会解绑），所有的子实例也会被销毁，该钩子在服务器端渲染期间不被调用。

**beforeDestroy 和 destroyed 只能通过手动触发 $destroy 来调用**

```javascript
let app = new Vue({
    beforeDestroy() {
        console.log('beforeDestroy');
    },
    destroyed() {
        console.log('destroyed');
    }
});
app.$destroy();   // 手动销毁
```



## 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。



### 插值

#### 插入的值是普通文本

数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值。

Mustache 标签将会替代为对应数据对象上 `msg` property 的值。无论何时，绑定的数据对象上 `msg` property 发生了改变，插值处的内容都会更新。

可以理解为：Vue 会寻找 {{  }} 这个符号，找到以后，会将符号中的变量，也就是插值替换为 Vue 实例中绑定的 data 数据。



#### 插入的值是 HTML 代码

双大括号会将数据解释为普通的文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html`。

你如果向模板语法中插入一个 HTML 标签，你就需要使用这个指令，如果没有使用该指令渲染，使用上述的 Mustache 语法来渲染标签的话，那么标签会变成字符串文本被渲染。

**注意：在站点上动态渲染任意的 HTML 可能会受到 XSS攻击。请对可信的内容使用 HTML 插值，绝不要对用户提供的内容使用插值。**



#### Attribute

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 `v-bind` 指令。

```html
<div v-bind:id="dynamicId"></div>
```

`v-bind` 的作用就是绑定一个标签的属性（attribute），而这个属性的值为一个 data 中的变量，当 data 中的变量值发生改变时，而这个标签的 attribute 会发生响应。



#### 插入的值是 JavaScript 表达式

在模板语法中可以使用 JavaScript 表达式。例如：

```html
{{ number + 1 }}
{{ ok ? 'yes' : 'no' }}
<div v-bind:id="'list-' + id"></div>
```

这些表达式会被 Vue 所解析。Vue 会将 data 数据中的值代入表达式，最后渲染表达式的值。

但是每个绑定都只能包含**单个表达式**，所以下面的例子都**不会生效**。

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，需要使用三元表达式 -->
{{ if (ok) { return message } }}
```

> 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。
>
> 理解：Vue 实例在挂载前，已经将模板语法编译进了虚拟 DOM，在模板表达式中只能拿取全局变量白名单中的一个变量，而且不能在模板表达式中访问用户自定义的全局变量，只能拿到系统变量。



### 指令

#### 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如：

```html
<a v-bind:href="url">...</a>
```

这里的 `href` 是参数，告知 `v-bind` 指令将元素的 `href` attribute 与表达式 `url` 的值绑定。



#### 动态参数

> 2.6.0 新增，从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

```html
<a v-bind:[attributeName]="url">...</a>
```

这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` property `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

意思就是说：在这里可以将 attribute 属性作为一个变量，在 data 里的 property 中进行动态改变，在 data 的 property 中 `attributeName` 是 `href` 的话，这里绑定的就是 a 标签的 `href` 属性，如果是 `id` 的话，那这里绑定的就是 a 标签的 `id` 属性。



**对动态参数的值的约束**

动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

**对动态参数表达式的约束**

动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。例如：

```html
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写：

```html
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```



#### 修饰符

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```



### 缩写（语法糖）

#### v-bind 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```



#### v-on 缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```





## 计算属性和侦听器

### 计算属性

#### 什么是计算属性？

模板内的表达式非常便利，但是更适用于简单运算的。放入太多的逻辑会让模板过重且难以维护。

通过一个函数来对原始值进行计算，返回一个计算后的值。

#### 计算属性的用法

通过 computed，可以获取到标签中的数据，对其进行函数运算，返回一个新的数据。例如：

```html
<div id='app'>
    <input type='text' v-model="val">
    <p>计算后的值：{{ newVal }}</p>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        val: '1',
    },
    computed:{
        newVal: function(){
            // `this` 指向 app 实例，也就是说 Vue 实例挂载在哪个元素上，this 就是指向哪个元素
            return this.val * 15;
        }
    }
});
```

结果：在 input 中输入任意一个数值，最后在 p 标签中显示的是对应数值的 15 倍。

流程：首先是将 app 挂载到 Vue 实例上，再对 data 进行初始化，将 `v-model` 绑定到 data 中的 val 中，在 Vue 实例中添加计算属性，计算属性的返回值为 this.val 也就是 Vue 实例的 val 值，将计算后的值 return 给 newVal，最后将 newVal 添加到插值中。

1. **计算属性可以依赖其他计算属性。**
2. **计算属性不仅可以依赖当前 Vue 实例的数据，还可以依赖其他实例的数据。**



计算属性包含一个 getter 属性和一个 setter 属性，一般我们都是使用计算属性的默认用法—— getter 来进行读取。

**当需要手动修改计算属性的值就像修改一个普通数据那样时，就需要使用 setter 属性，触发 setter 函数，执行一些自定义的操作。**





#### 什么是计算属性缓存

> **概念**
>
> 计算属性：计算出来的结果，保存在属性中，内存中运行：虚拟 DOM。计算属性可以理解为缓存，是为了避免每次调用都需要进行计算产生的系统开销。Vue 特有的。



#### 计算属性缓存 vs 方法

根据上面的例子，我们还可以通过另一种办法来达到同样的效果。将代码进行一点修改：

```html
<p>计算后的值为：{{ newVal() }}</p>
```

```javascript
// 在组件中添加
methods: {
    newVal: function(){
        return this.val * 15;
    }
}
```

可以将同一函数定义为一个方法，而不是一个计算属性。

**区别：**

- methods 使用 getMessage() 取值，computed 使用 getMessage 取值。

- 计算属性是基于它的依赖进行缓存的。也就是说**计算属性只有在它的相关依赖发生改变时才会重新求值**。而在 methods 中，**不管它的依赖值有没有发生改变，它都会再次执行计算**。





#### 计算属性 vs 侦听属性

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：**侦听属性**。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 `watch`——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性而不是命令式的 `watch` 回调。



#### 计算属性的 setter

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter。



### 侦听器

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。



### Class 和 Style 绑定

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

#### 绑定 HTML Class

##### 对象语法







## 组件

### 什么是组件？





**组件是 Vue 最为精彩设计的地方**

> 全局注册



> 局部注册



将每一个页面都视为组件



template 如何在组件中使用

在 vue.component 中添加



在 template 中只会存在一个根节点，只会渲染第一个，而且必须要有一个根节点





自定义的标签称为组件



第一步看 vue 挂载点

data 属于 vue 实例







> 数据传递线路

1. 首先看 vue 实例挂载在哪个节点上
2. 然后去 body 中找到这个节点
3. 再在 vue 实例中对 data 进行初始化
4. 初始化后将 body 对应的元素与 data 中的数据进行绑定
5. 绑定后创建一个组件
6. 将创建的组件在 body 中显示出来
7. 然后在 body 中的自定义组件绑定 data 中的数据
8. ……





> 自定义事件

子组件向父组件传递数据就需要自定义事件



$emit 是子组件向父组件传递数据的方法







在 vue 中，定义一个方法并调用是通过 data() {} 这种形式





计算属性是多个值影响一个值所用到的属性

watch 属性是一个值影响另一个值

watch 监听器，在监听器内 属性发生变化时，会传递到 function 的 val 中

