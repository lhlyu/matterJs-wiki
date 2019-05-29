> Matter.js 引擎如何渲染

1. [渲染示例](#renderer-example)
1. [Matter.Render 用法](#using-matterrender)
    1. [文档](#documentation)
    1. [用法](#usage)
    1. [选项](#options)

## 渲染示例
在原先入门(查看 [入门](https://github.com/liabru/matter-js/wiki/Getting-started))的源码，以body作为场景的渲染示例：

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

开始学习渲染的最好方式就是查看源码[Matter.Render](https://github.com/liabru/matter-js/blob/master/src/render/Render.js)，
你也可以复制它然后修改。


## 使用 Matter.Render

包含一个名为[Matter.Render](http://brm.io/matter-js/docs/classes/Render.html)的调试渲染器。
此模块是一个可选的基于画布的渲染器，用于可视化“matter.engine”的实例。它主要用于开发和调试目的，但也是简单游戏的合适起点。它包括许多绘图选项，包括线框、支持精灵和视区的矢量。
默认情况下，“matter.render”只将实体显示为线框（轮廓）。这对于测试和调试很有用，但要启用完整的实体渲染（如果正在使用精灵），必须设置“render.options.wireframes=false”。

#### 文档

查看文档 [Matter.Render](http://brm.io/matter-js/docs/classes/Render.html).

#### 用法

```js
var engine = Engine.create();

// ... 添加一些 bodies to the 场景（world）

var render = Render.create({
    element: document.body,
    engine: engine,
    options: options
});

Engine.run(engine);
Render.run(render);
```

Where:

- `element` 是一个容器元素被加入到canvas里面
- `engine` is a `Matter.Engine` 实例
- `options` 是渲染器的其他属性选项, 可以查看下面的 (选项)

可以通过一个canvas替代这个容器元素，如果你想自己创建canvas

#### 选项

`Matter.Render.create` 其他选项:

```js
var render = Render.create({
    element: document.body,
    engine: engine,
    options: {
        width: 800,
        height: 600,
        pixelRatio: 1,                       // 像素比率
        background: '#fafafa',
        wireframeBackground: '#222',         // 线框背景
        hasBounds: false,                    // 是否有边界
        enabled: true,                       // 是否启用
        wireframes: true,                   
        showSleeping: true,
        showDebug: false,
        showBroadphase: false,
        showBounds: false,
        showVelocity: false,                 // 展示速度
        showCollisions: false,               // 显示碰撞
        showSeparations: false,              // 显示分离 
        showAxes: false,                     // 显示轴线 
        showPositions: false,                // 显示坐标
        showAngleIndicator: false,           // 显示角度指示器
        showIds: false,                        
        showShadows: false,                  // 显示阴影
        showVertexNumbers: false,            // 显示顶点编号
        showConvexHulls: false,              // 显示凸透镜（凸形图）。。不知道怎么翻译，
        showInternalEdges: false,            // 显示内部边缘
        showMousePosition: false             // 显示鼠标坐标
    }
});
```
