> 运行一个 Matter.js engine 示例

1. [运行示例](#runner-example)
1. [使用 Matter.Runner](#using-matterrunner)
    1. [文档](#documentation)
    1. [用法](#usage)
    1. [选项](#options)

## 运行示例

### 浏览器

运行一个之前创建的基本示例 `Matter.Engine` (请看 [入门](https://github.com/liabru/matter-js/wiki/Getting-started)):

```js
(function run() {
    window.requestAnimationFrame(run);
    Engine.update(engine, 1000 / 60);
})();
```

你也可以看其他的资源 [Matter.Runner](https://github.com/liabru/matter-js/blob/master/src/core/Runner.js) 这里提供更高级的一些用法

### 节点

当你使用节点时，可以用 [setInterval](https://nodejs.org/api/timers.html#timers_setinterval_callback_delay_arg) 替代上面那个方法 `window.requestAnimationFrame`:

```js
setInterval(function() {
    Engine.update(engine, 1000 / 60);
}, 1000 / 60);
```
## 使用 Matter.Runner

这里包含一个名叫 [Matter.Runner](http://brm.io/matter-js/docs/classes/Runner.html) 的runner。
这个模块一个可选的实用程序，它提供了一个游戏循环，它可以持续为你更新`Matter.Engine`。它适合开发和调试，也适合一些简单的游戏。

#### 文档

文档请查阅 [Matter.Runner](http://brm.io/matter-js/docs/classes/Runner.html).

#### 用法

`Engine.run` 的简单用法:

```js
var engine = Engine.create();
Engine.run(engine);
```

或者你直接创建一个runner:

```js
var runner = Runner.create();
Runner.run(runner, engine);
```

#### 选项

`Matter.Runner.create` 可以设置更多的参数:

```js
var runner = Runner.create({
    delta: 1000 / 60,
    isFixed: false,
    enabled: true
});
```
