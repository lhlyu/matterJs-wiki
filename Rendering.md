> The library comes packaged with two simple render modules for displaying worlds [Matter.Render](http://brm.io/matter-js-docs/classes/Render.html) and [Matter.RenderPixi](http://brm.io/matter-js-docs/classes/RenderPixi.html).

1. [Using a built in renderer](#using-a-built-in-renderer)
    1. [Matter.Render](#matterrender)
    1. [Matter.RenderPixi](#matterrenderpixi)
1. [Using a custom renderer](#using-a-custom-renderer)

## Using a built in renderer

#### Matter.Render

The default renderer is [Matter.Render](http://brm.io/matter-js-docs/classes/Render.html) which uses a simple HTML5 canvas in immediate mode. It has built in support for both primitive and sprite based rendering of bodies in 2d.

When you use `Engine.create(element)` a `Matter.Render` instance will be created for you and it will automatically insert a canvas into the page at the specified `element`.

Following this calling `Engine.run(engine)` will spawn the built in game loop routine, which will automatically manage updating the engine and calling the renderer at the appropriate times.

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
})
```

#### Matter.RenderPixi

An alternate renderer [Matter.RenderPixi](http://brm.io/matter-js-docs/classes/RenderPixi.html) is provided as an example of using [Pixi.js](http://www.pixijs.com/) to render a world using WebGL and a scene graph. The features available should match those of `Matter.Render` although sometimes there may be implementation differences.

To make use of this module you must pass it to your engine at its creation:

```js
Engine.create({
    render: {
        element: document.body,
        controller: Matter.RenderPixi
    }
})
```

## Using a custom renderer

While the built in renderers are useful for early development, if you need to do any kind of complex rendering effects you will need a custom renderer.

The easiest way to do this is to simply copy [Render.js](https://github.com/liabru/matter-js/blob/master/src/render/Render.js) and customise it, giving it a new module name.

To then use your custom renderer you must pass it to your engine at its creation where `MyRenderer` is the name of your new render module:

```js
Engine.create({
    render: {
        element: document.body,
        controller: MyRenderer
    }
})
```