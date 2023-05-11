# webpack



## 认识 webpack

### 什么是 webpack？

webpack 是一个现代的 JavaScript 应用的静态**模块打包**工具。

理解：它可以帮助我们使一些模块打包成我们在服务器上能运行的文件，例如我们在开发中，会使用到许多的插件及框架等，它们的代码如果直接放在浏览器上运行的话，可能是无法运行的，这时候就需要打包工具，帮我们把这些浏览器无法识别的代码，转化为浏览器可识别的代码，并且如果我们在 ES6 中使用的一些语法，想要兼容一些 ES5 的浏览器也可以使用它。



### 前端模块化

在 Vue 学习笔记中有解释为什么需要前端模块化。

前端模块化目前使用的方案有：AMD、CMD、CommonJS、ES6。但是目前我们能用的规范只有 ES6 规范，因为只有该规范浏览器才支持。

但是这些方案在 webpack 中都可以使用，如果我们用到了一些其他的规范，webpack 都会自动帮助我们解析转化（比如用到了 AMD 规范，那么 webpack 会自动帮我们转化为 ES6 的规范，会帮我们自动转化浏览器能识别的代码）。

webpack 其中一个核心就是让我们能进行模块化开发，并且会帮助我们处理模块间的依赖关系，而且不仅仅是 JavaScript 文件，我们的 CSS、图片、json 文件等在 webpack 中都可以被当做模块来使用。



### 个人理解 webpack

webpack 是一个模块打包工具。我们在开发中，难免会使用到插件或其他的一些组件模块，那么我们如果手动将这些模块进行连接，那么会非常复杂，而 webpack 就可以帮我们处理他们之间的依赖关系（就是什么模块需要什么模块存在才可以运行）。

并且，webpack 可以帮助我们使代码与浏览器之间进行兼容。我们在开发时，由于原生 js 代码对于我们的开发效率、容错率实在是太低了，所以我们会在开发中引入其他的框架等模块的操作，而引入的这些东西其实是对原生 js 的一些封装，但是它们的语法不一定与原生 js 完全一样，所以需要解析才能够使用。而 webkpack 帮我们把这些开发中使用的代码，自动转化为浏览器能够识别的代码，我们所写的代码，通过 webpack 打包后，直接部署到服务器上即可使用，大大提高了我们开发的效率。





### 和 grunt/gulp 的对比

**grunt/gulp 的核心是 Task**

我们使用这两款工具时，可以通过配置一系列的 task，并且定义 task 要处理的事务（例如ES6、ts 转化、图片压缩、sass 转成 css）。之后我们让 grunt/gulp 依次执行这些 task，它们就会自动按照流程来执行完成我们给定的任务，所以 grunt/gulp 也被称为前端自动化任务管理工具。其实也就是我们给它定义任务，它会自动按照我们定义的任务去执行。

如果你的工程模块依赖非常简单，甚至是没有用到模块化的概念，只需要进行简单的代码合并、压缩，就使用 grunt/gulp 即可，但是如果整个项目使用模块化管理，而且相互依赖非常强，我们就可以使用更加强大的 webpack 了。

**grunt/gulp 与 webpack 的不同**

grunt/gulp 更加强调的是前端流程的自动化，也就是前端对一些事物的自动处理。

webpack 更加强调模块化开发管理，而文件压缩合并、预处理等功能只是它附带的功能。







## webpack 的安装

webpack 为了可以正常运行，必须依赖 node.js 环境。

**npm 工具（node packages manager），它是包管理工具，帮助我们管理 node 上面各种包。为了让我们环境能够运行起来，我们需要各种包，而各种包我们就可以通过 npm 来进行管理。**

安装流程：

- 安装 node.js

  可以通过命令行 `node -v` 查看自己的 node 版本。

- 全局安装 webpack（这里指定使用版本号 3.6.0，因为 vue cli2 依赖该版本）

  在命令行中输入 `npm install webpack@3.6.0 -g`

- 局部安装 webpack（后续才需要）

  `--save-dev` 是开发时依赖，项目打包后不需要继续使用的。

  ```shell
  cd 对应目录
  npm install webpack@3.6.0 --save-dev
  ```

  





## webpack 的起步

我们在开发项目的文件夹中（项目这个大的文件夹），一般都会包含两个文件夹：src（源代码）和 dist（打包后的东西 dist -> distribution）。

`dist` 文件夹就是我们打包的最终的东西，我们直接将这个文件夹发布到服务器上即可。

