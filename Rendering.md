> Examples of how to render a Matter.js engine

1. [Renderer example](#renderer-example)
1. [Using Matter.Render](#using-matter-render)
    1. [Documentation](#documentation)
    1. [Usage](#usage)
    1. [Options](#options)

## Renderer example

A basic example for rendering the bodies in a world as wireframes, given a previously created `Matter.Engine` (see [Getting started](https://github.com/liabru/matter-js/wiki/Getting-started)):

```js
var canvas = document.createElement('canvas'),
    context = canvas.getContext('2d');

canvas.width = 800;
canvas.height = 600;

document.body.appendChild(canvas);

(function render() {
    var bodies = Composite.allBodies(engine.world);

    window.requestAnimationFrame(render);

    context.fillStyle = '#fff';
    context.fillRect(0, 0, canvas.width, canvas.height);

    context.beginPath();

    for (var i = 0; i < bodies.length; i += 1) {
        var vertices = bodies[i].vertices;

        context.moveTo(vertices[0].x, vertices[0].y);

        for (var j = 1; j < vertices.length; j += 1) {
            context.lineTo(vertices[j].x, vertices[j].y);
        }

        context.lineTo(vertices[0].x, vertices[0].y);
    }

    context.lineWidth = 1;
    context.strokeStyle = '#999';
    context.stroke();
})();
```

A good place to start with your own rendering is to take a look at the source of [Matter.Render](https://github.com/liabru/matter-js/blob/master/src/render/Render.js) (you can also simply copy it and customise as you require).

## Using Matter.Render

There is an included debug renderer called [Matter.Render](http://brm.io/matter-js/docs/classes/Render.html).
This module is an optional canvas based renderer for visualising instances of `Matter.Engine`. It is mostly intended for development and debugging purposes, but may also be suitable starting point for simple games. It includes a number of drawing options including wireframe, vector with support for sprites and viewports.

By default `Matter.Render` will only show bodies as wireframes (outlines). This is useful for testing and debugging, but to enable full solid rendering (and sprites if you are using them) you must set `render.options.wireframes = false`.

#### Documentation

See the documentation for [Matter.Render](http://brm.io/matter-js/docs/classes/Render.html).

#### Usage

```js
var engine = Engine.create();

// ... add some bodies to the world

var render = Render.create({
    element: document.body,
    engine: engine,
    options: options
});

Engine.run(engine);
Render.run(render);
```

Where:

- `element` is a container element to insert the canvas into
- `engine` is a `Matter.Engine` instance
- `options` is an object specifying render settings, see below (optional)

You can also just pass a `canvas` instead of a container `element` if you wish to create the canvas yourself.

#### Options

A number of options may be passed to `Matter.Render.create`:

```js
var render = Render.create({
    element: document.body,
    engine: engine,
    options: {
        width: 800,
        height: 600,
        pixelRatio: 1,
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
        showSeparations: false,
        showAxes: false,
        showPositions: false,
        showAngleIndicator: false,
        showIds: false,
        showShadows: false,
        showVertexNumbers: false,
        showConvexHulls: false,
        showInternalEdges: false,
        showMousePosition: false
    }
});
```