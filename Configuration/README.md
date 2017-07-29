## 配置{#Configuration}

nwb的默认设置可以让您无需任何配置即可开发，测试和构建可生产就绪的应用程序和npm-ready组件。

如果您需要调整默认设置以满足项目需求，或者您想要使用Babel，Karma和Webpack生态系统提供的其他功能，则可以提供配置文件。

> You can also add new functionality by installing a [plugin module](/docs/Plugins.md#plugins).
> 您还可以通过[安装插件](/docs/Plugins.md#plugins)模块来添加新功能。

### Configuration File

By default, nwb will look for an `nwb.config.js` file in the current working directory for configuration.

You can also specify a configuration file using the `--config` option:

```
nwb --config ./config/nwb.js
```

This file should export either a configuration object...

```js
module.exports = {
  // ...
}
```

...or a function which returns a configuration object when called:

```js
module.exports = function(args) {
  return {
    // ...
  }
}
```

If a function is exported, it will be passed an object with the following properties:

- `args`: a parsed version of arguments passed to the `nwb` command
- `command`: the name of the command currently being executed, e.g. `'build'` or `'test'`
- `webpack`: nwb's version of the `webpack` module, giving you access to the other plugins webpack provides.