我们在 src 文件夹中，写我们的模块代码，webpack 会自动根据我们导入导出的依赖，将文件打包起来。例如我们在 src 文件夹中写我们的代码：

```js
// main.js
// 导入对象，定义两个变量接收
const {add, mul} = require('./mathUtils.js')
console.log(add(20, 30))
console.log(mul(20, 30))
```

```js
// mathUtils.js
function add(num1, num2) {		// 定义add函数
    return num1 + num2
}

function mul(num1, num2) {		// 定义mul函数
    return num1 * num2
}

// 导出对象，对象中包含add和mul
module.exports = {add, mul}
```

我们编写了两个模块，然后我们使用 webpack 对其进行打包，在 shell 中输入：

`webpack ./src/main.js ./dist/bundle.js` 即可。

参数 `webpack` 是执行 webpack 打包工具，`./src/main.js` 是源模块，`./dist/bundle.js` 是目标模块。webpack 会自动对我们源模块中的依赖关系进行解析，自动对我们代码中所包含的模块进行打包处理，最后输出到目标模块，而我们在网页中直接使用目标模块即可。

我们只需要给我们的网页入口模块即可。







## webpack 的配置

我们在上面使用 `webpack ./src/main.js ./dist/bundle.js` 这条命令对我们的模块进行打包，但是我们每次都要这样操作输入，很麻烦（输入的命令很长），我们怎么简化它的命令，只需要一个命令就可以自动帮我们打包呢？这就需要对 webpack 进行配置了。

我们需要在我们的工程项目中，创建一个 `webpack.config.js` 文件（这个文件名暂时不能改，否则无法使用）

我们在配置文件（`webpack.config.js` 文件）中进行配置，通过导出模块的方式进行配置：

```js
const path = require('path')

module.export = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    }
}
```

在上面的代码中，我们对 webpack 进行了配置，但是这里配置需要说明以下几点：

1. 首先是要对 npm 进行初始化，使我们 npm 能够找到需要的一些依赖。

2. 我们配置文件导出的是一个对象，`entry` 表示入口，`output` 表示输出，输出的配置项是一个对象，分别是 `path`（路径），`filename`（文件名）。

   这里的重点是 path，path 不能是一个相对路径，需要是一个绝对路径，所以我们使用 node 中的一个全局变量来动态获取当前文件所在的绝对路径，也就是 `path.resolve()`，第一个参数填写 `__dirname` ，它能够帮我们获取到绝对路径，第二个参数则为文件夹的名字。

3. `webpack.config.js` 这个文件需要在当前项目文件夹或上级文件夹中。

   因为我们在某个文件夹中运行 webpack，它将会在我们运行的文件夹中进行查找 webpack 配置文件，该文件夹中没有则一直向上一级寻找，一直找到根目录为止。

通过以上配置后，我们直接在 shell 中输入 webpack，它将会自动帮助我们打包。

虽然通过以上配置后，我们可以直接输入 webpack 进行打包，但是最终我们项目运行使用该命令不好区分和管理，并且我们的文件名 `webpack.config.js` 改为了其他的，例如 `production.config.js` ，那么我们运行 webpack 时，还需要给它执行我们的配置文件名是哪一个，所以我们还要对项目中的一些命令进行配置，使我们可以输入一些更易懂的命令就可以完成相关的操作，那么我们需要对命令进行映射。

打开 `package.json` 文件，我们可以看到一个 json 对象，对象中有一个 `script` 属性，其中有一个 `test` 属性。

我们简单概述一下作用。`script` 中的 `test` 可以使我们在 shell 中输入 `npm run test` 时，自动执行 `test` 所对应的 value 值的命令。例如：

```shell
...
"script":{
	"test": "node app.js",
	"build": "webpack",
}
...
```

通过上面代码中的配置，我们可以在 shell 中输入 `npm run test` 时，相当于它会自动在 shell 中输入 `node app.js` 这条命令，而输入 `npm run build` 时，相当于它自动输入了 `webpack` 这条命令，由于我们上面对 webpack 进行了配置，输入 webpack 就会自动打包，所以我们在这里，输入 `npm run build` 则会自动使用 webpack 将我们的模块进行打包。

而在这里定义脚本，有一个好处就是它会优先执行我们本地中的依赖，而不是全局的依赖（如果我们直接在终端输入 `webpack` 时，它会在我们的全局中进行寻找）。

**这里解释一下本地依赖和全局依赖以及开发时依赖和生产时依赖**

本地依赖是在我们这个项目中的依赖，全局依赖是我们安装在全局中的依赖（系统指定文件夹中）。

