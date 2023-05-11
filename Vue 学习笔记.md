# Vue 学习笔记

## Vue 介绍

Vue 是一款构建用户界面的**渐进式框架**。Vue 的核心库只关心视图层。

> 渐进式框架指的是，Vue 不强求你一次性使用它的全部功能，你可以使用它的部分功能来构建整个页面。



### Vue的核心功能

- 基础功能：页面渲染、表单处理提交、帮我们管理 DOM 节点。
- 组件化开发：增强代码的复用能力，复杂系统代码维护更简单。
- 前端路由：更流畅的用户体验、灵活的在页面切换已渲染组件的显示，不需与后端做多余的交互。
- 前端工程化：结合 webpack 等前端打包工具，管理多种静态资源，代码，测试，发布等，整合前端大型项目。



### MVVM

MVVM（Model View ViewModel）

- Model（模型）：模型也就是数据抽象出来的模型，可以直接理解为就是数据。
- View（视图）：视图就是页面中用户可见的视图。
- ViewModel（视图模型）：视图模型就是 Vue。

Vue 能够将 Model 与 View 进行双向绑定，使本来没有联系的两个东西，通过 Vue 进行连接。通过 Model 的输入，对 ViewModel 进行对应的数据处理设定，最终在 View 中进行输出。







## 基础语法、指令

### 挂载

使用 JavaScript 实例化 Vue。需要将需要挂载（通信）的组件，挂载到 Vue 实例中，随后对该实例对象中，所做的一切操作都会对该 Vue 实例挂载对应的组件生效。



### 声明式渲染

> Mustache语法（插值）

使用插值来对 Vue 实例中的数据进行输出。双大括号（Mustache语法）。

在 html 中使用模板语法：

```html
<div id="app">
    {{message}}
</div>
```

在 js 中对挂载的对象进行配置：

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: "hello Vue!",
    }
})
```

通过上述的配置，即可使用 Vue，并且将输出 “hello Vue!”

对于 Mustache 语法，不仅可以直接写变量，也可以写一些简单的表达式。 

例如：`{{ counter * 2 }}`，其中，`counter` 是 Vue 实例中 data 里的一个变量。



> v-html

使用该指令可以使对应的内容解析为 html 代码。例如：

```html
<div id="app">
    <span v-html="link"></span>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        link: '<a href="https://www.baidu.com">百度</a>'
    }
})
```

上面代码中，`link` 中的内容，将会传递到 span 中，span 将会通过 `v-html` 对其进行解析，解析为 html 代码并执行，最终会显示一个可点击的 a 标签在页面中，a 标签的内容为百度，点击后会跳转到对应的网址（https://www.baidu.com 百度的网址）。



> v-text

使用该指令可以显示 Vue 实例中变量的文本，不过该语法会将标签中原有的内容覆盖，灵活性不高。





> v-pre

使用该指令可以告诉 Vue ，这个标签中的内容原封不动的显示出来，不做解析。例如：

```html
<div id="app">
    <p>{{message}}</p>
    <p v-pre>{{message}}</p>
</div>
```

使用 Mustache 语法会被 Vue 默认的解析出来，而加上 `v-pre` 则会原封不动的将内容显示出来，不做解析。





> v-cloak

由于代码是从上往下进行解析的。但是可能会出现，上面的代码加载出来了，而下面的解析代码没有加载出来，所以导致页面中出现未被解析的字符，使用该指令能够很好的避免该种情况。

使用该指令加在标签上，会在 Vue 解析前对该代码进行隐藏，而 Vue 代码加载出来以后，对应的这段内容才会被解析显示出来。





> 过滤器（Vue3不支持）

使用管道符（|）可对数据进行过滤，用于文本格式化，比如字母全部大写等操作。实际上，过滤器就相当于简化了绑定这一行为，它可以在插值中直接接收来自过滤器返回的值。

```html
<div id="app">
    <p>{{ text | toUpper }}</p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        text: 'qwer'
    },
    filters: {
        toUpper: function(obj) {
            return obj.toUpperCase()
        }
    }
})
```

对上面的代码进行分析，`text` 是原有的值，管道符（|）后面是过滤器 toUpper，我们在 Vue 实例中定义了 filters ，其中有 `toUpper` 的过滤器，该过滤器为一个函数，函数会接收传递进来的参数，该参数为原始文本值。过滤器可以进行传递，也就是使用多个过滤器进行过滤，将上述代码稍加修改，变成这种形式 -> `<p>{{ text | filter1 | filter2 }}</p>` ，首先，text 会使用 filter1 进行过滤，过滤后得到结果，再使用得到的结果进行 filter2 过滤，最终经过两次过滤得到最终结果。

过滤器本质上也是函数的方式，进行值的返回，只不过它可以将管道符前面的返回值作为参数，传递到过滤器的形参中，这样完成了值传递，可以很清晰的表示出使用管道符的这一块区域需要做什么样的处理。





> v-bind

使用该指令对 DOM 元素的属性进行绑定。指令带有前缀 `v-`，以表示它们的 Vue 中特殊的 attribute。例如 `v-bind`，它们会在渲染的 DOM 上应用特殊的响应行为。上述说到的 `v-html` 也是指令。

`v-bind` 需要绑定对应的 DOM 元素属性，并且会将绑定的属性连接到 Vue 实例的 property 中。例如：

```html
<div id="app">
    <p v-bind:title="message">
        鼠标悬停在此即可查看到消息！
    </p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        message: "hello world!",
    }
})
```

在这个例子中，`v-bind` 对 p 元素的 title 属性进行了绑定，绑定到了 `message` 这个 property 中，那么 `message` 的值被改变时，p 元素的 title 也会改变，与 `message` 保持一致。

`v-bind` 绑定的是一个标签的 attribute，绑定后再使用等号给它赋值一个变量，变量是 Vue 实例中的 property。通过该操作后，页面中对应绑定的属性，Vue 会自动查找变量进行替换（解析）。

在 `v-bind` 绑定的属性中，我们可以给它添加对象，对象中为键值对，key 为类名，而 value 为一个布尔值，当值为 `true` 时，该类名会被添加到 `v-bind` 绑定的属性中。例如：

```html
<p v-bind:class="{attr1: true, attr2: false, attr3: true}"></p>
```

在上述代码中，给 p 标签的 `class` 属性进行了绑定，并且传递的值为一个对象，而通过该对象中的键值对可以发现，`attr1` 和 `attr3` 的值为 `true` ，所以该 p 标签最终有两个 `class` ，分别为 `attr1` 和 `attr2` 。而对象中的 `true` 和 `false` 我们都可以通过一个变量来保存，变成以下形式：

```html
<div id="app">
    <p v-bind:class="{attr1: isAttr1, attr2: isAttr2, attr3: isAttr3}"></p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        isAttr1: true,
        isAttr2: false,
        isAttr3: true,
    }
})
```

通过上述代码，我们就可以对 p 标签的 `class` 属性中的内容进行控制。

（我们如果需要给对象中的 key 传递一个固定的值，就需要加上引号。`<p v-bind:class="{attr1: isAttr1}"></p>` 这种形式的话，`isAttr1` 是一个变量，如果要给它赋值为一个固定的值，那么就需要加上引号 `<p :style="{fontSize: '50px'}"></p>` 这种形式）

但是上述的代码中，`v-bind` 绑定 `class` 的属性太长了，看起来不美观，我们可以通过方法返回值的方式来进行传值。如下：

html 代码：

```html
<div id="app">
    <p v-bind:class="getClasses()"></p>
