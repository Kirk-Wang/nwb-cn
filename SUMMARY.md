# Summary

* [扉页](README.md#nwb)
    * [安装](README.md#Install)
    * [快速开发](README.md#quick-development)
    * [React应用](README.md#react-apps)
    * [Preact应用](README.md#preact-apps)
    * [Inferno应用](README.md#inferno-apps)
    * [Vanilla JavaScript应用](README.md#vanilla-javascript-apps)
    * [React组件和库](README.md#react-components-and-libraries)
    * [web npm模块](README.md#npm-modules-for-the-web)
    * [指南](README.md#guides)
    * [文档](README.md#documentation)
    * [为什么使用nwb ?](README.md#why-use-nwb)
* [配置](Configuration/README.md#Configuration)
    * [配置文件](Configuration/README.md#Configuration-File)
    * [配置文件示例](Configuration/README.md#Example-Configuration-Files)
    * [通过参数配置](Configuration/README.md#Configuration-Via-Arguments)
    * [配置对象](Configuration/README.md#Configuration-Object)
        * [type](Configuration/README.md#type-string-required-for-generic-build-commands)
        * [polyfill](Configuration/README.md#polyfill-boolean)
        * [Babel配置](Configuration/README.md#babel-configuration)
            * [babel](Configuration/README.md#babel-object)
            * [babel.cherryPick](Configuration/README.md#cherrypick-string--arraystring)
            * [babel.loose](Configuration/README.md#loose-boolean)
            * [babel.plugins](Configuration/README.md#plugins-string--array)
            * [babel.presets](Configuration/README.md#presets-string--array)
            * [babel.removePropTypes](Configuration/README.md#removeproptypes-object--false)
            * [babel.reactConstantElements](Configuration/README.md#reactconstantelements-false)
            * [babel.runtime](Configuration/README.md#runtime-string--boolean)
            * [babel.stage](Configuration/README.md#stage-number--false)
        * [Webpack配置](Configuration/README.md#webpack-configuration)
            * [webpack](Configuration/README.md#webpack-object)
            * [webpack.aliases](Configuration/README.md#aliases-object)
            * [webpack.autoprefixer](Configuration/README.md#autoprefixer-string--object)
            * [webpack.compat](Configuration/README.md#compat-object)
            * [webpack.debug](Configuration/README.md#debug-boolean) 
            * [webpack.define](Configuration/README.md#define-object)
            * [webpack.extractText](Configuration/README.md#extracttext-object--boolean)
            * [webpack.hoisting](Configuration/README.md#hoisting-boolean)
            * [webpack.html](Configuration/README.md#html-object)
            * [webpack.install](Configuration/README.md#install-object)
            * [webpack.rules](Configuration/README.md#rules-object)
                * [默认规则](Configuration/README.md#default-rules)
                * [定制loader](Configuration/README.md#customising-loaders)
                * [禁用默认规则](Configuration/README.md#disabling-default-rules)
            * [webpack.publicPath](Configuration/README.md#publicpath-string)
            * [webpack.styles](Configuration/README.md#styles-object--false--old)
            * [webpack.uglify](Configuration/README.md#uglify-object--false)
            * [webpack.extra](Configuration/README.md#extra-object)
            * [webpack.config](Configuration/README.md#config-function)
        * [Dev Server配置](Configuration/README.md#dev-server-configuration)
            * [devServer](Configuration/README.md#devserver-object)
        * [Karma配置](Configuration/README.md#karma-configuration)
            * [karma](Configuration/README.md#karma-object)
            * [karma.browsers](Configuration/README.md#browsers-arraystring--plugin)
            * [karma.excludeFromCoverage](Configuration/README.md#excludefromcoverage-string--arraystring)
            * [karma.frameworks](Configuration/README.md#frameworks-arraystring--plugin)
            * [karma.plugins](Configuration/README.md#plugins-arrayplugin)
            * [karma.reporters](Configuration/README.md#reporters-arraystring--plugin)
            * [karma.testContext](Configuration/README.md#testcontext-string)
            * [karma.testFiles](Configuration/README.md#testfiles-string--arraystring)
            * [karma.extra](Configuration/README.md#extra-object-1)
        * [npm 构建配置](Configuration/README.md#npm-build-configuration)
            * [npm](Configuration/README.md#npm-object)
            * [npm.cjs](Configuration/README.md#cjs-boolean)
            * [npm.esModules](Configuration/README.md#esmodules-boolean)
            * [npm.umd](Configuration/README.md#umd-string--object)
                * [umd.global](Configuration/README.md#global-string)
                * [umd.externals](Configuration/README.md#externals-object)
            * [package.json字段](Configuration/README.md#packagejson-umd-banner-configuration)
* [使用nwb开发React应用程序](ReactApps/README.md#Developing-React-Apps-with-nwb)
    * [创建新项目](ReactApps/README.md#creating-a-new-project)
    * [项目布局](ReactApps/README.md#project-layout)
    * [`npm run`脚本](ReactApps/README.md#npm-run-scripts)
    * [运行开发服务器](ReactApps/README.md#running-the-development-server)
    * [零配置开发设置](ReactApps/README.md#zero-configuration-development-setup)
    * [React组件思想](ReactApps/README.md#npm-run-scripts)
        * [React组件思想](ReactApps/README.md#npm-run-scripts)

            


    