我们在这里将使用开发时依赖，也就是这些依赖文件在我们开发的时候才会用到，而项目上线后，不需要这些依赖文件。而生产时依赖则是上线后，所要依赖的一些模块。

我们通过 `npm install webpack@3.6.0 --save-dev` 安装 webpack 并且指定它是开发时依赖（`--save-dev` 命令）。安装完了后，可以发现我们的 `package.json` 中多了一个 `devDependencies` 属性，而这里面指定的就是开发时依赖。

使用本地依赖进行打包我们需要打开目录，一层一层找到 webpack 文件，再对其进行打包操作（在 `package.json` 中的 `script` 中定义了我们就不需要这么麻烦了）。









## loader 的使用

loader 是 webpack 中一个非常核心的概念。

在我们之前的实例中，我们主要是用 webpack 来处理我们写的 js 代码，并且 webpack 会自动处理 js 之间相关的依赖，但是在开发中我们不仅仅有基本的 js 代码处理，我们也需要加载 css、图片，也包括一些高级的，将 ES6 转成 ES5 代码，将 TypeScript 转成 ES5 代码，将 css、less 转成 css，将 .jsx、.vue 文件转成 js 文件等等。那么 webpack 本身的能力来说，这些转化是不支持的，我们需要给 webpack 进行扩展，而扩展的这个东西就叫做 loader。

**loader 使用过程**

1. 通过 npm 安装需要使用的 loader（大部分 loader 我们都可以在 webpack 的官网中找到，并且学习对应的用法）。
2. 在 `webpack.config.js` 中的 modules 关键字下进行配置。



**模块引入说明**

我们的整个项目工程，由于现在的 webpack 只能对 js 文件进行打包，我们需要将我们创建的 css 文件进行打包的话就做不到，我们不需要将 css 文件一个一个引入到主文件（`main.js`）中，我们只需要将所需要的 css 文件，导入到主文件中，我们再通过 webpack 对主文件进行打包，那么它会自动搜索文件依赖，自动处理 css 文件，将 js 和 css（或其他文件）直接打包到一块。

我们在主文件中添加 css 依赖，把 css 文件也当作一个模块，直接 require css 文件即可：

```js
require('./css/normal.css')
```

而且这里不需要定义变量来接收，只需要 require（需要在已经安装好 loader 的情况下）。

css-loader 只负责加载 css 文件，但是不负责解析，需要使用 style-loader，同时使用才会加载并解析。



如果我们使用 less 文件的话，我们则需要下载对应的 less 以及对应的 loader，在 webpack 官网上都有解释。



**ES6 语法转化为 ES5**

我们在使用 webpack 进行打包时，实际上它内部有许多 ES6 的语法，可能无法被一些浏览器所识别，所以我们需要使用 webpack 对其进行处理，转化成 ES5 的语法使大多数浏览器都能够识别。

我们需要使用 babel。在 webpack 中，我们直接使用 babel 对应的 loader 就可以了。

`npm install --save-dev babel-loader@7 babel-core babel-preset-es2015`

配置 `webpack.config.js` 文件后重新打包，查看打包后的文件，可以发现其中的内容变成了 ES5 的语法了。

上面的命令中，我们还安装了 `babel-core`，因为在 ES6 转化为 ES5 时，它必须需要这个 `babel-core` 才可以



 





## webpack 中配置 Vue

在真实开发中，我们通常不会使用 CDN 或静态的方式引入 vue，而是将 vue 看作一个模块，通过 npm 进行安装。

我们使用 `npm install vue --save` 可以安装 vue，在我们的模块中，通过 import 对 vue 进行引入：`import Vue from 'vue'`。

但是我们引入的 vue，它默认是 runtime-only 的形式运行的。

runtime-only 代码中，不可以有任何的 template 模板，但是在 runtime-compiler 代码中，我们可以有 template，因为有 compiler 可以用于编译 template。

我们需要对 vue 进行配置，对 vue 默认使用的版本进行配置，我们在 webpack 中增加一行 `resolve` 字段。

```js
const path = require('path')

module.exports = {
    entry: './src/main.js,
    output: {filename: 'bundle.js'...},
    module: {...},
    resolve: {
        alias: {
            "vue$": 'vue/dist/vue.esm.js'
        }
    }
}
```

通过上面的配置（`resolve` 的配置），我们 import vue 的时候，webpack 解析代码时就会先去我们这个配置的这个路径中去找 vue，我们就是通过该配置来指定使用哪个版本的 vue。





**index.html**

