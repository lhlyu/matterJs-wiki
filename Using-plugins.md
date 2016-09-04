> Using Matter.js plugins 

1. [Introduction](#introduction)
1. [Using plugins](#using-plugins)
    1. [Loading plugins](#loading-plugins)
    1. [Installing plugins](#installing-plugins)
1. [List of plugins](#list-of-plugins)
1. [Creating plugins](#creating-plugins)

## Notice

**This is a pre-release feature**  
Branch: [plugins](https://github.com/liabru/matter-js/tree/plugins)  
Issue and discussion: [#213](https://github.com/liabru/matter-js/issues/213)  

_The following information may be subject to change_

## Introduction

Plugins allow developers to extend Matter.js with new features in highly modular way.  
They are designed to be simple to use, create and compose.

They work by installing themselves through patching the `Matter.*` modules directly.
This approach is a powerful way to extend virtually any part of the library, in a way that is decoupled and composable.

It is also possible to specify other plugins that must be installed first as dependencies.
This allows plugins to be reused through composition, for example the `matter-gravity` plugin `uses` the plugin `matter-attractors`.

The plugin system automatically tracks, resolves and installs dependencies recursively, ensuring they are installed only once in an order that satisfies all dependencies (where possible).

Plugins are versioned using the [semver](http://semver.org/) approach, making it easier to specify compatibility. Versions may be specified for plugins themselves, the version of Matter.js they are recommended for and the versions of their dependencies.

Note that while they share some things in common, plugins are not a new module or package format (as any of these existing formats may be used to build or package a plugin).

## Using plugins

### Loading plugins
To use a plugin you must first load its files along with any of its dependencies' files.
How you do this is up to you, the simplest way is to just use multiple `<script src="...">` tags,
but it is recommended to use a bundler such as [webpack](https://webpack.github.io/) or [browserify](http://browserify.org/) (but this is not required).

### Installing plugins

Once you have loaded your plugin, you can install it using `Matter.use` like so:

```js
Matter.use(
    'matter-gravity', 
    'matter-world-wrap'
);
```

Check the console to confirm that they were installed correctly and in order, you should see something like:

```
matter-js: ✅ matter-attractors@0.1.0  ✅ matter-gravity@0.1.0  ✅ matter-world-wrap@0.1.0
```

Here we see that `matter-attractors` was also installed, due to it being a dependency of `matter-gravity`. 
Note that plugins do not usually bundle their dependencies in their distributions, so it is down to you to source and load them.
Also note that plugins may only ever be installed once on a given module.

Any plugins that could not be resolved will show a red cross ❌, any plugins that threw any warnings will show a 🔶.
The latter may or may not operate as expected, depending on the reason for the warning.

You can see a working example that uses plugins in [examples/attractors.js](https://github.com/liabru/matter-js/blob/plugins/examples/airFriction.js).

Advanced users should check out `Plugin.use` which provides more flexibility.

## List of plugins

The plugin system is still in pre-release stages at the moment, but here are some existing plugins:

- [matter-attractors](https://github.com/liabru/matter-js/blob/plugins/examples/attractorsPlugin.js)
- [matter-gravity](https://github.com/liabru/matter-js/blob/plugins/examples/gravityPlugin.js)
- [matter-world-wrap](https://github.com/liabru/matter-js/blob/plugins/examples/worldWrapPlugin.js)

If you create a plugin, message [@liabru](https://twitter.com/liabru) and it will be added to this list.

## Creating plugins

See the wiki page on [Creating Plugins](https://github.com/liabru/matter-js/wiki/Creating-plugins).
