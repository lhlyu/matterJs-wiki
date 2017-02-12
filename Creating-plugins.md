> Creating Matter.js plugins 

1. [Using plugins](#using-plugins)
1. [Creating plugins](#creating-plugins)
    1. [Example](#example)
    1. [Boilerplate](#boilerplate)
    1. [Versioning](#versioning)
    1. [Registering](#registering)
    1. [Patching](#patching)
    1. [Using other plugins as dependencies](#using-other-plugins-as-dependencies)
1. [Guidelines for plugins](#guidelines-for-plugins)
1. [Documentation](#documentation)
1. [Examples](#examples)

**Experimental**  
_The following information may be subject to change_

## Using plugins

To find out how to use and install plugins, see the wiki page on [using plugins](https://github.com/liabru/matter-js/wiki/Using-plugins). This article explains how you can make your own.

## Creating plugins

### Example

Here is a basic Matter.js plugin boilerplate:

```js
var MyPlugin = {
    name: 'matter-my-plugin',

    version: '0.1.0',

    for: 'matter-js@^0.10.0',

    uses: [
        'matter-some-dependency@^0.1.0',
        'matter-another-dependency@^0.1.0'
    ],

    options: {
        something: true
    },

    install: function(base) {
        // patch the Matter namespace here
    },

    // implement your plugin functions etc...
};

Matter.Plugin.register(MyPlugin);
```

A description of the **required** properties:

- `name` _string, **required**_ The unique name of the plugin (e.g. same as package.json)
- `version` _string, **required**_ The version of the plugin (a limited subset of [semver](http://semver.org/))
- `install` _function, **required**_ The install function, it will be called only once and is passed a reference to the module it is being installed on as the first argument

There are some additional optional properties:

- `for` _string, optional_ The name and optionally version of module this plugin should be installed on
- `uses` _array, optional_ The names and optionally versions of any other plugins this plugin requires to be installed before itself It is also possible to directly pass references to plugin objects here, but it is not recommended
- `options` _object, optional_ Any global options for the plugin

While the above boilerplate is given as a guide, any equivalent representations exposing these properties should also work (such as [CommonJS](https://webpack.github.io/docs/commonjs.html) or [ES6 modules](http://exploringjs.com/es6/ch_modules.html)).

### Boilerplate

A complete, production ready boilerplate starter project is available at [matter-plugin-boilerplate](https://github.com/liabru/matter-plugin-boilerplate) which you can fork to start your plugin with all the essentials (build, dev, lint, test, doc, release etc.).

### Versioning

Plugins are versioned using the [semver](http://semver.org/) approach, making it easier to specify compatibility. Versions may be specified for plugins themselves, the version of Matter.js they are recommended for and the versions of their dependencies.

Versions are strictly of the format `x.y.z` (as in [semver](http://semver.org/)).
Versions may optionally have a prerelease tag in the format `x.y.z-alpha`.
Ranges are a strict subset of [npm ranges](https://docs.npmjs.com/misc/semver#advanced-range-syntax).
Only the following range types are supported:

- Tilde ranges e.g. `~1.2.3`
- Caret ranges e.g. `^1.2.3`
- Exact version e.g. `1.2.3`
- Any version `*`

If a version or range is not specified, then any version (`*`) is assumed to satisfy.

### Registering

It is very important to call `Plugin.register` after your plugin definition.
This allows it to be resolved by name inside the plugin system, which is the recommended way to specify dependencies.

```js
Matter.Plugin.register(MyPlugin);
```

### Patching

A plugin's `install` function is where it should apply patches that implement the plugin's features on `Matter.*` modules.

Included in the library are the functions `Matter.before` and `Matter.after` that you should use for patching in most cases, as they will help ensure that you do not break the original function or any other plugins that may also patch it. 

Here is an example of the recommended approach:

```js
var MyPlugin = {
    // ...

    install: function(base) {
        base.before('Engine.create', function(options) {
            MyPlugin.Engine.beforeCreate(options);
        });

        base.after('Engine.create', function() {
            MyPlugin.Engine.afterCreate(this);
        });
    },

    Engine: {
        beforeCreate: function(options) {
            console.log('Creating engine for:', options);
        },
        afterCreate: function(engine) {
            console.log('Engine created:', engine);
        }
    }
};

// ...
```

When this plugin is installed, it will log to the console before and after `Matter.Engine.create` gets called.

Under the hood, `Matter.before` and `Matter.after` are both helpers for `Common.chain`.

Tips for patching:
- the `arguments` passed are the same for each function in the chain
- the value of `this` is the last object returned in the chain
- don't change the original function signature
- don't return inside a `Matter.before` and `Matter.after` unless you intend to change the result
- when returning, ensure the same type as the patched function
- don't store references to `Matter.*` functions (as they may reference unpatched versions)

### Using other plugins as dependencies

Plugins may be broken down in to smaller parts, shared and combined. Dependencies can be specified inside the `uses` property, and these will be installed _before_ the plugin that specifies them.

```js
var MyPlugin = {
    // ...

    uses: [
        'matter-some-dependency@^0.1.0',
        'matter-another-dependency@^0.1.0'
    ],

    // ...
};

// ...
```

Note that these dependencies will only ever be installed once, no matter how many plugins use them.
If a plugin's code is loaded multiple times, the one with the highest version will be used.

## Guidelines for plugins

Some general guidelines for plugins:

- consider whether to implement as a plugin at all (a pull request might be more appropriate)
- version everything
- document everything (code and readme)
- build plugins with sharing and reuse in mind
- don't store references to `Matter.*` functions (as they may reference unpatched versions)
- don't _add_ new functions to modules or namespaces you don't maintain (only patch existing functions)
- add custom properties to `obj.plugin` e.g. `body.plugin.newFeature.enabled`
- follow the same namespacing structure as the core (e.g. `MyPlugin.Body.init`, `MyPlugin.Engine.update`)
- expose and implement your plugin's functions so that they can be called manually
- avoid relying a particular order of execution where possible
- the smaller the better
- but avoid unnecessary dependencies
- don't modify options or settings in unexpected or undocumented ways
- use conditionals before operating on possibly undefined properties
- don't bundle dependencies (document them)

## Documentation

Check the [documentation](https://github.com/liabru/matter-js/blob/plugins/src/core/Plugin.js) for a full description of the `Matter.Plugin` API.

## Examples

See the [list of plugins](https://github.com/liabru/matter-js/wiki/List-of-plugins) for more code examples of how plugins are implemented.