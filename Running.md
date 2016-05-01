> Examples of how to run a Matter.js engine

1. [Runner example](#runner-example)
1. [Using Matter.Runner](#using-matter-runner)
    1. [Documentation](#documentation)
    1. [Usage](#usage)
    1. [Options](#options)

## Runner example

A basic example for running a previously created `Matter.Engine` (see [Getting started](https://github.com/liabru/matter-js/wiki/Getting-started)):

```js
(function run() {
    window.requestAnimationFrame(run);
    Engine.update(engine, 1000 / 60);
})();
```

See also the source of [Matter.Runner](https://github.com/liabru/matter-js/blob/master/src/core/Runner.js) that provides some more advanced features.

## Using Matter.Runner

There is an included runner called [Matter.Runner](http://brm.io/matter-js/docs/classes/Runner.html).
This module is an optional utility which provides a game loop, that handles continuously updating a `Matter.Engine` for you. It is intended for development and debugging purposes, but may also be suitable for simple games.

#### Documentation

See the documentation for [Matter.Runner](http://brm.io/matter-js/docs/classes/Runner.html).

#### Usage

The simplest way is to use the `Engine.run` helper:

```js
var engine = Engine.create();
Engine.run(engine);
```

Alternatively you can create a runner directly:

```js
var runner = Runner.create();
Runner.run(runner, engine);
```

#### Options

A number of options may be passed to `Matter.Runner.create`:

```js
var runner = Runner.create({
    delta: 1000 / 60,
    isFixed: false,
    enabled: true
});
```