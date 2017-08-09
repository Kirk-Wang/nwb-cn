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
  - [`webpack.extra`](#extra-object) - an escape hatch for extra Webpack config, which will be merged into the generated config
  - [`webpack.config`](#config-function) - an escape hatch for manually editing the generated Webpack config
- [Dev Server Configuration](#dev-server-configuration)
  - [`devServer`](#devserver-object) - configure Webpack Dev Server
- [Karma Configuration](#karma-configuration)
  - [`karma`](#karma-object)
  - [`karma.browsers`](#browsers-arraystring--plugin) - browsers tests are run in
  - [`karma.excludeFromCoverage`](#excludefromcoverage-string--arraystring) - globs for paths which should be excluded from code coverage reporting
  - [`karma.frameworks`](#frameworks-arraystring--plugin) - testing framework
  - [`karma.plugins`](#plugins-arrayplugin) - additional Karma plugins
  - [`karma.reporters`](#reporters-arraystring--plugin) - test results reporter
  - [`karma.testContext`](#testcontext-string) - point to a Webpack context module for your tests
  - [`karma.testFiles`](#testfiles-string--arraystring) - patterns for test files
  - [`karma.extra`](#extra-object-1) - an escape hatch for extra Karma config, which will be merged into the generated config
- [npm Build Configuration](#npm-build-configuration)
  - [`npm`](#npm-object)
  - [`npm.cjs`](#esmodules-boolean) - toggle creation of a CommonJS build
  - [`npm.esModules`](#esmodules-boolean) - toggle creation of an ES modules build
  - UMD build
    - [`npm.umd`](#umd-string--object) - enable a UMD build which exports a global variable
      - [`umd.global`](#global-string) - global variable name exported by UMD build
      - [`umd.externals`](#externals-object) - dependencies to use via global variables in UMD build
    - [`package.json` fields](#packagejson-umd-banner-configuration)



