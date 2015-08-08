> The library comes packaged with a render module, or simply create your own.

1. [Using a built in renderer](#using-a-built-in-renderer)
    1. [Matter.Render](#matterrender)
    1. [Matter.RenderPixi](#matterrenderpixi)
    1. [Render options](#render-options)
1. [Using a custom renderer](#using-a-custom-renderer)
1. [Using a custom game loop](#using-a-custom-game-loop)

## Using a built in renderer

#### Matter.Render

The default renderer is [Matter.Render](http://brm.io/matter-js-docs/classes/Render.html) which uses a simple HTML5 canvas in immediate mode. It has built in support for both primitive and sprite based rendering of bodies in 2d.

When you use `Engine.create(element)` a `Matter.Render` instance will be created for you and it will automatically insert a canvas into the page at the specified `element`.

Following this calling `Engine.run(engine)` will spawn the built in game loop routine, which will automatically manage updating the engine and calling the renderer at the appropriate times.

By default this renderer will only show bodies as wireframes (outlines). This is useful for testing and debugging, but to enable full solid rendering (and sprites if you are using them) you must set `render.options.wireframes = false`.

#### Matter.RenderPixi

An alternate renderer [Matter.RenderPixi](http://brm.io/matter-js-docs/classes/RenderPixi.html) is provided as an example of using [Pixi.js](http://www.pixijs.com/) to render a world using WebGL and a scene graph. The features available should match those of `Matter.Render` although sometimes there may be implementation differences.

To make use of this module you must pass it to your engine at its creation:

```js
Engine.create({
    render: {
        element: document.body,
        controller: Matter.RenderPixi
    }
});
```

Note that you must include [Pixi.js](http://www.pixijs.com/) in your page to use `Matter.RenderPixi`.

#### Render options

A number of options may be passed to the renderer:

```js
Engine.create({
    render: {
        options: {
            width: 800,
            height: 600,
            background: '#fafafa',
            wireframeBackground: '#222',
            hasBounds: false,
            enabled: true,
            wireframes: true,
            showSleeping: true,
            showDebug: false,
            showBroadphase: false,
            showBounds: false,
            showVelocity: false,
            showCollisions: false,
            showAxes: false,
            showPositions: false,
            showAngleIndicator: false,
            showIds: false,
            showShadows: false
        }
    }
});
```

## Using a custom renderer

While the built in renderers are useful for early development, if you need to do any kind of complex rendering effects you will need a custom renderer.

Use the latest [edge build](https://github.com/liabru/matter-js/blob/master/build/matter.js) of Matter.js if you need this, as rendering has since been better decoupled.

Currently it is required that your renderer module (e.g. `MyRenderer`) at least defines the following:

- a `create` function, that returns an object that at least includes the property `controller = MyRenderer`
- a `world` function, which is to be called on every step by the engine and contains your rendering code

```js
var MyRenderer = {
	create: function() {
		return { controller: MyRenderer };
	},

	world: function(engine) {
		// your code here to render engine.world
	}
};
```

To then use your custom renderer you must pass it to your engine at its creation where `MyRenderer` is the name of your new render module:

```js
Engine.create({
    render: {
        controller: MyRenderer
    }
});
```

Take a look at [Render.js](https://github.com/liabru/matter-js/blob/master/src/render/Render.js) to see how the built in renderer works. It may be helpful to use this module as the basis for your own.

(this is to be made more straight forward in future versions!)

## Using a custom game loop

The engine includes a game loop utility, [Engine.run](https://github.com/liabru/matter-js/blob/master/src/core/Runner.js#L35), to get you started straight away. It automatically handles updating the engine, calling key events and calling the renderer at the correct times.

But if you already have a game loop you wish to use, it is very easy to tick the physics engine when ever you need to, like this:

```js
Engine.update(engine, delta, correction);
```

Where:

- `engine` is your `Matter.Engine` to update the state of
- `delta` is the timestep in ms (e.g. 60FPS = 1000/60 = 16.666)
- `correction` is the timestep correction factor as described in [Time Corrected Verlet](http://lonesock.net/article/verlet.html) (a value of `1` means no correction)

Note that you may also need to implement [engine events](http://brm.io/matter-js-docs/classes/Engine.html#events), e.g. `beforeTick`, `tick` and `afterTick` etc for all features to work correctly. See the code for [Matter.Runner](https://github.com/liabru/matter-js/blob/master/src/core/Runner.js#L57-L112) on when to fire them.