> A simple guide to getting started with Matter.js

1. [Install](#install)
1. [Usage examples](#usage-example)
1. [Running and Rendering](#running-and-rendering)
1. [Documentation](#documentation)

### Install

Download the [edge build (master)](https://github.com/liabru/matter-js/blob/master/build/matter.js) or get a [stable release](https://github.com/liabru/matter-js/releases) and include the script in your web page:

```html
<script src="matter.js"></script>
```

For the latest features try the [edge version (master)](https://raw.github.com/liabru/matter-js/master/build/matter.js), but it may not be fully stable.

You can also install using the package managers [Bower](http://bower.io/search/?q=matter-js) and [NPM](https://www.npmjs.org/package/matter-js).

    bower install matter-js
    npm install matter-js

### Usage example

The following is a minimal example using the built in renderer and runner to get you started:

```javascript
// module aliases
var Engine = Matter.Engine,
    Render = Matter.Render,
    World = Matter.World,
    Bodies = Matter.Bodies;

// create an engine
var engine = Engine.create();

// create a renderer
var render = Render.create({
    element: document.body,
    engine: engine
});

// create two boxes and a ground
var boxA = Bodies.rectangle(400, 200, 80, 80);
var boxB = Bodies.rectangle(450, 50, 80, 80);
var ground = Bodies.rectangle(400, 610, 810, 60, { isStatic: true });

// add all of the bodies to the world
World.add(engine.world, [boxA, boxB, ground]);

// run the engine
Engine.run(engine);

// run the renderer
Render.run(render);
```

Include the above script into a page that has Matter.js installed and then open the page in your browser. Make sure the script is at the bottom of the page (or called on the window load event, or after DOM is ready).

Hopefully you will see two rectangle bodies fall and then hit each other as they land on the ground. 
If you don't, check the browser console to see if there are any errors.

Check out the [demo page](http://brm.io/matter-js-demo/) for more examples and then refer to [Demo.js](https://github.com/liabru/matter-js/blob/master/demo/js/Demo.js) to see how they work. Some of the demos are also available on [codepen](http://codepen.io/collection/Fuagy/), where you can easily experiment with them (but they may not always be completely up to date).

### Running and Rendering

The above example uses the built in renderer and runner, but these are both optional and it's easy to render in your own way and use your own game loop.
See the [Rendering](https://github.com/liabru/matter-js/wiki/Rendering) and [Running](https://github.com/liabru/matter-js/wiki/Running) wiki pages for information.

### Documentation

See the [Matter.js Documentation](http://brm.io/matter-js/docs/) for detailed information on the API.