</div>
```

js 代码：

```js
var app = new Vue({
    el: '#app',
    data: {
        isAttr1: true,
        isAttr2: false,
        isAttr3: true,
    },
    methods: {
        getClasses: function() {
            return {
                Attr1: this.isAttr1,
                Attr2: this.isAttr2,
                Attr3: this.isAttr3,
            }
        }
    }
})
```

通过这种方式，代码整洁美观多了，不过这里 return 的值要用到 this，因为这里是对 Vue 实例中的 property 进行操作。





### 条件与循环

> v-if

`v-if` 语句会根据后面的 property 值选择是否渲染。与 `v-show` 不同的是，`v-show` 只是通过为元素添加 `display`属性来达到显示隐藏的效果，而 `v-if` 则是使元素是否渲染来达到效果。`v-show` 适用于元素需要经常开关的场景，这样不会耗费特别多的资源。

`v-if` 使用案例：

```html
<div id="app">
    <p v-if="seen">现在可以看到该元素</p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        seen: true,
    }
})
```

挂载在 id 为 app 的节点上，对其中的 seen 属性设置为 true。id 为 app 的节点中有一个 p 标签，`v-if` 会去 Vue 实例中查找 `seen` 这个 property 的值，查找到了以后，将根据 `seen` 的值来选择是否显示（渲染）该元素。

`v-else` 是当 `v-if` 不满足时，显示另外一部分（`v-else` 中的部分）；`v-else-if` 则是当 `v-if` 不满足时，再进行条件判断。



> v-for

`v-for` 语句会对选定的 property 中对应的数组进行读取，并且会将读取到的内容，存储在一个临时变量中，并且会监听 property 中对应数组的改变，以得到循环的结果。

`v-for` 使用案例：

```html
<div id="app">
    <ul>
        <li v-for="todo in todos">{{ todo.text }}</li>
    </ul>
</div>
```

通过该 html 代码，可以看出其中的意思。`todo` 是从 `todos` 中获取到的每一项，而 `todos` 则是 Vue 实例中的 property （该 property 是一个数组），由于该 property 的每一项是一个对象，那么我们使用 `.` 来获取每个对象中 `text` 的值。

下面是 Vue 实例的代码：

```js
var app = new Vue({
    el: '#app',
    data: {
        todos: [
            { text: 'C prime Plus' , data: 1 }，
            { text: 'Java开发宝典' , data: 2 }，
    		{ text: 'Python入门到精通' , data: 3 }，
            { text: 'JavaScript高级编程' , data: 4 }，
        ]
    }
})
```

通过 Vue 实例的配置，那么我们最终会显示在页面的效果就是 4 个无序列表项，每个列表项的内容分别对应 Vue 实例中 `todos` 的每一项。html 中的代码渲染的只是 `todos` 中每一项的 `text` ，而我们要渲染 `data` 的话，则将 html li 中的 `todo.text`  改为 `todo.data` 即可。

而上述案例中，`todo` 是我们定义的一个变量名，也就是从 `todos` 中遍历，拿到的每一项，我们还可以拿到对应的下标值，使用这种形式：`(todo, index)` ，这种形式，我们也可以拿到其下标值。

**我们还可以通过 v-for 来遍历一个对象**

```html
<div id="app">
    <!-- 第一种方式，直接获取对象中的 value，不获取 key -->
    <ul>
        <li v-for="item in info">{{item}}</li>
    </ul>
    
    <!-- 第二种方式，获取 key 和 value -->
    <ul>
    	<li v-for="(value, key) in info">{{value}} - {{key}}</li>
    </ul>
    
    <!-- 第三种方式，获取 key 和 value 以及 index -->
    <ul>
    	<li v-for="(value, key, index) in info">{{value}} - {{key}} - {{index}}</li>
    </ul>
</div>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        info: {
            name: 'swk',
            age: 18,
            gender: '男',
        }
    }
})
```







> 提示

上述案例都是通过对 Vue 实例中，添加对应的 property，再将 html 中的元素连接到 property 中，使得 html 中的内容来源于 property，并且会随着 property 的值的改变而改变。我们可以在控制台中输入，比如：`app.seen = false` ，那么它将会随着原型链向底层寻找 `seen` 这个属性并改变，改变后的属性会立刻响应到页面中，页面中的 `现在可以看到该元素` 这段内容将会消失；而 `app.todos.push()` 也可以向 Vue 实例中的 `todos` 这个数组添加内容，最后渲染到页面中的内容就会显示添加后的内容了。





### 处理用户输入

> v-on

`v-on` 是事件监听，它将会对选定元素的指定事件进行监听。例如输入框，输入框拥有内容改变，文本输入等事件，那么我们对其中的一个事件（文本输入事件 input）进行监听并绑定对应的处理方法，那么在输入框中输入任何文本，都会触发此方法。例如：

```html
<div id="app">
    <input type="text" v-on:input="content_change">
    <p>{{ content_length }}</p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        content_length: 0
    },
    methods: {
        content_change(event) {
            this.content_length = event.target.value.length
        }
    }
})
```

上面的案例产生的效果是：在输入框中输入任意内容，能够获取输出内容的长度，并且在下方进行显示。其中，p 标签中的 `content_length` 的值来源于 Vue 实例 data 中的值，然后在 input 标签中，对它的 input 事件进行监听（input 事件是文本框的输入事件），然后绑定到对应的变量（content_change）中，而我们在 Vue 实例中，添加了方法列表，其中一个方法就是 `content_change` ，并且直接执行（这里有一点小区别在下面进行讲解），函数执行会获得一个隐含的形参 `event` ，它将会传递到函数中，我们可以通过这个隐含的参数来获取 input 事件的一些信息。我们将输入框中的值的长度（`event` 传递进来的一些信息，我们可以通过这些信息获取输入框的值）赋值给 this （也就是该 Vue 实例）的 `content_length` ，那么它将会随着原型链自上向下寻找 `content_length` 这个 property ，并对其进行赋值操作。

**v-on参数**

如果该方法不需要额外参数，那么方法后的小括号可以不加，如果需要同时传入某个参数，同时需要 event 时，可以通过 `$event` 传入事件，但是如果方法本身中只有一个参数，那么会默认将原生事件 event 参数传递进去。

**v-on的修饰符**

`.stop`：调用 event.stopPropagation()，能够阻止事件的冒泡。例如：

```html
<!-- 该种情况会产生冒泡行为，也就是当按钮被点击时，div 也会触发点击事件 -->
<div @click="divClick">
    <button @click="btnClick">按钮</button>
</div>

<!-- 需要改为以下代码，才会让按钮被点击时，不触发其他事件 -->
<div @click="divClick">
    <button @click.stop="btnClick">按钮</button>
</div>
```

`.prevent`：调用 event.preventDefault()，能够阻止默认事件。例如：

```html
<!-- 遇到此种情况，点击按钮会自动提交 -->
<from action="baidu.com">
    <input type="submit" value="提交" @click="submitClic">
</from>

<!-- 需要为 submit 绑定点击事件并给定一个修饰符 prevent 即可取消默认的提交事件 -->
<from action="baidu.com">
    <input type="submit" value="提交" @click.prevent="submitClic">
</from>
```

`.(keyCode | keyAlias)` 只当事件是从键盘特定键触发时才触发回调。例如：

```html
<!-- 监听键盘某个键按下 -->
<input type="text" @keyup.enter="keyUp">
<!-- 这里给 keyup 键盘弹起进行绑定事件，并且绑定一个 keyUp 的函数，对其进行修饰，这里使用了 .enter 也就是回车键，最后达到的效果是按下回车即可触发事件 -->
```

`.once` 只触发一次回调。







> v-model

`v-model` 是双向绑定的操作，也就是说将数据绑定到视图模型中，再将视图模型与视图进行绑定，从而达到数据影响视图的效果。我们可以通过输入框，或其他方法改变原有的数据，而这些被改变的数据会自动的影响视图中显示的效果。例如：

```html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
```

在 html 中，通过上述的操作，即可将输入框中的值绑定到 p 标签中，也就是说，在输入框中输入任何内容，都会实时的显示到 p 标签中。接下来就是 js 代码：

```js
var app = new Vue({
    el: '#app',
    data: {
        message: "hello Vue!"
    }
})
```

通过上述代码，input 中内容的任何改变都将会影响 p 标签显示的内容。

`v-model` 同样也可以在 textarea 元素中使用。

**v-model 的原理**

`v-model` 其实是一个语法糖，背后实质上是包含两个操作：`v-bind` 和 `v-on`。对 input 元素绑定 `v-model` 的话，它将会对数据模型实现双向绑定，也就是改变 input 中的内容，也会对应改变 Vue 实例中的数据，而改变了 Vue 实例中的数据，也会改变 input 中的内容。所以我们可以通过对 input 绑定 `v-model` ，然后影响视图模型（就是 Vue 实例中的数据），然后视图模型回显到其他地方。

将 `v-model` 拆分可以写成以下方式：

```html
<div id="app">
    <input type="text" :value="message" @input="getValue">{{message}}
