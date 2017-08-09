## 配置{#Configuration}

nwb的默认设置可以让您无需任何配置即可开发，测试和构建可生产就绪的应用程序和npm-ready组件。

如果您需要调整默认设置以满足项目需求，或者您想要使用Babel，Karma和Webpack生态系统提供的其他功能，则可以提供配置文件。

> 您还可以通过[安装插件](https://github.com/insin/nwb/blob/master/docs/Plugins.md#plugins)模块来添加新功能。

### 配置文件{#Configuration-File}

默认情况下，nwb将在当前工作目录中查找一个`nwb.config.js`文件进行配置。

您还可以使用`--config`选项指定配置文件：

```
nwb --config ./config/nwb.js
```

此文件也应导出一个配置对象...

```js
module.exports = {
  // ...
}
```

...或一个在调用时返回一个配置对象的函数：

```js
module.exports = function(args) {
  return {
    // ...
  }
}
```

如果一个函数被导出，它将被传递一个具有以下属性的对象：

- `args`：一个传递给`nwb`命令的参数的解析版本
- `command`：当前正在执行的命令的名称。 `'build'`或`'test'`
- `webpack`：`webpack`模块的nwb版本，让您访问webpack提供的其他插件。

#### 配置文件示例{#Example-Configuration-Files}

- [react-hn的`nwb.config.js`](https://github.com/insin/react-hn/blob/concat/nwb.config.js)是一个简单的配置文件，对Babel和Webpack配置进行了微调。
- [React Yelp Clone的nwb.config.js](https://github.com/insin/react-yelp-clone/blob/nwb/nwb.config.js)配置了Babel，Karma和Webpack，以便将nwb放入到已有应用程序中来处理它的开发工具，从而减少需要管理的devDependencies和配置的数量。

### 通过参数配置{#Configuration-Via-Arguments}

下面介绍的`babel`，`webpack`，`devServer`，`karma`和`npm`属性的配置也可以通过使用虚线路径的参数来提供，而不是调整您的`nwb.config.js`文件进行单次运行，或者不需要为[快速开发命令](https://github.com/insin/nwb/blob/master/docs/guides/QuickDevelopment.md#quick-development-with-nwb)添加配置文件：

```sh
nwb build-react-app --babel.stage=2 --webpack.hoisting
```

> **注意：**如果您有一个配置文件，这些参数将作为覆盖。
>
> nwb使用[minimist](https://github.com/substack/minimist)来进行参数解析，所以如果你想使用这个功能，这里是速查表：
>
> - `--config.example` is `true`
> - `--no-config.example` is `false`
> - `--config.example=value` is `'value'`
> - `--config.example=one --config.example=two` is `['one', 'two']`

### 配置对象{#Configuration-Object}

配置对象可以包括以下属性：

- nwb配置
  - [`type`](#type-string-required-for-generic-build-commands)
  - [`polyfill`](#polyfill-boolean) - 控制自动polyfilling
- [Babel配置](#babel-configuration)
  - [`babel`](#babel-object)
  - [`babel.cherryPick`](#cherrypick-string--arraystring) - 启用cherry-picking，解构`import`声明
  - [`babel.loose`](#loose-boolean) - 启用Babel松散模式
  - [`babel.plugins`](#plugins-string--array) - 使用额外的Babel插件
  - [`babel.presets`](#presets-string--array) - 使用额外的Babel预置
  - [`babel.removePropTypes`](#removeproptypes-object--false) - 禁用或配置在生产版本中删除React组件“propTypes”
  - [`babel.reactConstantElements`](#reactconstantelements-false) - 禁用在生产构建中使用React常量元素提升
  - [`babel.runtime`](#runtime-string--boolean) - 启用具有不同配置的`transform-runtime`插件
  - [`babel.stage`](#stage-number--false) - 控制哪些实验和即将推出的JavaScript功能可以使用
- [Webpack配置](#webpack-configuration)
  - [`webpack`](#webpack-object)
  - [`webpack.aliases`](#aliases-object) - 重写某些导入路径
  - [`webpack.autoprefixer`](#autoprefixer-string--object) - Autoprefixer的选项
  - [`webpack.compat`](#compat-object) - 启用Webpack兼容性调整常用模块
  - [`webpack.debug`](#debug-boolean) - 创建一个更可调试的生产构建
  - [`webpack.define`](#define-object) - `DefinePlugin`的选项，用于用值替换某些表达式
  - [`webpack.extractText`](#extracttext-object--boolean) - `ExtractTextPlugin`的选项
  - [`webpack.hoisting`](#hoisting-boolean) - 使用Webpack 3的`ModuleConcatenationPlugin`启用部分作用域提升
  - [`webpack.html`](#html-object) - `HtmlPlugin`的选项
  - [`webpack.install`](#install-object) - `NpmInstallPlugin`的选项
  - [`webpack.rules`](#rules-object) - 调整生成的Webpack规则及其加载器的配置
    - [默认规则](#default-rules)
    - [定制loader](#customising-loaders)
    - [禁用默认规则](#disabling-default-rules)
  - [`webpack.publicPath`](#publicpath-string) - 静态资源的路径
  - [`webpack.styles`](#styles-object--false--old) - 自定义创建样式表的Webpack规则
  - [`webpack.uglify`](#uglify-object--false) - 配置使用Webpack的`UglifyJsPlugin`
  - [`webpack.extra`](#extra-object) - 用于Webpack额外的配置，它将被合并到生成的配置中
  - [`webpack.config`](#config-function) - 用于手动编辑生成的Webpack配置
- [Dev Server配置](#dev-server-configuration)
  - [`devServer`](#devserver-object) - 配置Webpack Dev Server
- [Karma配置](#karma-configuration)
  - [`karma`](#karma-object)
  - [`karma.browsers`](#browsers-arraystring--plugin) - 浏览器测试运行
  - [`karma.excludeFromCoverage`](#excludefromcoverage-string--arraystring) - 应该从代码覆盖率报告中排除的路径
  - [`karma.frameworks`](#frameworks-arraystring--plugin) - 测试框架
  - [`karma.plugins`](#plugins-arrayplugin) - 外的Karma插件
  - [`karma.reporters`](#reporters-arraystring--plugin) - 测试结果报告
  - [`karma.testContext`](#testcontext-string) - 指向您的测试的Webpack上下文模块
  - [`karma.testFiles`](#testfiles-string--arraystring) - 测试文件的模式
  - [`karma.extra`](#extra-object-1) - 用于额外的Karma配置，将被合并到生成的配置中
- [npm 构建配置](#npm-build-configuration)
  - [`npm`](#npm-object)
  - [`npm.cjs`](#esmodules-boolean) - 切换创建一个CommonJS构建
  - [`npm.esModules`](#esmodules-boolean) - 切换创建ES模块构建
  - UMD build
    - [`npm.umd`](#umd-string--object) - 启用导出全局变量的UMD构建
      - [`umd.global`](#global-string) - 由UMD构建导出的全局变量名
      - [`umd.externals`](#externals-object) - 在UMD构建中通过全局变量使用依赖关系
    - [`package.json` 字段](#packagejson-umd-banner-configuration)


#### `type`: `String` (通用构建命令需要指定此字段){#type-string-required-for-generic-build-commands}

当使用通用构建命令（如`build`）时，nwb使用此字段来确定它正在使用的项目类型。 如果您正在使用具有您正在使用的项目类型的名称的命令，则不需要进行配置。

如果配置，它必须是以下之一：

- `'inferno-app'`
- `'preact-app'`
- `'react-app'`
- `'react-component'`
- `'web-app'`
- `'web-module'`

#### `polyfill`: `Boolean`{#polyfill-boolean}

对于应用程序，默认情况下，nwb将为`Promise`，`fetch`和`Object.assign`提供Polyfills。

要禁用此功能，请将`polyfill'设置为`false`：

```js
module.exports = {
  polyfill: false
}
```

### Babel Configuration

#### `babel`: `Object`

[Babel](https://babeljs.io/) configuration can be provided in a `babel` object, using the following properties.

> For Webpack builds, any Babel config provided will be used to configure `babel-loader` - you can also provide additional configuration in [`webpack.rules`](#rules-object) if necessary.

##### `cherryPick`: `String | Array<String>`

Module names to apply `import` cherry-picking to.

> This feature only works if you're using `import` syntax.

If you import a module with destructuring, the entire module will normally be included in your build, even though you're only using specific pieces:

```js
import {This, That, TheOther} from 'some-module'
```

The usual workaround for this is to individually import submodules, which is tedious and bloats import sections in your code:

```js
import This from 'some-module/lib/This'
import That from 'some-module/lib/That'
import TheOther from 'some-module/lib/TheOther'
```

If you use `cherryPick` config, you can keep writing code like the first example, but transpile to the same code as the second, by specifying the module name(s) to apply a cherry-picking transform to:

```js
module.exports = {
  babel: {
    cherryPick: 'some-module'
  }
}
```

This is implemented using [babel-plugin-lodash](https://github.com/lodash/babel-plugin-lodash) - please check its issues for compatibility problems with modules you're using `cherryPick` with and report any new ones you find.

##### `loose`: `Boolean`

Some Babel plugins have a [loose mode](http://www.2ality.com/2015/12/babel6-loose-mode.html) in which they output simpler, potentially faster code rather than following the semantics of the ES6 spec closely.

**nwb enables loose mode by default**.

If you want to disable loose mode (e.g. to check your code works in the stricter normal mode for forward-compatibility purposes), set it to `false`.

e.g. to disable loose mode only when running tests:

```js
module.exports = {
  babel: {
    loose: process.env.NODE_ENV === 'test'
  }
}
```