我们通过上面的配置，将文件组织好了以后，在文件夹外部只需要有一个 `index.html` 入口，其他的文件都放到 src 文件夹中。

在 `index.html` 中，我们只需要保留 `<div id="app"></div>` 这个代码在 body 中即可，其他不需要修改。





**el 和 template 的区别**

我们定义好了 `index.html` 文件后，不要往内部写 html 代码，我们所写的 html 代码都放在 `main.js`（也就是主文件模块中）的 template 中即可。

也就是在 Vue 实例中，我们既有 el 又有 template，而 template 会替换掉我们 el 挂载的 DOM 对象。





**封装处理 .vue 文件**

在我们前文中，提到了组件化、模块化开发的思想，那么我们需要对代码进行完全的解耦，将我们的模板、样式、脚本等文件，都抽取出来使其完全分离。

我们不需要在根目录中的 `index.html` 文件中编写代码了，只需要给它保留一个最初的模板，而我们的所有代码都是通过其他模块，一层一层依赖。`index.html` 只需要保留一个挂载的代码，使其能够与主模块 `App.vue` 进行通信即可，而我们的其他模块只需要与这个主模块进行通信，形成一个组件树，只有一个根模块（主模块），其他的页面或模块再与主模块进行连接即可。

那么我们就将创建一个 `.vue` 为后缀的文件，我们可以将 html 代码写在该文件的 template 标签中，将 js 代码（vue 的代码）写在 script 标签中，而 css 代码写在 style 标签中。通过对这些代码的编写，我们的模块就编写出来了，而在模块中需要引入其他模块的话，直接在该模块中添加 `components` 即可，注册其他模块。

在通过这些模块与模块之间的连接后，我们需要对整体的一个项目进行编译打包，就需要使用 webpack 了。我们使用 webpack 对这些进行打包的话，其实上是一个预处理的过程，也就是将模板中的内容替换为我们需要的目标代码，再将目标代码打包成为最终的项目文件。而我们就需要为 webpack 安装对应的 loader 来完成这些预处理的工作。

安装 vue-loader 和 vue-template-compiler，通过命令行输入 `npm install vue-loader vue-template-compiler --save-dev`。

安装完成后，我们需要修改 webpack 中的配置，使这些安装的 loader 能够解析处理 `.vue` 文件的代码。在 module 的数组中添加一项：

```js
{
    test: /\.vue$/,
    use: ['vue-loader']
}
```











## plugin 的使用

plugin 是插件的意思，通常是用于对某个现有的架构进行扩展。

**添加版权的Plugin**

我们可以使用一个最简单的插件，为我们所有通过 webpack 打包的文件添加版权声明，该插件名字叫 BannerPlugin，属于 webpack 自带的插件，通过以下方式来修改 `webpack.config.js` 的文件：

```js
const path = require('path')
const webpack = require('webpack')

module.exports = {
    ...
    Plugins: [
        new webpack.BannerPlugin('最终版权归xxx所有')
    ]
}
```

将配置修改完成后，我们重新打包程序可以看到打包后的文件头部，有 `最终版权归xxx所有` 的信息。



**打包 html 的 plugin**

由于我们前面打包是将所有的模块进行打包的，而没有将我们的根页面进行打包（`index.html`），最终我们是要直接将目标的文件夹直接发布出去，而我们的根页面没有再目标文件夹中的，我们就需要使用一个插件来对根页面进行处理，使最终的项目完整。所以，我们需要将 `index.html` 打包到 dist 文件夹中，需要使用 HtmlWebpackPlugin 插件。

HtmlWebpackPlugin 插件可以为我们自动生成一个 `index.html` 文件（可以通过指定模板来生成），并且会将打包的 js 文件，自动通过 script 标签插入到 body 中。