</div>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        message: 'hello Vue!'
    },
    methods: {
        getValue: function(event){
            this.message = event.target.value
        }
    }
})
```

上述的例子中，是通过对 input 的 `value` 进行绑定，将 Vue 实例中的数据绑定到 input 的value 中，然后监听 input 的 `input` 属性，该属性是 input 中的输入的属性，将其对应一个函数，代表输入了内容则执行该函数，而我们对这个函数进行处理，在 input 输入事件触发时，会传递进来一个 event 事件，event 事件包含输入事件（`input` 属性触发）中的一些事件对象，事件对象中就包含了输入框中最新的文本数据，我们将这最新的数据绑定到 Vue 中的 `message` 内（因为 input 绑定了 `message` ，所以它改变，对应的一些内容也会改变）。

我们还可以通过不对 Vue 去特定绑定一个方法的方式：

```html
<div id="app">
    <input type="text" :value="message" @input="message = $event.target.value">{{message}}
</div>
```

通过这段 html 中的代码，在 js 代码中的 Vue 实例，就不需要去用一个函数来处理 `input` 事件所进行的操作了。



> v-model: radio

使用 `v-model` 对 radio 也可以进行双向绑定，并且可以不使用 name 属性来进行单选框的互斥（不使用 `v-model` 需要使用 name 属性来进行互斥），由于是双向绑定，我们还可以在 Vue 实例中对特定的值给一个默认值，同样会绑定到选项中。例如：

```html
<div id="app">
    <label for="male">
        <input type="radio" id="male" value="男" v-model="sex">男
    </label>
    <label for="female">
        <input type="radio" id="female" value="女" v-model="sex">女
    </label>
    <h2>你选择的性别是：{{sex}}</h2>
</div>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        sex: '男'
    }
})
```

上面代码中，对 `v-model` 绑定一个 property ，它会对 Vue 实例中的 `sex` 属性进行双向绑定，并且不需要 name 属性就可以达到互斥的效果。



> v-model: checkbox

复选框分为：单选框和多选框。

单选框，我们使用 `v-model` 绑定一个属性，该属性为布尔值即为单选框。

多选框，我们使用 `v-model` 绑定一个属性，该属性为数组类型即为复选框。





> v-model: select

select 也分为单选和多选。

使用 `v-model` 绑定到 select 中，那么它下面的 option 选项所选择的值，会绑定到 Vue 实例的数据中，我们也可以通过对 property 给定默认值，使选项能够默认选中某一项。

我们还可以对其绑定多选，不过 property 需要使用数组类型，并且需要在 select 后增加 multiple 这个属性即可。

```html
<!-- 单选 -->
<select v-model="select1">
    <option value="apple">苹果</option>
    <option value="banana">香蕉</option>
    <option value="orange">橘子</option>
</select>

<!-- 多选 -->
<select v-model="select2" multiple>
    <option value="apple">苹果</option>
    <option value="banana">香蕉</option>
    <option value="orange">橘子</option>
</select>
```

```js
data: {
    select1: '',		// 单选
    select2: []			// 多选
}
```



#### 值绑定

上述的一些案例中，我们对一些选项进行定义，我们都是直接给定内容写死，而在真实的开发中我们并不能这样做，通常是通过后台服务器获取数据，将获取到的数据动态渲染到选项中，所以有了值绑定的这个概念。例如：

```html
<div id="app">
    <label v-for:"item in originHobbies" :for="item">
        <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
    </label>
