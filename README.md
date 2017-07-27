# nwb{#nwb}

![Linux](https://github.com/insin/nwb/blob/master/resources/linux.png) [![Travis][travis-badge]][travis]
![Windows](https://github.com/insin/nwb/blob/master/resources/windows.png) [![Appveyor][appveyor-badge]][appveyor]
[![npm package][npm-badge]][npm]
[![Coveralls][coveralls-badge]][coveralls]

![nwb](https://github.com/insin/nwb/raw/master/resources/cover.jpg)

nwb是一个工具包：

- [快速用React，Inferno，Preact 或 vanilla JavaScript开发项目](#quick-development)
- 开发:
  - [React Apps](#react-apps)
  - [Preact Apps](#preact-apps)
  - [Inferno Apps](#inferno-apps)
  - [Vanilla JavaScript Apps](#vanilla-javascript-apps)
  - [React Components and Libraries](#react-components-and-libraries)
  - [npm Modules for the Web](#npm-modules-for-the-web)

提供零配置开发设置，但是nwb也支持[配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#configuration)和[插件模块](https://github.com/insin/nwb/blob/master/docs/Plugins.md#plugins)，它添加额外的功能（例如：支持[Sass](http://sass-lang.com/)），你应该需要它们

## 安装{#Install}

全局安装将提供了一个`nwb`命令，用于快速从事项目开发。

```sh
npm install -g nwb
```

> 建议使用**npm> = 3**，npm 2由于缺乏重复数据删除，使用它Babel 6需要更多的时间和磁盘空间来安装。

要在项目中使用nwb的工具，请将其作为`devDependency`安装，并在`package.json``"scripts"`中使用`nwb`命令：

```sh
npm install --save-dev nwb
```
```json
{
  "scripts": {
    "start": "nwb serve-react-app",
    "build": "nwb build-react-app"
  }
}
```

## 快速开发{#Quick-Development}

要快速使用React，Inferno，Preact或vanilla JavaScript开发，请使用`nwb react`，`nwb inferno`，`nwb preact`或`nwb web`命令。

```js
import React, {Component} from 'react'

export default class App extends Component {
  render() {
    return <h1>Hello world!</h1>
  }
}
```
```sh
$ nwb react run app.js
✔ Installing react and react-dom
Starting Webpack compilation...
Compiled successfully in 5033 ms.

The app is running at http://localhost:3000/
```
```sh
$ nwb react build app.js
✔ Building React app

File size after gzip:

  dist\app.cff417a3.js  46.72 KB
```

**请参阅[快速使用nwb开发](/docs/guides/QuickDevelopment.md#quick-development-with-nwb)以获取更详细的指南。**

## React应用{#react-apps}

使用`nwb new react-app`创建一个[React](https://facebook.github.io/react/)应用程序骨架，预配置使用`nwb`命令的npm脚本进行开发：

```sh
nwb new react-app my-app
cd my-app/
npm start
```

打开[localhost:3000](http://localhost:3000)，开始编辑代码，更改将被热加载到正在运行的应用程序中。

`npm test`将运行应用程序的测试，并且`npm run build`将创建一个生产的构建。

**请参阅[使用nwb开发React应用程序](/docs/guides/QuickDevelopment.md#quick-development-with-nwb)以获取更详细的指南。**

## Preact应用{#preact-apps}

使用`nwb new preact-app`创建一个[Preact](https://preactjs.com/)应用程序骨架：

```sh
nwb new preact-app my-app
```

## Inferno应用{#inferno-apps}

使用`nwb new inferno-app`创建一个[Inferno](https://infernojs.org/)应用程序骨架：

```sh
nwb new inferno-app my-app
```

## Vanilla JavaScript应用{#vanilla-javascript-apps}

使用`nwb new web-app`创建一个vanilla JavaScript应用程序骨架：

```sh
nwb new web-app my-app
```

## React组件和库{#react-components-and-libraries}

```sh
nwb new react-component my-component

cd my-component/
```

`npm start`将运行一个演示应用程序，您可以使用它来开发您的组件或库。

`npm test`将运行项目的测试，`npm run build`将创建ES5，ES6模块和UMD版本，以便发布到npm。

**请参阅[使用nwb开发React组件和库](/docs/guides/ReactComponents.md#developing-react-components-and-libraries-with-nwb)以获取更详细的指南。**

## web npm模块{#npm-modules-for-the-web}

```sh
nwb new web-module my-module

cd my-module/
```

`npm test`将运行项目的测试，并且`npm run build`将创建ES5，ES6模块和UMD版本以便发布到npm。

## [指南](/docs/guides/#table-of-contents)

- [快速使用nwb开发](/docs/guides/QuickDevelopment.md#quick-development-with-nwb)
- [使用nwb开发React应用程序](/docs/guides/ReactApps.md#developing-react-apps-with-nwb)
- [使用nwb开发React组件和库](/docs/guides/ReactComponents.md#developing-react-components-and-libraries-with-nwb)

## [文档](/docs/#table-of-contents)

- [特点](https://github.com/insin/nwb/blob/master/docs/Features.md#features)
- [命令](https://github.com/insin/nwb/blob/master/docs/Commands.md#commands)
  - [`nwb`](https://github.com/insin/nwb/blob/master/docs/Commands.md#nwb)
  - [`nwb react`, `nwb inferno`, `nwb preact` and `nwb web`](https://github.com/insin/nwb/blob/master/docs/guides/QuickDevelopment.md#quick-development-with-nwb)
- [配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#configuration)
  - [配置文件](https://github.com/insin/nwb/blob/master/docs/Configuration.md#configuration-file)
  - [配置对象](https://github.com/insin/nwb/blob/master/docs/Configuration.md#configuration-object)
    - [Babel配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#babel-configuration)
    - [Webpack配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#webpack-configuration)
    - [Karma配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#karma-configuration)
    - [npm Build配置](https://github.com/insin/nwb/blob/master/docs/Configuration.md#npm-build-configuration)
- [测试](https://github.com/insin/nwb/blob/master/docs/Testing.md#testing)
- [插件](https://github.com/insin/nwb/blob/master/docs/Plugins.md#plugins)
- [中间件](https://github.com/insin/nwb/blob/master/docs/Middleware.md#middleware)
- [示例](https://github.com/insin/nwb/blob/master/docs/Examples.md#examples)
- [常见问题](https://github.com/insin/nwb/blob/master/docs/FAQ.md#frequently-asked-questions)
- [版本](https://github.com/insin/nwb/blob/master/docs/Versioning.md#versioning)

## 为什么使用nwb ?

**快速开始**. 从单个`.js`文件开始开发，或者[生成一个项目骨架](https://github.com/insin/nwb/blob/master/docs/Commands.md#new)。

**涵盖整个开发周期**. 开发工具，测试和生产构建项目开箱即用，无需配置。

**灵活**. 虽然一切都可以开箱即用，您也可以使用可选的[配置文件](https://github.com/insin/nwb/blob/master/docs/Configuration.md#configuration-file)来调整您的喜好。

**管理您的主要开发依赖关系和配置**. 查看使用nwb的效果的[示例](https://github.com/insin/react-yelp-clone/compare/master...nwb)，它被放入在真实项目中管理devDependencies和配置数量的效果。

## MIT Licensed

*封面图片由[GeorgioWan](https://github.com/GeorgioWan)创建*

*使用[Icons8](https://icons8.com/)创建的操作系统图标*

[travis-badge]: https://img.shields.io/travis/insin/nwb/master.png?style=flat-square
[travis]: https://travis-ci.org/insin/nwb

[appveyor-badge]: https://img.shields.io/appveyor/ci/insin/nwb/master.png?style=flat-square
[appveyor]: https://ci.appveyor.com/project/insin/nwb

[npm-badge]: https://img.shields.io/npm/v/nwb.png?style=flat-square
[npm]: https://www.npmjs.org/package/nwb

[coveralls-badge]: https://img.shields.io/coveralls/insin/nwb/master.png?style=flat-square
[coveralls]: https://coveralls.io/github/insin/nwb