安装 HtmlWebpackPlugin 插件，我们在命令行中输入 `npm install html-webpack-plugin --save-dev`，使用插件，修改的 `webpack.config.js` 文件中的 plugins 部分的内容如下：

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    ...
    Plugins: [
        new webpack.BannerPlugin('最终版权归xxx所有'),
        new htmlWebpackPlugin({
        	template: 'index.html'
        }),
    ]
}
```

这里的 template 表示根据什么模板来生成 `index.html`，另外，我们需要删除之前在 output 中添加的 publicPath 属性，否则插入的 script 标签中的 src 可能会有问题。



**js 压缩的 plugin**

在正式发布项目之前，我们必然要对 js 等文件进行压缩处理，使其更节省空间。我们需要使用到一个插件来对其进行压缩，输入 `npm install uglifyjs-webpack-plugin@1.1.1 --save-dev` 来进行安装，并且需要修改 `webpack.config.js` 文件以使用插件：

```js
const path = require('path')
const webpack = require('webpack')
const uglifyJsPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
    ...
    plugins: [
        new webpack.BannerPlugin('最终版权归xxx所有')
        new uglifyJsPlugin()
    ]
}
```











## 搭建本地服务器

我们通过 webpack 可以对代码进行打包，每次更改了代码都要对代码进行打包编译一次，这会非常的麻烦，所以我们需要一个东西能够自动帮我们打包（或者刷新修改后的代码），这就使用到了本地服务器。

webpack 提供了一个可选的本地开发服务器，这个本地服务器基于 node.js 搭建，内部使用 express 框架，可以使我们修改代码后，在浏览器自动刷新修改后的结果。它会自动对我们的项目文件夹进行监视是否改变，并且将改变之后的代码存储到内存中，我们在进行调试时，会看到看到改变后的代码，而最后编译才会将内存中的代码保存到硬盘中，所以我们完成整个项目保存时，它才会自动将所有内容保存到硬盘中。

它是一个单独的模块，我们需要在 webpack 使用之前安装它：

`npm install --save-dev webpack-dev-server@2.9.1`

安装了以后，我们同样需要对它进行配置，告诉它需要监视哪个文件夹或其他操作。

我们安装好了以后，在 `webpack.config.js` 文件中配置：

```js
module.exports = {
    ...
    devServer: {
        contentBase: './dist',
        inline: true
    }
}
```

`contentBase` 表示监听哪个文件夹，`inline` 表示是否实时监听。

通过以上的配置，我们可以将本地服务器跑起来了。

在命令行中输入 `webpack-dev-server`，但是通过该命令，它会报错，因为在命令行中输入的内容，它会优先在全局中进行查找，所以我们在这里无法运行命令，我们可以通过

`./node_modules/\.bin/webpack-dev-server` 即可，或通过配置 `package.json` 中的 script，添加一条 `"dev": "webpack-dev-server"` ，在命令行中输入 `npm run dev` 即可将服务器运行起来。

服务器运行起来后，我们在项目中将代码进行了修改，该服务器（默认是在 8080 端口）会自动对我们修改后的内容进行渲染。

我们在启动服务器后，可以通过配置 `package.json` 文件，让它启动后自动帮我们打开浏览器，此时在 script 中的代码里加一个 `--open` 即可： `"dev": "webpack-dev-server --open"`，这样，在我们启动服务器后，它会自动帮我们打开浏览器。







## webpack 配置文件的分离

我们在使用 webpack 时，进行了很多配置，而有些配置是只有我们在开发阶段才会用到的，项目如果上线了，我们就需要将这些配置删除，所以我们需要对 webpack 的一些配置进行分离。也就是开发时依赖的一个配置文件，发布时依赖另一个配置文件。

我们需要安装 webpack-merge 来对 webpack 的配置文件进行分离。

我们在项目中新建一个 `build` 文件夹，将所有公共的配置集成一个文件 `base.config.js`，再将我们开发时依赖和发布时依赖分别抽离到各自的文件夹中，开发时依赖：`dev.config.js` 和 发布时依赖：`prod.config.js`。

然后引入我们安装的 webpack-merge 和我们的公共配置文件，然后导出：

```js
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig, {
    devServer: {
        contentBase: './dist',
        inline: true,
    }
})
```

这是开发时依赖，而发布时依赖也是差不多，只是在导出的对象中，配置我们发布时需要的一些配置。

接下来我们在 script 脚本中，对 webpack 的配置进行指定，否则它会默认去找 `webpack.config.js` 这个文件中的配置项，实际上我们将这个文件分离为了3个文件，我们在对脚本进行修改后，它会自动对我们指定的配置文件进行合并运行。

```json
{
    ...
    "script": {
        ...
        "build": "webpack --config ./build/prod.config.js",
        "dev": "webpack-dev-server --open --config ./build/dev.config.js"
    },
    ...
}
```

通过以上配置后，我们直接输入 `npm run build` 它会自动进行打包。但是我们最后打包的文件，其实是有问题的，它会将我们打包后的文件放入 build 文件夹中，因为在前面我们进行了配置，使打包后的文件直接在当前文件夹直接拼接一个 dist 路径，所以我们需要在 `base.config.js` 文件中更改配置。

```js
...

module.exports = {
    output: {
        path: path.resolve(__dirname, '../dist'),
        filename: 'bundle.js',
        ...
    },
    ...
}
```

通过以上配置即可。

