</div>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        hobbies: [],
        originHobbies: ['篮球', '足球', '乒乓球', '羽毛球', '台球', '高尔夫球']
    }
})
```

对于上述案例，我们实际不能通过此方式直接指定数组中的内容，我们一般是使用对象来包裹这些内容，因为在 html 代码中，id 不能为中文，所以我们在此方式中指定 item 为 id 实际是不可取的，一般是由对象包裹，指定 id 以 `item.id` 这种方式。

Vue 实例中的 `originHobbies` 我们可以通过从服务器获取的方式，来指定它的内容，拿到了内容以后，通过对 label 标签的循环渲染，将其渲染出对应的数据项。

所以，**值绑定也就是动态的给 value 绑定值**，通过 `v-bind:value` 的方式。





**v-model 的修饰符**

`.lazy` ：使 `v-model` 的更新频率没有那么快。默认情况下，`v-model` 是在 input 事件中同步输入框数据的，一旦有对应的 data 中的数据发生改变，那么 `v-model` 绑定的对应输入框会自动发生改变。该修饰符可以使数据在失去焦点或者敲下回车时才对数据进行更新。

`.number` ：使改变的数据为 number 类型。我们对输入框进行绑定，每次改变输入框中的内容，输入框都会进行更新，而无论输入什么，更新的内容都是一个 string 类型的数据，即使我们将 type 修改为 number，Vue 也会将获取到的内容作为 string 类型来赋值给 data 中的对应 property，所以我们需要使用该修饰符，来使 Vue 对 data 中的 property 赋值为 number 类型。

`.trim` ：会自动将数据两边的空格进行去除。







### 语法糖

就是一个指令的简写方式。

`v-bind` 的语法糖为：`:`（就是一个冒号）。

`v-on` 的语法糖为：`@`。

例如： `<a v-bind:href="url"></a>` === `<a :href="url"></a>` 两种方式都是一样的效果，第二种为语法糖的方式。







## Vue - 虚拟DOM

### 组件的key属性

官网推荐我们在使用 `v-for` 时，给对应的元素或组件添加一个 `:key` 属性。

`:key` 值的存在，可以保证组件不被复用，如果没有 `:key` 的存在，则组件会被复用。

使用 key 这个属性与 Vue 的虚拟 DOM 的 Diff 算法有关。当某一层有很多相同的节点时，也就是列表节点时，我们希望插入一个新的节点，例如在 A B C D E 这5个元素中，在第3个元素的位置（B 和 C）插入 F 元素，Diff 算法默认执行起来会将 C 更新为 F，将 D 更新为 C，将 E 更新为 D，最后插入 E，这么一个过程，实际上是很没有效率的。

所以我们需要使用 key 来给每个节点做一个**唯一标识**（保证 key 的唯一性），使 Diff 算法能够正确的识别节点并找到正确的位置插入新的节点。

==key 的作用主要是为了高效的更新虚拟 DOM。==

**理解：**在进行页面渲染时，Vue 会将即将收集到的内容，渲染到虚拟 DOM 中，并且在虚拟 DOM 中进行一些数据的比对（例如计算属性的缓存），最后再进行页面渲染。而这里使用 key 属性（唯一）的话，会使 key 与 DOM 中的元素对应起来，而如果此时需要在 DOM 中插入元素，例如在无序列表中的 li 插入一个元素，那么它会对 key 绑定的 DOM 元素进行检测，发现它们一直都处于对应的状态没有改变，则直接在它们中间进行元素的插入；否则 Vue 将会对插入位置后面的元素进行重渲染，也就是从插入的位置开始，一直到结尾的元素，都会重新计算进行渲染，这大大降低了性能。

对于 `v-for` 指令，我们需要给定一个唯一的 key，使其数据发生变化时不会重绘，由于 id 是唯一的，所以建议给其一个 id `:key` = `item.id`。



### 响应式渲染

响应式是 Vue 最独特的特性之一。

对于数组的响应式，可以通过常规的一些方法来达到。

但是通过索引值来修改数组中的元素并不会响应式渲染。例如：`data[0] = 'vue'` ，通过此方式对数组进行修改，数组会被修改，但不会响应的显示，因为 Vue 内部没有对这种方式进行监听，所以通过此方式不会渲染虚拟 DOM。不过 Vue 提供了一个方法给我们来对对象进行操作。`Vue.set()`，传入的参数为3个，分别是：set(要修改的对象, 索引值, 修改后的值)，此种方式可以达到响应式的效果。







## 计算属性

### 什么是计算属性？

模板的表达式非常便利，但是更适合用于简单的运算，放入太多的逻辑会让模板变得很难维护（也就是变得很长很难看）。

我们可以通过一个函数来对原始值进行计算，返回一个计算后的值。



### 如何使用计算属性

在 Vue 实例中，使用 `computed` 即可，与 `methods` 不同，计算属性在调用的地方不需要加小括号。

我们通常可以使用模板语法来进行一些变量的拼接，但是这样对于过于复杂的值，我们需要通过很多的操作来进行拼接，这大大降低了维护性。

通常我们会使用如下几种方法来进行模板内容的拼接：

```html
<p>{{firstName + ' ' + lastName}}</p>
<p>{{firstName}} {{lastName}}</p>
```

同样也可以通过方法返回值的形式来得到内容：

```html
<p>{{getFullName()}}</p>
```

```js
methods: {
    getFullName(){
        return this.firstName + ' ' + this.lastName
    }
}
```

该种方法同样可以得到相同的值。但是在写法上，调用方法与计算属性为了更好的区分，归类写会比较好，在对代码进行修改时，可以通过语义来对代码进行理解。

以下是通过计算属性来得到的结果：

```html
<p>{{fullName}}</p>
```

```js
computed: {
    fullName: function(){
        return this.firstName + ' ' + this.lastName
    }
}
```

计算属性其实更偏向强调的是属性这个概念，所以在 html 代码中，没有函数调用的括号，而只是单纯的一个变量名，但是实质还是通过调用函数获得返回值的形式。





### setter 和 getter

每个计算属性都包含一个 getter 和 setter。例如：

```html
<p>{{ fullName }}</p>
```

```js
computed: {
    fullName: {
        set: function(){
            
        },
        get: function(){
            return 'abc'   // html 页面中会显示这里所给的值
        }
    }
}
```

以下是计算属性的简写方式，不过我们一般都采用上述的完整方式。

```js
computed: {
    fullName: function(){
        return firstName + ' ' + lastName
    }
}
```

其实，计算属性本质会调用对应变量的属性中的 `get` 方法，而我们在其内部对 `get` 方法进行设定，能够让使用对应变量的计算属性拿到设定的值。计算属性一般是没有 `set` 方法的，它是一个只读属性，所以我们一般都会将其简写为下面这种方式，而不对其设置 `set` 方法。`set` 方法是监视 `fullName` 的值，当 `fullName` 的值发生改变时，会自动回调 `set` 方法，所以我们可以在 `set` 方法中进行对应的操作，以达到值被修改时，自动调用 `set` 里方法的效果。

在 `set` 方法中，形参中会接受到新值。





### 计算属性缓存

**计算属性和 methods 对比**

通过 `methods` 进行属性的计算操作的话，是不会有缓存的，也就是你调用了几次函数，它就会计算几次；而通过计算属性进行属性的计算操作的话，它是会有缓存的，你调用了几次函数，它会将值进行缓存，如果值发生改变则对返回值进行改变处理，如果最终值没有改变，则不对其进行重复计算（`methods` 无论多少次，都会重复进行计算）。

可以通过以下例子来进行演示：

```html
<div id="app">
    <p>{{ getFullName() }}</p>
    <p>{{ getFullName() }}</p>
    <p>{{ getFullName() }}</p>
    <p>{{ getFullName() }}</p>
    -----以下是计算属性-----
    <p>{{ fullName }}</p>
    <p>{{ fullName }}</p>
    <p>{{ fullName }}</p>
    <p>{{ fullName }}</p>
