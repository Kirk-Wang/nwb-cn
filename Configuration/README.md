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

### Babel配置{#babel-configuration}

#### `babel`: `Object`{#babel-object}

可以使用以下属性在`babel`对象中提供[Babel](https://babeljs.io/)配置。

> 对于Webpack构建，任何提供的Babel配置将用于配置`babel-loader` - 如果需要，您还可以在[`webpack.rules`](#rules-object)中提供其他配置。

##### `cherryPick`: `String | Array<String>`{#cherrypick-string--arraystring}

应用`import`cherry-picking到模块名称。

> 此功能仅在您使用`import`语法时有效。

如果您导入具有解构的模块，则整个模块通常会包含在构建中，即使您只使用特定的部分：

```js
import {This, That, TheOther} from 'some-module'
```

通常的解决方法是单独导入子模块，这在您的代码中是繁琐的：

```js
import This from 'some-module/lib/This'
import That from 'some-module/lib/That'
import TheOther from 'some-module/lib/TheOther'
```

如果您使用`cherryPick`配置，您可以像第一个示例一样编写代码，但是可以通过指定要将cherry-picking变换应用于的模块名称来转换为与第二个相同的代码：

```js
module.exports = {
  babel: {
    cherryPick: 'some-module'
  }
}
```

这是使用[babel-plugin-lodash](https://github.com/lodash/babel-plugin-lodash)实现的 - 请检查其问题与您正在使用cherryPick的模块的兼容性问题，并报告您找到的任何新的模块。

##### `loose`: `Boolean`{#loose-boolean}

一些Babel插件具有[松散的模式](http://www.2ality.com/2015/12/babel6-loose-mode.html) ，其中输出更简单，可能更快的代码，而不是严格遵循ES6规范的语义。

**nwb默认启用松散模式**。

如果要禁用松散模式（例如，为了检查您的代码是否在更严格的正常模式下工作以实现向前兼容性），请将其设置为`false`。

例如：仅在运行测试时禁用松散模式：

```js
module.exports = {
  babel: {
    loose: process.env.NODE_ENV === 'test'
  }
}
```

##### `plugins`: `String` | `Array`{#plugins-string--array}

使用额外的Babel插件。

一个不需要配置对象的附加插件可以被指定为一个String，否则提供一个Array。

nwb命令在当前工作目录中运行，因此如果您需要配置其他Babel插件或预设，您可以在本地安装它们，传递名称并让Babel为您导入。

例如 安装和使用[babel-plugin-react-html-attrs](https://github.com/insin/babel-plugin-react-html-attrs#readme)插件：

```
npm install babel-plugin-react-html-attrs
```
```js
module.exports = {
  babel: {
    plugins: 'react-html-attrs'
  }
}
```

##### `presets`: `String` | `Array`

额外的Babel预设使用。

不需要配置对象的单个附加预设可以指定为String，否则提供一个Array。

##### `removePropTypes`: `Object | false`

由于React`propTypes`仅用于开发模式，所以nwb默认使用[react-remove-prop-types](https://github.com/oliviertassinari/babel-plugin-transform-react-remove-prop-types)转换将它们从React应用程序生产构建中删除。

设置为`false`以禁止使用此转换：

```js
module.exports = {
  babel: {
    removePropTypes: false,
  }
}
```

提供一个对象来配置[转换的选项](https://github.com/oliviertassinari/babel-plugin-transform-react-remove-prop-types#options)：

```js
module.exports = {
  babel: {
    removePropTypes: {
      // Remove imports of the 'prop-types' module as well
      // Only safe to enable if you're only using this module for propTypes
      removeImport: true
    },
  }
}
```

##### `reactConstantElements`: `false`{#reactconstantelements-false}

将其设置为`false`可禁用在React应用程序生产构建中使用[React常量元素提升转换](https://babeljs.io/docs/plugins/transform-react-constant-elements/)。

##### `runtime`: `String | Boolean`

默认情况下，Babel的[runtime transform](https://babeljs.io/docs/plugins/transform-runtime/)有3件事情：

1. 从`babel-runtime`导入helper模块，而不是在需要它们的每个模块中复制**helpers**。
2. 在您的代码中使用新的ES6内置（`Promise`）和静态方法（例如`Object.assign`）时，导入本地**polyfill**。
3. 导入在需要使用`async`/`await`所需的**regenerator** 运行时。

nwb的默认配置打开regenerator运行时导入，因此你可以使用`async`/`await`和generator。

要启用其他功能，您可以命名（`'helpers'`或`'polyfill'`）：

```js
module.exports = {
  babel: {
    runtime: 'helpers'
  }
}
```

要启用所有功能，请将`runtime`设置为`true`。

要禁用使用运行时转换，将`runtime`设置为`false`。

> **注意：**如果在React Component或Web Module项目中使用`async`/`await`或启用运行时转换的其他功能，则需要在您的package.json`peerDependencies`中添加`babel-runtime`，以确保其他人使用模块时可以从npm resolved。

##### `stage`: `Number | false`{#stage-number--false}

> nwb实现了自己相当于Babel 5的Babel 6的`stage`配置

控制哪些Babel预设将用于在代码中使用实验性，建议和即将到来的JavaScript功能，按照TC39过程中的阶段分组，以提出新的JavaScript功能：

| Stage | TC39 Category | Features |
| ----- | ------------- | -------- |
| [0](https://babeljs.io/docs/plugins/preset-stage-0) | Strawman, just an idea |`do {...}` expressions, `::` function bind operator |
| [1](https://babeljs.io/docs/plugins/preset-stage-1) | Proposal: this is worth working on | export extensions |
| [2](https://babeljs.io/docs/plugins/preset-stage-2) | Draft: initial spec | class properties, `@decorator` syntax (using the [Babel Legacy Decorator plugin](https://github.com/loganfsmyth/babel-plugin-transform-decorators-legacy)) - **enabled by default** |
| [3](https://babeljs.io/docs/plugins/preset-stage-3) | Candidate: complete spec and initial browser implementations | object rest/spread `...` syntax,  `async`/`await`, `**` exponentiation operator, trailing function commas |

例如，如果您想在应用中使用导出扩展程序，则应将`stage`设置为`1`：

```js
module.exports = {
  babel: {
    stage: 1
  }
}
```

默认情况下启用Stage 2 - 完全禁用stage预设，将`stage`设置为`false`：

```js
module.exports = {
  babel: {
    stage: false
  }
}
```

### Webpack配置{#webpack-configuration}

#### `webpack`: `Object`{#webpack-object}

可以使用以下属性在Webpack对象中提供[Webpack](https://webpack.js.org/)配置：

##### `aliases`: `Object`{#aliases-object}

配置[Webpack别名](https://webpack.js.org/configuration/resolve/#resolve-alias)，允许您控制模块resolution。 通常使用别名来使应用程序中嵌套目录中的某些模块更容易导入。

```js
module.exports = {
  webpack: {
    aliases: {
      // Enable use of 'img/file.png' paths in JavaScript and
      // "~images/file.png" paths in stylesheets to require an image from
      // src/images from anywhere in the the app.
      'img': path.resolve('src/images'),
      // Enable use of require('src/path/to/module.js') for top-down imports
      // from anywhere in the app, to promote writing location-independent
      // code by avoiding ../ directory traversal.
      'src': path.resolve('src')
    }
  }
}
```

您应该小心避免创建与Node.js内置或npm包名称冲突的别名，因为您将无法导入。

##### `autoprefixer`: `String | Object`{#autoprefixer-string--object}

为nwb的默认PostCSS configuration配置[Autoprefixer选项](https://github.com/postcss/autoprefixer#options)。

如果您只需要配置浏览器的范围前缀，添加/删除是基于（nwb的默认值是`>1%, last 4 versions, Firefox ESR, not ie < 9`），您可以使用字符串：

```js
module.exports = {
  webpack: {
    autoprefixer: '> 1%, last 2 versions, Firefox ESR, ios >= 8'
  }
}
```

如果您需要设置任何Autoprefixer的其他选项，请使用对象。

例如，如果您还想禁用删除配置的浏览器范围不需要的前缀：

```js
module.exports = {
  webpack: {
    autoprefixer: {
      remove: false,
    }
  }
}
```

您可以使用[browserl.ist](http://browserl.ist)服务检查您的Autoprefixer配置将使用哪些浏览器。

##### `compat`: `Object`{#compat-object}

某些库需要特定的配置才能很好地与Webpack一起使用 - 如果您使用`compat`对象在使用它时告诉它，nwb可以为您处理这些细节。

支持以下库：

###### `enzyme`: `Boolean`

设置为`true`为[Enzyme](http://airbnb.io/enzyme/)兼容性 - 这假定您使用的是最新版本的React（v15）。

###### `intl`: `Object`

如果您在Webpack构建中使用[intl](https://www.npmjs.com/package/intl)，则它支持的所有语言环境将默认导入，并且您的构建将比您期望的更大！

提供一个对象，一个`locales`数组指定要加载的区域设置的语言代码，如果只需要一个区域设置，则提供一个String。

###### `moment`: `Object`

如果您在Webpack构建中使用[Moment.js](http://momentjs.com/)，则它支持的所有语言环境将默认导入，您的构建将比您期望的更大！

提供一个对象，一个`locales`数组指定要加载的区域设置的语言代码，如果只需要一个区域设置，则提供一个String。

###### `react-intl`: `Object`

如果您在Webpack构建中使用[react-intl](https://github.com/yahoo/react-intl)，则默认情况下将导入所有支持的语言环境，并且您的构建将比您期望的更大！

提供一个对象，一个`locales`数组指定要加载的区域设置的语言代码，如果只需要一个区域设置，则提供一个String。

###### `sinon`: `Boolean`

对于[Sinon.js](http://sinonjs.org/)1.x兼容性设置为`true`。

---

示例配置显示使用多个`compat`设置：

```js
module.exports = {
  webpack: {
    compat: {
      enzyme: true,
      moment: {
        locales: ['de', 'en-gb', 'es', 'fr', 'it']
      },
      sinon: true
    }
  }
}
```

##### `debug`: `boolean`{#debug-boolean}

启用更可调试的生产构建：

- 通常从生产构建中删除的代码将被删除，但代码不被压缩。
- 模块ID将是文件系统名称，而不是生成的hash。

建议您通过配置参数来切换，以便您不要忘记稍后删除它：

```sh
npm run build -- --webpack.debug
```
```sh
nwb preact build app.js --webpack.debug
```

如果您在配置文件中设置`debug`，nwb将始终显示一个提示，提醒您稍后将其删除。

> **注意：**如果您通过[`webpack.uglify` config](#uglify-object--false)定制了`UglifyJsPlugin`设置，那么在调试版本中将只使用`webpack.uglify.compress`。

##### `define`: `Object`{#define-object}

默认情况下，nwb将使用Webpack的[`DefinePlugin`](https://webpack.js.org/plugins/define-plugin/)将所有出现的`process.env.NODE_ENV`替换为包含`NODE_ENV`的字符串当前值。

您可以配置一个`define`对象来添加你自己的常量值。

例如，从`package.json`中用包含app version的字符串替换所有出现的`__VERSION__`：

```js
module.exports = {
  webpack: {
    define: {
      __VERSION__: JSON.stringify(require('./package.json').version)
    }
  }
}
```

##### `extractText`: `Object | Boolean`{#extracttext-object--boolean}

配置[`ExtractWebpackPlugin选项`](https://github.com/webpack-contrib/extract-text-webpack-plugin#options)。

默认情况下，nwb使用[`ExtractTextWebpackPlugin`](https://github.com/webpack-contrib/extract-text-webpack-plugin#readme)在创建构建时将导入的样式表提取到`.css`文件中。

默认配置是从所有块中提取样式表（这也避免了构建中包含`style-loader`运行时），并在提取的文件名中使用内容哈希，相当于：

```js
module.exports = {
  webpack: {
    extractText: {
      allChunks: true,
      filename: process.env.NODE_ENV === 'production' ? `[name].[contenthash:8].css` : '[name].css'
    }
  }
}
```

设置为`false`以禁止在构建中提取.css文件（在这种情况下，[`style-loader`](https://github.com/webpack-contrib/style-loader#readme)将在运行时处理注入`<style>`标签，就像在运行开发服务器时一样）。

##### `hoisting`: `Boolean`{#hoisting-boolean}

使用Webpack 3的`ModuleConcatenationPlugin`启用[partial scope hoisting](https://github.com/webpack/webpack/tree/master/examples/scope-hoisting#readme)。

```js
module.exports = {
  webpack: {
    hoisting: true
  }
}
```

##### `html`: `Object`{#html-object}

配置[HtmlWebpackPlugin的选项](https://github.com/ampedandwired/html-webpack-plugin#readme)。

对于应用程序，nwb将寻找一个`src/index.html`模板注入`<link>`和`<script>`标签，由Webpack生成的CSS和JavaScript的bundle。

如果您想要使用其他地方的HTML文件，请使用`template`config：

```js
module.exports = {
  webpack: {
    html: {
      template: 'html/index.html'
    }
  }
}
```

如果您在`src/index.html`没有模板，或者没有通过`template`指定一个模板，那么nwb将会退回到使用可以配置的以下属性的基本模板：

- `title` - `<title>`的内容

  > 默认值：您应用程序的package.json中的`name`值

- `mountId` - 为挂载您app所提供的`<div>`标签的`id`

  > Default: `'app'`

```js
module.exports = {
  webpack: {
    html: {
      mountId: 'root',
      title: 'Unimaginative documentation example'
    }
  }
}
```

也可以使用其他`HtmlWebpackPlugin`选项。例如，如果您的`src/`目录中有一个`favicon.ico`，你可以将它包含在你的app构建时生成的`index.html`中，并将它复制到输出目录中，如下所示：

```js
module.exports = {
  webpack: {
    html: {
      favicon: 'src/favicon.ico'
    }
  }
}
```

##### `install`: `Object`

Configures [options for `NpmInstallPlugin`](https://github.com/webpack-contrib/npm-install-webpack-plugin#usage), which will be used if you pass an `--install` flag to nwb commands which run a development server.

##### `rules`: `Object`

Each [Webpack rule](https://webpack.js.org/configuration/module/#module-rules) generated by nwb has a unique id you can use to customise it:

```js
module.exports = {
  webpack: {
    rules: {
      // Inline GIFs and PNGs smaller than 10KB
      graphics: {
        limit: 10000
      }
    }
  }
}
```

Rule configuration objects can contain rule options such as `include` and `exclude` (which control which files the rule applies to) and options for the rule's loader (which control what happens to the resource).

Loader options can be provided directly in the configuration object, as shown above, or you can use a nested `options` object.

Refer to the documentation of the Webpack loader used in each rule (linked to in [default rules](#default-rules) below) for options which can be configured.

###### Default Rules

Default rules generated by nwb and their associated ids are:

- `babel` - handles `.js` files with [babel-loader][babel-loader]

  > Default config: `{exclude: /node_modules/, options: {babelrc: false, cacheDirectory: true}}`

- `graphics` - handles `.gif`, `.png` and `.webp` files using using [url-loader][url-loader]

- `svg` - handles `.svg` files using using [url-loader][url-loader]

- `jpeg` - handles `.jpg` and `.jpeg` files using [url-loader][url-loader]

- `fonts` - handles `.eot`, `.otf`, `.ttf`, `.woff` and `.woff2` files using [url-loader][url-loader]

- `video` - handles `.mp4`, `.ogg` and `.webm` files using [url-loader][url-loader]

- `audio` - handles `.wav`, `.mp3`, `.m4a`, `.aac`, and `.oga` files using [url-loader][url-loader]

> Default config for all url-loaders is `{options: {limit: 1, name: '[name].[hash:8].[ext]'}}`.
>
> Default `limit` config prevents any files being inlined by default, while allowing you to configure `url-loader` to enable inlining if you need it.

See the [Stylesheets documentation](/docs/Stylesheets.md#) for default rules generated for stylesheets.

###### Customising loaders

Provide `loader` config for a rule to replace its loader:

```js
module.exports = {
  webpack: {
    rules: {
      svg: {
        loader: 'svg-inline-loader',
        options: {classPrefix: true}
      }
    }
  }
}
```

To chain loaders, provide `use` config:

```js
module.exports = {
  webpack: {
    rules: {
      svg: {
        use: [
          {
            loader: 'svg-inline-loader',
            options: {classPrefix: true}
          },
          'image-webpack-loader'
        ]
      }
    }
  }
}
```

###### Disabling default rules

To disable a default rule, set its id to `false`:

```js
module.exports = {
  webpack: {
    rules: {
      svg: false
    }
  }
}
```

##### `publicPath`: `String`

> This is just Webpack's [`output.publicPath` config](https://webpack.js.org/configuration/output/#output-publicpath) pulled up a level to make it more convenient to configure.

`publicPath` defines the URL static resources will be referenced by in build output, such as `<link>` and `<src>` tags in generated HTML, `url()` in stylesheets and paths to any static resources you `require()` into your modules.

The default `publicPath` configured for most app builds is `/`, which assumes you will be serving your app's static resources from the root of whatever URL it's hosted at:

```html
<script src="/app.12345678.js"></script>
```

If you're serving static resources from a different path, or from an external URL such as a CDN, set it as the `publicPath`:

```js
module.exports = {
  webpack: {
    publicPath: 'https://cdn.example.com/myapp/'
  }
}
```

The exception is the React component demo app, which doesn't set a `publicPath`, generating a build without any root URL paths to static resources. This allows you to serve it at any path without configuration (e.g. on GitHub Project Pages), or open the generated `index.html` file directly in a browser, which is ideal for distributing app builds which don't require a server to run.

If you want to create a path-independent build, set `publicPath` to blank or `null`:

```js
module.exports = {
  webpack: {
    publicPath: ''
  }
}
```

The trade-off for path-independence is HTML5 History routing won't work, as serving up `index.html` at anything but its real path will mean its static resource URLs won't resolve. You will have to fall back on hash-based routing if you need it.

##### `styles`: `Object | false | 'old'`

Configures how nwb creates Webpack config for importing stylesheets.

See the [Stylesheets documentation](/docs/Stylesheets.md#stylesheets) for details.

##### `uglify`: `Object | false`

Configures [options for Webpack's `UglifyJsPlugin`](https://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin), which will be used when creating production builds.

Any additional options provided will be merged into nwb's defaults, which are:

```js
{
  compress: {
    warnings: false
  },
  output: {
    comments: false
  },
  sourceMap: true
}
```

For example, if you want to strip development-only code but keep the output readable for debugging:

```js
module.exports = {
  webpack: {
    uglify: {
      mangle: false,
      beautify: true
    }
  }
}
```

To completely disable use of UglifyJS, set `uglify` to false:

```js
module.exports = {
  webpack: {
    uglify: false
  }
}
```

##### `extra`: `Object`

Extra configuration to be merged into the generated Webpack configuration using [webpack-merge](https://github.com/survivejs/webpack-merge#webpack-merge---merge-designed-for-webpack) - see the [Webpack configuration docs](https://webpack.js.org/configuration/) for the available properties.

> **Note:** Webpack 2 validates the structure of the config it's given, so providing invalid or unexpected config will break the build.

e.g. to add an extra rule which isn't managed by nwb's own `webpack.rules` config, you would need to provide a list of rules at `webpack.extra.module.rules`.

```js
var path = require('path')

module.exports = function(nwb) {
  return {
    type: 'react-app',
    webpack: {
      extra: {
        // Example of adding an extra rule which isn't managed by nwb,
        // assuming you have installed html-loader in your project.
        module: {
          rules: [
            {test: /\.html$/, loader: 'html-loader'}
          ]
        },
        // Example of adding an extra plugin which isn't managed by nwb
        plugins: [
          new nwb.webpack.optimize.MinChunkSizePlugin({
            minChunkSize: 1024
          })
        ]
      }
    }
  }
}
```

##### `config`: `Function`

Finally, if you need *complete* control, provide a `webpack.config()` function which will be given the generated config.

> **Note:** you *must* return a config object from this function.

```js
module.exports = {
  webpack: {
    config(config) {
      // Change config as you wish

      // You MUST return the edited config object
      return config
    }
  }
}
```

### Dev Server Configuration

nwb uses [Webpack Dev Server](https://github.com/webpack/webpack-dev-server#readme) to serve apps for development - you can tweak the options nwb uses and also enable additional features.

#### `devServer`: `Object`

Configuration for Webpack Dev Server - see Webpack's [`devServer` config documentation](https://webpack.js.org/configuration/dev-server/#devserver) for the available options.

Any `devServer` options provided will be merged on top of the following default options nwb uses:

```js
{
  historyApiFallback: true,
  hot: true,
  noInfo: true,
  overlay: true,
  publicPath: webpackConfig.output.publicPath,
  quiet: true,
  watchOptions: {
    ignored: /node_modules/,
  },
}
```

Notable features you can configure using these options:

- [`devServer.historyApiFallback`](https://webpack.js.org/configuration/dev-server/#devserver-historyapifallback) - configure `disableDotRule` if you need to use dots in your path when using the HTML5 History API.

- [`devServer.https`](https://webpack.js.org/configuration/dev-server/#devserver-https) - enable HTTPS with a default self-signed certificate, or provide your own certificates.

- [`devServer.overlay`](https://webpack.js.org/configuration/dev-server/#devserver-overlay) - disable the compile error overlay, or have it also appear for warnings.

- [`devServer.proxy`](https://webpack.js.org/configuration/dev-server/#devserver-proxy) - proxy certain URLs to a separate API backend development server.

- [`devServer.setup`](https://webpack.js.org/configuration/dev-server/#devserver-setup) - access the Express app to add your own middleware to the dev server.

e.g. to enable HTTPS:

```js
module.exports = {
  devServer: {
    https: true
  }
}
```

### Karma Configuration

nwb's default [Karma](http://karma-runner.github.io/) configuration uses the [Mocha](https://mochajs.org/) framework and reporter plugins for it, but you can configure your own preferences.

> **Note:** At runtime, Karma sets a `usePolling` autoWatch option to `true` [if the platform is detected to be macOS or Linux](https://github.com/karma-runner/karma/blob/master/lib/config.js#L318). However, Karma's non-polling file-watching works correctly and consumes dramatically less CPU on macOS. nwb users on macOS will want to set `usePolling: false` within the [`extra:`](#extra-object-1) Object in the `karma:` config section of their `nwb.config.js`.

#### `karma`: `Object`

Karma configuration can be provided in a `karma` object, using the following properties:

##### `browsers`: `Array<String | Plugin>`

> Default: `['PhantomJS']`

A list of browsers to run tests in.

PhantomJS is the default as it's installed by default with nwb and should be able to run in any environment.

The launcher plugin for Chrome is also included, so if you want to run tests in Chrome, you can just name it:

```js
module.exports = {
  karma: {
    browsers: ['Chrome']
  }
}
```

For other browsers, you will also need to supply a plugin and manage that dependency yourself:

```js
module.exports = {
  karma: {
    browsers: ['Firefox'],
    plugins: [
      require('karma-firefox-launcher')
    ]
  }
}
```

nwb can also use the first browser defined in a launcher plugin if you pass it in `browsers`:

```js
module.exports = {
  karma: {
    browsers: [
      'Chrome',
      require('karma-firefox-launcher')
    ]
  }
}
```

##### `excludeFromCoverage`: `String | Array<String>`

> Default: `['test/', 'tests/', 'src/**/__tests__/']`

Globs for paths which should be excluded from code coverage reporting.

##### `frameworks`: `Array<String | Plugin>`

> Default: `['mocha']`

Karma testing framework plugins.

You must provide the plugin for any custom framework you want to use and manage it as a dependency yourself.

e.g. if you're using a testing framework which produces [TAP](https://testanything.org/) output (such as [tape](https://github.com/substack/tape)). this is how you would use `frameworks` and `plugins` props to configure Karma:

```
npm install --save-dev karma-tap
```
```js
module.exports = {
  karma: {
    frameworks: ['tap'],
    plugins: [
      require('karma-tap')
    ]
  }
}
```

nwb can also determine the correct framework name given the plugin itself, so the following is functionally identical to the configuration above:

```js
module.exports = {
  karma: {
    frameworks: [
      require('karma-tap')
    ]
  }
}
```

If a plugin module provides multiple plugins, nwb will only infer the name of the first plugin it provides, so pass it using `plugins` instead and list all the frameworks you want to use, for clarity:

```js
module.exports = {
  karma: {
    frameworks: ['mocha', 'chai', 'chai-as-promised'],
    plugins: [
      require('karma-chai-plugins') // Provides chai, chai-as-promised, ...
    ]
  }
}
```

> **Note:** If you're configuring frameworks and you want to use the Mocha framework plugin managed by nwb, just pass its name as in the above example.

##### `testContext`: `String`

Use this configuration to point to a [Webpack context module](/docs/Testing.md#using-a-test-context-module) for your tests if you need to run code prior to any tests being run, such as customising the assertion library you're using, or global before and after hooks.

If you provide a context module, it is responsible for including tests via Webpack's  `require.context()` - see the [example in the Testing docs](/docs/Testing.md#using-a-test-context-module).

If the default [`testFiles`](#testfiles-string--arraystring) config wouldn't have picked up your tests, you must also configure it so they can be excluded from code coverage.

##### `testFiles`: `String | Array<String>`

> Default: `.spec.js`, `.test.js` or `-test.js` files anywhere under `src/`, `test/` or `tests/`

[Minimatch glob patterns](https://github.com/isaacs/minimatch) for test files.

If [`karma.testContext`](#testcontext-string) is not being used, this controls which files Karma will run tests from.

This can also be used to exclude tests from code coverage if you're using [`karma.testContext`](#testcontext-string) - if the default `testFiles` patterns wouldn't have picked up your tests, configure this as well to exclude then from code coverage.

##### `plugins`: `Array<Plugin>`

A list of plugins to be loaded by Karma - this should be used in combination with [`browsers`](#browsers-arraystring--plugin), [`frameworks`](#frameworks-arraystring--plugin) and [`reporters`](#reporters-arraystring--plugin) config as necessary.

##### `reporters`: `Array<String | Plugin>`

> Default: `['mocha']`

Customising reporters follows the same principle as frameworks, just using the `reporters` prop instead.

For built-in reporters, or nwb's version of the Mocha reporter, just pass a name:

```js
module.exports = {
  karma: {
    reporters: ['progress']
  }
}
```

For custom reporters, install and provide the plugin:

```
npm install --save-dev karma-tape-reporter
```
```js
module.exports = {
  karma: {
    reporters: [
      require('karma-tape-reporter')
    ]
  }
}
```

##### `extra`: `Object`

Extra configuration to be merged into the generated Karma configuration using [webpack-merge](https://github.com/survivejs/webpack-merge#webpack-merge---merge-designed-for-webpack).

> **Note:** you *must* use Karma's own config structure in this object.

e.g. to tweak the configuration of the default Mocha reporter:

```js
module.exports = {
  karma: {
    extra: {
      mochaReporter: {
        divider: '°º¤ø,¸¸,ø¤º°`°º¤ø,¸,ø¤°º¤ø,¸¸,ø¤º°`°º¤ø,¸',
        output: 'autowatch'
      }
    }
  }
}
```

### npm Build Configuration

By default, nwb creates ES5 and ES6 modules builds for publishing to npm.

#### `npm`: `Object`

npm build configuration is defined in a `npm` object, using the following fields:

##### `cjs`: `Boolean`

> Defaults to `true` if not provided.

Determines whether or not nwb will create a CommonJS build in `lib/` when you run `nwb build` for a React component/library or web module project.

Set to `false` to disable this:

```js
module.exports = {
  npm: {
    cjs: false
  }
}
```

##### `esModules`: `Boolean`

> Defaults to `true` if not provided.

Determines whether or not nwb will create an ES6 modules build for use by ES6 module bundlers when you run `nwb build` for a React component/library or web module project.

When providing an ES6 modules build, you should also provide the following in `package.json` so compatible module bundlers can find it:

```
"module": "es/index.js",
```

These are included automatically if you create a project with an ES6 modules build enabled.

##### `umd`: `String | Object`

Configures creation of a UMD build when you run `nwb build` for a React component/library or web module.

If you just need to configure the global variable the UMD build will export, you can use a String:

```js
module.exports = {
  npm: {
    umd: 'MyLibrary'
  }
}
```

If you also have some external dependencies to configure, use an object containing the following properties:

###### `global`: `String`

The name of the global variable the UMD build will export.

###### `externals`: `Object`

A mapping from `peerDependency` module names to the global variables they're expected to be available as for use by the UMD build.

e.g. if you're creating a React component which also depends on [React Router](https://github.com/reactjs/react-router), this configuration would ensure they're not included in the UMD build:

```js
module.exports = {
  npm: {
    umd: {
      global: 'MyComponent',
      externals: {
        'react': 'React',
        'react-router': 'ReactRouter'
      }
    }
  }
}
```

#### `package.json` UMD Banner Configuration

A banner comment added to UMD builds will use as many of the following `package.json` fields as are present:

- `name`
- `version`
- `homepage`
- `license`

If all fields are present the banner will be in this format:

```js
/*!
 * your-project v1.2.3 - https://github.com/you/your-project
 * MIT Licensed
 */
```

[babel-loader]: https://github.com/babel/babel-loader
[url-loader]: https://github.com/webpack-contrib/url-loader

