> 使用Matter.js插件

1. [介绍](#introduction)
1. [使用插件](#using-plugins)
    1. [加载插件](#loading-plugins)
    1. [安装插件](#installing-plugins)
1. [创建列表](#list-of-plugins)
1. [创建创建](#creating-plugins)

**实验**  
_以下信息可能会有所变更_

## 介绍

插件允许开发人员以高度模块化的方式扩展Matter.js的新功能。
它们的设计易于使用，创建和组合。

他们通过自己安装工作，[修补](https://github.com/liabru/matter-js/wiki/Creating-plugins#patching) 的Matter.*直接模块。这种方法是一种以分离和可组合的方式扩展库的几乎任何部分的强大方法。

也可以指定必须首先作为[依赖项](https://github.com/liabru/matter-js/wiki/Creating-plugins#using-other-plugins-as-dependencies)安装的其他插件。这允许通过组合重用插件，例如matter-gravity插件uses插件matter-attractors。

插件系统以递归方式自动跟踪，解析和安装依赖项，确保它们仅以满足所有依赖项的顺序安装一次（如果可能）。

插件使用[semver](http://semver.org/)方法进行 [版本化](https://github.com/liabru/matter-js/wiki/Creating-plugins#versioning)，从而更容易指定兼容性。可以为插件本身指定版本，建议使用它们的Matter.js版本以及它们的依赖项版本。

请注意，虽然它们共享一些共同点，但插件不是新的模块或包格式（因为任何这些现有格式都可用于构建或打包插件）。

## 使用插件

### 加载插件

要使用插件，必须首先加载其文件及其任何依赖项文件。你怎么做，这是你的，最简单的方法是只使用多个<script src="...">标签，但建议使用捆绑如的[webpack](https://webpack.github.io/)或[browserify](http://browserify.org/) (but this is not required)（但不是必需的）。

### 安装插件

加载插件后，您可以使用Matter.use如下方式安装它：

```js
Matter.use(
    'matter-gravity', 
    'matter-world-wrap'
);
```

检查控制台以确认它们已正确安装并按顺序，您应该看到类似的内容：

```
matter-js: ✅ matter-attractors@0.1.0  ✅ matter-gravity@0.1.0  ✅ matter-world-wrap@0.1.0
```

在这里，我们看到它matter-attractors也被安装，因为它是一个依赖matter-gravity。请注意，插件通常不会将它们的依赖项捆绑在它们的发行版中，因此由您来源和加载它们。另请注意，插件只能在给定模块上安装一次。

任何无法解析的插件都会显示红叉❌，任何发出任何警告的插件都会显示🔶。后者可能会或可能不会按预期运行，具体取决于警告的原因。

确保在创建任何函数别名之前尽早在项目生命周期中安装插件（例如var Body = Matter.Body;）否则这些别名将不会引用这些函数的新修补版本，并且插件将以意外方式失败。

您可以在[examples/attractors.js](https://github.com/liabru/matter-js/blob/plugins/examples/airFriction.js)中看到一个使用插件的工作示例。

高级用户应该检查Plugin.use哪些提供了更大的灵活性。


## 插件列表

查看 [插件列表](https://github.com/liabru/matter-js/wiki/List-of-plugins).

## 创建插件

请参阅[创建插件](https://github.com/liabru/matter-js/wiki/Creating-plugins)的Wiki页面。还可以查看[问题 - 插件 - 样板](https://github.com/liabru/matter-plugin-boilerplate)。