</div>
```

```js
var app = new Vue({
    el: '#app',
    data: {
        firstName: 'Kobe',
        lastName: 'Bryant',
    },
    methods: {
        getFullName: function(){
            console.log('调用了getFullName()方法');
            return this.firstName + ' ' + this.lastName;
        }
    },
    computed: {
        fullName: function(){
            console.log('调用了计算属性');
            return this.firstName + ' ' + this.lastName;
        }
    }
})
```

通过上述的代码，进行调试，可以在网页中的控制台看到结果，语句“调用了getFullName()方法“会输出4遍，而语句”调用了计算属性“只会输出一遍。因为使用计算属性时，它会将最后返回的结果进行缓存，在进行下一次调用时，它会将下一次调用的结果与缓存中的结果进行对比，如果没有改变则直接输出缓存中的结果，如果有改变则重新计算；而 `methods` 不会进行缓存，它将会每次都计算调用的结果，无论上一次与这一次的结果是否一样，所以 `methods` 的性能是比较低的。也可以认为计算属性是对值进行了监听，如果值一样则不对缓存进行改变，如果值不一样则清空缓存重新计算，监听也会耗费资源，但是耗费的资源很少，却可以大大提升效率。







## 补充使用到的ES6语法

### 块级作用域

在 ES5 之前因为 if 和 for 都没有块级作用域的概念（js 的设计缺陷，设计之初没有考虑），所以在很多时候，我们必须借助于 function 的作用域来解决外面变量的问题（ES5 之前只有 function 有作用域，所以我们使用 var 来定义变量的话，出现一些作用域的问题我们就需要使用 function 的作用域来解决）。在 ES6 中，加入了 let，let 它是有 if 和 for 的块级作用域的。

==ES5 中的 var 是没有块级作用域的（if/for）；ES6 中的 let 是有块级作用域的（if/for）。==

1. **变量作用域**：变量是在什么范围内可用。

2. **没有块级作用域引起的问题**：会使在代码块内部定义或改变变量的值，在外面会收到改变后的值。也就是说假如我在一个代码块内部定义了一个变量 `func` ，给其赋值了一个函数，我本来的目的是想让其只在该代码块定义并且调用，但是我在代码块外部也可以对其进行调用、修改，这可能导致有的变量可能在某些值传递时，已经被修改了，污染全局变量等情况。

3. **没有块级作用于引起的问题——for的块级**：在初次接触 js 时，使用 for 循环我们都是使用 var 来定义初值，那么这样会引起的问题是，假如我们在 for 内部循环绑定了按钮的监听事件，并且为其绑定一个 for 循环中用 var 定义的变量（假设 for(var i = 0;……)），那么我们在它内部为每个按钮绑定了一个监听事件并且点击输出第 i 个按钮被点击的字符串 `console.log('第' + i + '个按钮被点击')`，那么它永远都会输出循环最后的那个值，那么这时候为了解决这个问题，我们需要使用到闭包的方式来解决。所以我们一般都使用 let （ES6语法）来进行定义。

   可以通过该例进行对比：

   ```js
   const btns = document.getElementByTagName('button');
   for(var i = 0; i < btns.length; i++){
       btns[i].addEventListener('click', function(){
           console.log('第' + i + '个按钮被点击');
       })
   }
   // 将 for 中的 var 改成 let 即可，否则需要使用闭包的方式。
   ```



### let/var

前提：在 html 中定义了三个按钮，想要为三个按钮分别绑定一个函数，并且点击时输出是第几个按钮。

**情况一：ES5 中没有使用闭包（错误的方式）**

```js
var btns = document.getElementByTagName('button');
for(var i = 0; i < btns.length; i++){
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
```

上述的代码实际不会对 for 中的代码没有作用域，由于我们点击按钮实际上是一个回调函数，所以在我们点击时，才会调用内部的函数，而调用内部的函数时，它会去找对应的变量，而该种情况没有作用域，它实际是以下情况：

```js
i = 3;
{
    // i = 0; // 第一次循环会在此进行定义，第一行的代码此时为0
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
{
    // i = 1; // 第二次循环会在此进行定义，第一行的代码此时为1
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
{
    // i = 2; // 第三次循环会在此进行定义，第一行的代码此时为2
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
i++;
// 所以最终 i 会等于3，我们通过点击按钮得到的值为3，上面3个代码块中都没有自己的作用域，都会对第一行的 i 的值进行改变，并且取的也是第一行 i 的值
```

该种情况，它实际改变的是第一行定义的值，而第3、9行代码改变的值无效，都会被最后第15行代码改变的值覆盖掉，所以，当我们点击按钮时，它在自身的代码块中，找不到对应的值，而找到的是第一行 i 的值，而此时 i 已经被改为了3（因为最后有个i++，执行了所有循环后进行了i++这个操作，所以为3），所以最后点击任何一个按钮，输出的结果都是3。



**情况二：ES5 中使用闭包（可以实现）**

```js
var btns = document.getElementByTagName('button');
for(var i = 0; i < btns.length; i++){
    (function(i){
    	btns[i].addEventListener('click', function(){
        	console.log('第' + i + '个按钮被点击');
    	})
    })(i)
}
```

该代码实际在执行时，它会变成以下情况：

```js
i = 3;
// 第一次执行的时候
function(i){
    i = 0; //外部传进来的
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}(0) // 传递到第3行的形参中

// 第二次执行
function(i){
    i = 1; //外部传进来的
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}(1) // 传递到第11行的形参中

// 第三次执行
function(i){
    i = 2; //外部传进来的
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}(2) // 传递到第19行的形参中
```

之所以闭包可以解决这个问题，因为函数它内部是有作用域的，因为我定义形参了以后，那么在我这个函数的内部，是有一个作用域的。我通过循环进行调用并且把值传递了进去，那么内部的函数接收到了传递的值，它的形参对其进行了定义，在函数的内部有对应的值，无论外部的值怎么变化，函数内部有自己的作用域不会受到外部值的影响（因为在 for 循环时，调用函数时传进去的参数已经是确定的了）。



**情况三：ES6 中使用 let**

```js
const btns = document.getElementByTagName('button');
for(let i = 0; i < btns.length; i++){
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
```

使用 let 来对 for 进行定义，最终会变成以下效果：

```js
i = 3;
{
    i = 0; // 作用域内有属于自己的i，所以这里的 i 只对2到7行代码有效
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
{
    i = 1; // 作用域内有属于自己的i，所以这里的 i 只对8到13行代码有效
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
{
    i = 2; // 作用域内有属于自己的i，所以这里的 i 只对14到19行代码有效
    btns[i].addEventListener('click', function(){
        console.log('第' + i + '个按钮被点击');
    })
}
```



### const 的使用

使用 const 修饰的标识符为常量，不可以再次赋值。当我们修饰的标识符不会被再次赋值时，就可以使用 const 来保证数据的安全性。

**建议：**在 ES6 开发中，优先使用 const，只有需要改变某一标识符的时候才使用 let。

**注意一：一旦给 const 修饰的标识符被赋值之后，不能修改。**

**注意二：在使用 const 定义标识符，必须进行赋值。**

**注意三：常量的含义是指向的对象不能修改，但是可以改变对象内部的属性。**





### 对象增强写法

**属性的增强写法**

```js
const name = 'why';
const age = 18;
const height = 1.88;

// ES5 属性的写法：
const obj = {
    name: name,
    age: age,
    height: height,
}

// ES6 属性的写法：
const obj = {
    name,
    age,
    height,
} // 它会自动将变量名作为 key，值作为 value
```



**函数的增强写法**

```js
// ES5 函数的写法：
const obj = {
    run: function(){
        
    },
    eat: function(){
        
    },
}

// ES6 函数的写法：
const obj = {
    run(){
        
    },
    eat(){
        
    },
}
```



### 可变参数

定义参数时，可以在形参中使用 `...data` 这种形式，调用时可传入多个参数，并且传入的这些参数会自动创建一个数组保存。例如：

```js
function sum(...num){
    let result = 0;
    for(let i = 0; i < num.length; i++){
        result += num[i];
    }
    return result;
}
```





## 组件化开发

**可以理解为搭积木。**

理解：将小的东西组合成一个整体，而组成的这个整体又是组成完整物体的一部分。我们需要将一些元素，赋予其对应的属性或样式等，再将这些元素拼凑成一个完整的、可定制的一个组件，可以参考类的思想，将组件比作一个类，我们对这个类定义一些属性或方法（对元素定义一些操作或样式等），最后封装，需要时直接调用。

如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后期的维护和扩展。如果我们将页面拆分成一个个的小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后的整个页面维护和扩展就变得非常容易了。

每个页面就是一个模块，而且我们将一个完整的页面拆分成很多个组件，每个组件都用于实现页面的一个功能块，而每个组件又可以进行细分。



### Vue 组件化思想

组件化是 Vue.js 中的重要思想。它提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。任何的应用都会被抽象成一棵组件树。有了组件化的思想，我们就要充分的利用它。尽可能的将页面拆分成一个个小的、可复用的组件，这样让我们的代码更加方便组织和管理，并且扩展性也强。



### 注册组件的基本步骤

组件的使用分成三个步骤：

1. **创建组件构造器**：调用 Vue.extend() 方法**创建组件构造器**。

   ```js
   const cpnConstructor = Vue.extend({
       template: `
   	<div>
   		<h2>title</h2>
   		<p>这是第一行</p>
   		<p>这是第二行</p>
   	</div>`
   })
   ```

   调用 Vue.extend() 创建的是一个组件构造器。通常在创建组件构造器时，传入 template 代表我们自定义组件的模板。该模板就是在使用到组件的地方，要显示的 html 代码。（不过此种写法已经被淘汰，我们将使用语法糖的形式，不过底层还是使用此种方法实现的）

2. **注册组件**：调用 Vue.component() 方法**注册组件**。

   ```js
   Vue.component('my-cpn', cpnConstructor)
   ```

   调用 Vue.component() 是将刚才的组件构造器注册为一个组件，并且给它起一个组件的标签名称，所以需要传递两个参数：注册组件的标签名和组件构造器。

3. **使用组件**：在 Vue 实例的作用范围内**使用组件**。

   ```html
   <div id="app">
       <my-cpn></my-cpn>
       <my-cpn></my-cpn>
       <my-cpn></my-cpn>
       <my-cpn></my-cpn>
       <!-- 会显示四次模板中的内容 -->
   </div>
   ```



**注册组件语法糖**

全局注册：

```js
Vue.component('cpn1', {
    template: `
		<h2>我是标题</h2>
		<p>我是第一行</p>
		<p>我是第二行</p>`
})
```

局部注册：

```js
components: {
    'cpn2': {
        template: `
			<h2>我是标题</h2>
			<p>我是第一行</p>
			<p>我是第二行</p>`
    }
}
```

Vue 为了简化这个过程，提供了注册的语法糖，主要是省去了调用 Vue.extend() 的步骤，而是可以直接使用一个对象来代替。









### 全局组件和局部组件

**全局注册**

全局注册意味着在全局注册的组件，可以在多个 Vue 的实例下面使用，直接在 script 标签下使用 Vue.component() 注册全局组件。

**局部注册**

局部注册是在某个 Vue 实例中对组件进行注册，注册的组件只对局部有效，在 Vue 实例中使用 components 属性进行局部注册，key 是组件标签名，value 是组件构造器。







### 父组件和子组件

每个对象都可以看做是一个组件，而在该对象中的构造并注册使用的是该对象的子组件。例如：

```html
<div id="app">
    <cpn2></cpn2>
</div>
```

```js
// 定义组件1的模板
const cpnconstructor1 = Vue.extend({
    template: `
		<div>
			<h2>我是标题</h2>
			<p>我是第一行</p>
			<p>我是第二行</p>
		</div>
	`
})

// 定义组件2的模板
const cpnconstructor2 = Vue.extend({	// 父组件
    template:`
		<div>
			<h2>我是标题</h2>
			<p>我是第一行</p>
			<cpn1></cpn1>
		</div>
	`,
    components: {				// 在组件内部注册的组件为子组件
        cpn1: cpnconstructor1			// 子组件
    }
})

// Vue 实例化
const app = new Vue({
    el: '#app',
    components: {
        cpn2: cpnconstructor2
    }
})
```

上述代码中，在 Vue 实例中局部注册了组件 cpn2，并且在组件 cpn2 的模板中定义了组件 cpn1 的模板，并且在它内部对 cpn1 进行了注册，所以在这里 cpn2 是 cpn1 的**父组件**，cpn1 是 cpn2 的**子组件**，因为 cpn1 是在 cpn2 内部进行注册的，所以只能在 cpn2 的模板中进行使用，脱离了 cpn2 无法使用，除非在 Vue 实例中再进行注册。但是其实，new Vue 实例中的对象也可以看做是一个组件（el: '#app' 的这个对象），它可以看做是组件 cpn2 的父组件，Vue 实例可以称为 root 组件（根组件）。

假如我们通过上种注册方式注册了局部组件以后，我们再在 html 页面中使用组件 cpn1 的话，是无法使用的（会报错），因为 Vue 在对内容进行解析时，遇到组件时首先会在自身进行寻找，没有找到则向上一级（父级）寻找，一直向上寻找，如果没有找到则报错，找到了则直接将定义好的模板对内容进行替换，它不会向子组件或子节点中进行寻找。

组件和组件之间存在层级关系，而其中一种非常重要的关系就是父子组件关系。







### 组件模板抽离

我们通常在 js 代码中使用 template 模板（也就是 html 标签），这样会显得很乱，而且代码不好维护，所以我们通过将模板抽离出去，使其在其他地方定义，在 js 中只需要声明一下就行了。有两种方案来定义 html 模板内容：

**使用 script 标签**

使用此方式，需要将 script 的 type 属性改为 `type="text/x-template"`。

```html
<script type="text/x-template" id="cpn1">
	<div>
		<h2>我是标题</h2>
		<p>我是第一行</p>
		<p>我是第二行</p>
	</div>
</script>
```



**使用 template 标签**

```html
<template id="cpn2">
    <div>
    	<h2>我是标题</h2>
		<p>我是第一行</p>
		<p>我是第二行</p>
    </div>
</template>
```



使用上面两种方式都可以达到定义模板的效果，我们需要对其绑定 id 属性，然后在 Vue 中需要使用到模板的地方，直接将 id 填写上去即可。如：

```js
Vue.component('cpn1', {
    template: '#cpn1',		// 这里为你定义模板的id
})
```

**注意：**使用 script 标签对模板进行定义时，需要将 type 进行更改！！并且定义模板时，**需要用 div 将它们包裹起来！！**





### 组件中的数据

在上面的组件模板中，我们的数据都是写死的，这样非常不友好，所以我们需要使组件中也能使用 Mustache 语法。在 Vue 实例中我们是使用 data 对象的方式来进行传值，而在组件中我们不能这样使用，因为子组件是不能引用父组件的数据的，并且假如我们一个页面有成千上百个组件，它们的数据都写在 Vue 实例的 data 中，那么会使整个代码变得臃肿，难以维护，所以我们组件化的思想就是将这些东西分离，组件中的 data 数据就在组件内部来保存。

我们通过以下方式来使用组件的数据：

```js
Vue.component('cpn', {
    template: '#cpn',
    data() {
        return {
            counter: 0
        }
    },
    methods: {
        increment() {
            this.counter++
        },
        decrement() {
            this.counter--
        }
    }
})
```

以上是一个计数器组件的代码，它使用了 data 函数来保存数，使用 methods 来操作按钮，和我们之前直接定义在 Vue 实例中是一样的方式，不过它保存在组件内部，与 Vue 实例分开了。

**这里为什么使用 data 函数返回值的形式来保存我们的变量？**因为函数有它自己的作用域，我们使用 data 函数返回值的形式来保存的话可以使我们的组件不会共享一个变量，假如有多个组件它们也都是在自己的内部空间使用自己的变量，不会出现因为一个组件中的变量改变，而影响另一个组件中的变量的情况。因为函数有自己的作用域，在函数内部定义的变量，不会与其他变量共享。

假如我们使用以下方式定义，则会造成数据共享，也就是同时使用多个组件时，他们之间的值会互相影响，这样很不好。

```js
const obj = {
    counter: 0
}

Vue.component('cpn', {
    template: '#cpn',
    data() {
        return obj
    },
    methods: {
        increment() {
            this.counter++
        },
        decrement() {
            this.counter--
        }
    }
})
```







### 父子组件的通信

在真实开发中，组件之间的通信是无处不在的。我们访问一个网页，它的信息基本都是通过请求服务器获取的，而我们请求到的数据则需要向各个组件进行传递，而这个过程就是组件之间的通信。在 Vue 中，通常一个页面中有许多的组件，而我们整个页面可以看成是根组件，根组件中又有许多一级子组件，一级子组件中可能又有多个二级子组件...以此类推。请求服务器获得的数据一般是保存在根组件中（Vue 实例的 data 中），如果不保存在根组件中的话，每个组件每次载入都需要请求一次数据，这样会造成服务器压力过大，那么我们的数据保存在根组件中的话，就需要它把子组件需要的数据向下传递，而 Vue 中父子组件的通信是不同的。

**父传子** props

我们在父组件中定义了数据，通过子组件中的 `v-bind` 绑定到子组件，也就是说 `v-bind` 绑定的是子组件的变量，而子组件通过 props 来接收父组件传递进来的数据，相当于形参。传递进来的数据它会自动放进子组件的 data 函数的返回对象中。

如： `<cpn v-bind:cmessage="message"></cpn>` ，该代码是定义在 html 中的代码，是子组件的一行代码，而这其中的 `cmessage` 是子组件中 props 内的变量，用来接收父组件传递进来的数据，而 `message` 则是父组件中的数据。

完整代码如下：

```html
<div id="app">
    <cpn v-bind:cmessage="message"></cpn>
</div>

<template id="cpn">
    <div>
        <h2>{{cmessage}}</h2>
    </div>
</template>
```

```js
const cpn = {
    template: '#cpn',
    props: ['cmessage'],
    /*
    props: {
        cmessage: String
    }
    */
    /*
    props: {
    	cmessage: {
	    	type: String,
	    	default: 'hello Vue!',
	    	required: true
    	}
    }
    */
    data() {
        return {}
    },
}

const app = new Vue({
    el: '#app',
    data: {
        message: 'abcabc',
    },
    components: {
        cpn
    }
})
```

上面代码中，html 代码的第2行，`cmessage` 是子组件中的变量，用来接收父组件的数据，而 `message` 则是父组件中的变量，第7行代码则是使用了 `cmessage` 也就是子组件中的变量，它将显示从父组件传递过来的数据内容，整个数据传递的路径是：data 保存在 `message` 中，传递到 `cmessage` 中，然后保存在 props 中，然后从 props 中传递到第7行的 `message` 中。而 js 代码中，cpn 内的 props 类似于形参，放置在该位置等待变量的传入，而 props 可以有几种写法，第3行的写法是最简单的写法，直接接收数据传递；第4到8行则是使用对象接收传递进来的数据，并且可以对其做数据类型的限制；第9到第17行不仅对其数据类型做了限制，还对它的默认值进行了设置（也就是没有传值过来时显示的默认值），以及该变量是否必须进行了限制，为 `true` 表示必须，否则会报错。

注意：如果使用第9到17行的方式，type 为对象（数组、函数和对象都属于对象类型）类型，那么 default 就必须为一个工厂函数，否则会报错。也就是使用一个函数来返回一个对象。`default() { return [ ] }`

**驼峰标示**

在 html 代码中，Vue 不支持驼峰命名法，例如：`<cpn :cInfo="info"></cpn>`，这种方式是不支持的，不能够获取到数据，必须将驼峰改为横线：`<cpn :c-info="info"></cpn>`，此种方式才行，在 js 文件中正常使用驼峰命名就行了。html 中使用横线命名对应 js 中 驼峰命名：（js 中命名）`cInfo` => `c-info`（html 中命名）。







**子传父** $emit

在真实的开发中，我们可能会遇到在子组件中点击了某一项内容，而产生对应的内容的操作，它其实是通过子组件的发生的事件，传递对应的数据给父组件，再由父组件向服务器去请求对应数据，再返回给子组件，这就需要子组件向父组件传递对应的数据。

子传父需要用到自定义事件，由于子组件不能访问父组件的数据，所以我们需要用到一个事件将数据发射出去，它就是 `$emit`。

在子组件中，我们通过 methods 函数，在函数的内部增加一行代码，这行代码就是子组件向父组件发送需要的数据的代码。如：

```js
const cpn = {
    template: '#cpn',
    data() {
        return {
            obj: {
            	name: 'swk',
            	age: 18,
            }
        }
    },
    methods: {
        send() {
            this.obj.name = 'zbj'
            this.$emit('obj-info', this.obj)
        }
    }
}
```

通过以下代码，就完成了子组件向父组件发送数据，而子传父的核心代码就是14行的这段代码，第一个参数为自定义事件的名字，第二个参数是发送的内容。

而我们通过子组件已经向父组件发送了数据，父组件怎么才能获取到呢？在这里，父组件获取子组件的数据通过监听子组件的自定义事件的名字，由于子组件向父组件传递数据是通过自定义事件进行传递的，并且给定了一个事件名，所以此时父组件已经拿到了这个事件名，我们将监听它，使用 `v-on` 指令。如下：

```html
<cpn @obj-info="info"></cpn>
```

该代码中 `@objInfo` 是子组件发射进来的事件，如果它有参数，则会默认传递进来了参数，并且会默认在父组件接收时会传递进来（由于该事件不是 window 的默认事件，也就是它不会传入 event 事件，所以我们这里不加括号获取的是子组件传递进来的数据）。如下：

```js
methods: {
    info(obj) {
        console.log(obj);
    }
}
```

该代码中，`obj` 为子组件传递进来的数据，所以我们可以直接获取到。

以下是一个完整的实例：

```html
<div id="app">
  <cpn @increment="increment" @decrement="decrement"></cpn>
</div>
<template id="cpn">
  <div>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>
```

```js
const cpn = {
  template: '#cpn',
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    increment() {
      this.counter++
      this.$emit('increment', this.counter)
    },
    decrement() {
      this.counter--
      this.$emit('decrement', this.counter)
    },
  }
}

const app = new Vue({
  el: '#app',
  components: {
    cpn
  },
  methods: {
    increment(info) {
      console.log('收到了！counter为：', info);
    },
    decrement(info) {
      console.log('收到了！counter为：', info);
    },
  }
})
```

该完整的案例中，我们首先是定义了一个组件，它在 Vue 实例中进行了注册，所以是 Vue 实例的子组件，Vue 实例是父组件。然后我们在子组件中定义了两个按钮，一个加一个减，并且给他们赋予了点击事件，分别对应 `increment` 和 `decrement` 方法，所以点击按钮会分别触发这两个方法，两个方法中都有 `this.$emit()` 这个方法，该方法是自定义事件，发送给父组件，在这里我们定义了自定义事件的名字，分别为 `increment` 和 `decrement` ，并且传入了参数 `counter` ，该参数会随着自定义事件一起发送到父组件中，在父组件中我们使用了 `v-on` 来进行监听，也就是 html 代码中的第2行。分别监听两个事件，监听的这两个事件是来自子组件的自定义事件（监听一般都是监听 `click` `input` 等事件，这里监听 `increment` 是我们自定义的事件，由于它是一个事件我们就需要给它绑定一个对应的处理函数），右边是事件触发对应的处理函数（类似于 `@click="btnClick"` ，而这里是 `@increment="increment"` ，其实为了区别我们也可以写成 `@increment="custom"`，而这里的 `increment` 就是子组件的事件，`custom` 就是父组件调用的函数），我们在父组件中（Vue 实例中）绑定了对应的 methods 来处理对应的事件，这整个过程就是子组件向父组件传递数据。







### 父子组件的访问方式

父组件访问子组件使用 `$children` 或 `$refs` ，子组件访问父组件使用 `$parent`。

有时候，我们想要父子组件之间不再只是传输数据了，而是直接拿到一个对象或方法，并进行调用等操作，我们可以使用接下来的方法。

**父组件访问子组件**

`$children` 访问拿到的是一个数组，我们有多少个子组件，那么该数组就会有多少个，所以我们如果需要访问其中某个组件对象，需要通过索引值来进行访问。

但是这样就会遇到一个问题，假如我们在组件中间插入其他的组件，那么这个索引值就会打乱，从而导致访问到的不是原来想要访问到的组件对象，所以我们需要使用 `$refs` 的方式进行访问，该方式能够准确访问我们需要的组件对象。我们给需要拿到的组件一个 `ref` 属性，`ref` 属性的值为对象中的 key。例如：

```html
<div id="app">
    <cpn ref="abc"></cpn>
</div>
```

最后我们打印一下 `refs` ，通过 methods 打印，`console.log(this.$refs)`，可以输出得到一个对象 `{abc: VueComponent}`，我们拿到这个对象直接使用 `console.log(this.$refs.abc)` 即可。

而我们通常使用 `$children` 是用作遍历组件，用 `$refs` 拿到具体组件。





**子组件访问父组件**

通过 `$parent` 来访问父组件，通过 `$root` 来访问根组件（Vue 实例）。

如下代码：

```html
<div id="app">
    <cpn></cpn>
</div>

<template id="cpn">
    <div>
        <h2>我是cpn组件</h2>
        <ccpn></ccpn>
    </div>
</template>

<template id="ccpn">
    <div>
        <h2>我是子组件</h2>
        <button @click="btnClick">按钮</button>
    </div>
</template>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        message: '你好呀'
    },
    components: {
        cpn: {
            template: '#cpn',
            data() {
                return {
                    name: '我是cpn组件的name'
                }
            },
            components: {
                ccpn: {
                    template: '#ccpn',
                	methods: {
                        btnClick() {
                            console.log(this.$parent)
                            console.log(this.$parent.name)
                            console.log(this.$root)
                        }
                    }
                }
            }
        }
    }
})
```

该代码中，使用了三层嵌套：根组件（Vue 实例） -> cpn -> ccpn，由父到子的关系。我们在 ccpn 中打印 `this.$parent` 得到的是它的父节点 cpn，所以最后输出的结果会是 `VueComponent`，我们如果打印 `this.$root` 的话，则会输出 `Vue` ，因为 Vue 是根节点。但是如果我们只有两层嵌套的话，只有 Vue 和 cpn，cpn 是在 Vue 中的组件，我们在 cpn 中打印 `this.$parent` ，输出的则是 `Vue` 。

不过我们在实际开发中很少使用此种方式，因为此种方式代码耦合度太高了。









## 插槽slot

**组件的插槽是为了让我们封装的组件更加具有扩展性**。让使用者可以决定组件内部的一些内容到底展示什么。

类似于一些网站的导航栏，我们可以将其封装成一个组件。我们的组件可以在这些网站中进行复用，但是具体在每个网站中，还是会有一些不一样的功能。比如在第一个页面是一个搜索框加一个扫二维码，第二个页面则是一个返回按钮加一个搜索框等等这类，这时候就需要用到插槽了。我们可以将这些不同的地方，预留成插槽，当某个页面需要使用到某个功能模块时，接入到插槽中即可。

我们封装带插槽的组件最好是**抽取共性，保留不同**。





### 插槽的使用

直接在模板中定义一个 `<slot></slot>` 即可。例如：

```html
<template id="cpn">
    <div>
        <h2>我是标题</h2>
        <p>我是p标签</p>
        <slot></slot>
    </div>
</template>
```

直接像第5行代码这样定义即可，就代表我们在此放置了一个插槽。那么我们如何去使用这个插槽呢？（如何往插槽里放东西）直接向组件中间插入元素即可。例如：

```html
<div id="app">
    <cpn></cpn>
    <cpn>
        <div>我是div元素</div>
        <p>我是p元素</p>
        <i>我是i元素</i>
    </cpn>
    <cpn>
        <div>我是div元素</div>
    </cpn>
    <cpn></cpn>
</div>
```

我们像上面这样，往组件中间插入我们需要的标签即可。如果像第3行到第7行这里，我们在一个插槽中插入了多个元素，那么它将会一起作为替换元素（一起都会在组件的插槽中显示）。

假如说我们需要用到插槽，只需要在几个地方进行替换，而其他地方保持不变的话，我们可以为插槽添加一个默认值：

```html
<slot><button>按钮</button></slot>
```

例如此代码，在插槽中间给定我们需要的默认值。那么该代码，将会在上面代码中的第2行和第11行的组件中，显示 button 按钮，而第3行到第10行的两个组件不会显示 button 按钮，因为它们中间已经使用了插槽，所以不会显示插槽的默认值。







### 具名插槽

具名插槽就是具体名字的插槽。我们定义模板的时候，可以定义左边显示什么内容，中间显示什么内容，右边显示什么内容等。那么我们可以定义3个插槽来分别放置在左、中、右，那么就定义了3个插槽，如下：

```html
<template id="cpn">
    <slot><span>左边</span></slot>
    <slot><span>中间</span></slot>
    <slot><span>右边</span></slot>
</template>
```

按照上面的这个模板，我们如果直接在组件中使用插槽，那么它将会占用我们上面代码定义的3个插槽。此时我们需要它对号入座，而不是将所有的都占用。

我们通过给插槽指定一个 name 属性，来指定具体的名字，如下：

```html
<template id="cpn">
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
</template>
```

通过上述的方式，就给每个插槽指定了一个具体的名字，那么我们使用插槽时，如果要使用对应的插槽就需要指定一个 slot 属性，而 slot 属性的值就为插槽的 name 值，如下：

```html
<div id="app">
    <cpn><span slot="center">这是中间的内容</span></cpn>
    <cpn><button slot="left">这是左边的按钮</button></cpn>
</div>
```

按照上述的方式，我们使用了2个组件，第一个组件占用了插槽的中间部分，指定了一个 span 标签占用这个插槽，而第二个组件占用了插槽的左边部分，指定了一个 buuton 按钮占用了这个插槽。





### 编译作用域

在真正学习插槽之前，需要理解一个概念：编译作用域。

在 Vue 中，我们使用一些指令等操作，它都有自己的作用域，例如：

```html
<div id="app">
    <cpn v-show="isShow"></cpn>
    <cpn v-for="item in items"></cpn>
</div>

<template id="cpn">
    <div>
        <h2>我是子组件中的标题</h2>
        <p>我是子组件中的内容</p>
        <button v-show="isShow">按钮</button>
    </div>
</template>
```

```js
const app = new Vue({
    el: '#app',
    data: {
        message: '你好呀',
        isShow: true
    },
    components: {
        cpn: {
            template: '#cpn',
            data() {
                return {
                    isShow: false
                }
            }
        }
    }
})
```

上面的代码中，可以发现有两个 `isShow` 属性，一个是在 Vue 实例中，一个是在子组件中。那么我们在 html 代码中，id 为 app 的 div 使用子组件，并且给了一个 `v-show` 的指令，它将会去 Vue 实例中进行查找该属性，而不会去子组件中，因为该组件是在 Vue 实例中定义的，它有一个自己的作用域，所以它只会在自己作用域范围内进行属性的查找。

而 template 中，我们可以看到，给 button 的一个 `v-show` 指令，它是一个组件模板，所以这里的 `isShow` 属性，它会在组件中进行属性的搜索。

可以通过官方给出的一句话来理解：**父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。**





## 模块化开发

### 为什么要有模块化

在我们真实的开发中，我们并不是一个人开发一个项目的，而是多个人合作一起完成一个项目。而我们将一个 js 文件标记为一个模块，在整个项目中会有非常非常多个模块，而这些模块最终都要引入我们的主文件中。那么由此就会产生一些问题，**全局变量的问题**，比如 A 开发了一个模块，其中使用了 name 和 flag 属性，B 开发了一个模块，他也使用了 flag 属性。在 A 开发的模块中，他将 flag 定义为了 'abc'，他又在其他模块中引入了这个 flag 属性，并且判断 flag 为 'abc' 这个值的话，进行下一步操作；而 B 使用了同名的 flag 属性，并且在 B 的模块中赋了其他值。在最后文件合并时，A 的另一个模块中的内容无法显示了，这就产生了 bug。

在最初，ES5 的语法中，我们通常会使用闭包的方式来解决这个问题。由于函数是有作用域的，我们将我们所有的代码写到一个匿名函数中，并且让他自调用，这样的话，所定义的一些变量和函数就与其他模块隔离开了。但是这样也引发了一个问题：这个模块封闭了以后，我们其他模块如果需要这里的内容，怎么在另一个模块中引入我们需要的这个内容。接下来我们通过 return 解决了这个问题。

```js
var abc = (function() {
    var obj = {}
    var name = "swk"
    var flag = "aaa"
    obj.name = name
    obj.flag = flag
    return obj
})()
```

我们通过这样的方式来向外导出一些属性或方法。但是使用这样的方法还是比较麻烦的，并且不好维护或出现其他问题。

ES6 给我们提供了模块的导入和导出。

在 ES6 中新增了两个关键字：**export（导出） / import（导入）**。





### ES6 的模块化使用

在导入 js 文件到 html 页面中时，使用 script 标签，需要加上 type="module" 以标记这是一个模块文件。



**export 基本使用**

export 指令用于导出变量，有两种写法：

```js
export let name = 'swk'
export let age = 18
export let height = 1.88
```

```js
let name = 'skw'
let age = 18
let height = 1.88
export { name, age, height }
```



也可以导出函数/类

```js
export function fn1(num1, num2){
    return num1 + num2
}
```

```js
export class Person{
    run() {
        console.log('运行中...')
    }
}
```





**import 基本使用**

import 指令用于导入，有两种写法：

```js
import flag from './aaa.js'
import age from './bbb.js'
```

```js
import {name, age, height} from './abc.js'
```

第二种为增强写法，直接导入对象并且对应。







**export default**

我们上面的导出/导入，都需要相同的名字。比如我导出的变量名为 address，那么我导入也需要使用 address 这个名字，那么就会很不方便，export default 就帮助我们导出的东西可以不命名，由导入者自己命名。

导出：

```js
// aaa.js
export default address
```

导入：

```js
// bbb.js
import abc from './aaa.js'
```

通过 `aaa.js` 我们通过 default 导出 address，那么我们在 `bbb.js` 中导入时，就不需要在 `abc` 旁边加大括号（不需要 `{abc}` 这种形式），而 `abc` 得到的就是我们 `aaa.js` 导出的东西。

不过 export default **只能有一个！**否则在其他文件导入的时候，无法分清导入哪个。

```js
// 导出的这个函数不用函数名
export default function() {
    console.log('你好啊')
}
```

```js
// 导入 - 导入的是导出的那个文件默认导出的东西
import hello from './hello.js'
```

全部导入：`import * as abc from './aaa.js'` ，使用通配符 `*`。















































































