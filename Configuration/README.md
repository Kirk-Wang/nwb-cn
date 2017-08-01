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

Configuration for the `babel`, `webpack`, `devServer`, `karma` and `npm` properties documented below can also be provided via arguments using dotted paths, instead of tweaking your `nwb.config.js` file for a single run, or instead of needing to add a config file for [Quick Development commands](/docs/guides/QuickDevelopment.md#quick-development-with-nwb):

```sh
nwb build-react-app --babel.stage=2 --webpack.hoisting
```

> **Note:** If you have a config file, these arguments will act as overrides.
>
> nwb uses [minimist](https://github.com/substack/minimist) for argument parsing, so here's quick cheatsheet if you wish to make use of this functionality:
>
> - `--config.example` is `true`
> - `--no-config.example` is `false`
> - `--config.example=value` is `'value'`
> - `--config.example=one --config.example=two` is `['one', 'two']`

