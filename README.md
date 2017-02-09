# constellation-canvas
Draws cute animated canvas constellations.

[See it in action here!](https://lawwrr.github.io/constellation/)

<p align="center">
  <img src="http://i.imgur.com/gLCMGoi.png">
</p>

## Usage (es6/webpack)
Grab the code from here or npm

    npm install constellation-canvas --save

    #or#

    git checkout git@github.com:lawwrr/constellation.git
    npm install

Then just import it and feed it some parameters. It will create a random svg if it can't find any.

    const Constellation = require('constellation-canvas');
         /*↖️ hehe it's a double const*/

    let constellation = Constellation({
        size:[500,800],
        canvas: document.querySelector('canvas'),
        starCount: 30,
        lineCount: 60,
        style: {
            starSize: 4,
            starPadding: 5
            lineSize: 2
        }

    });


### Parameters

All of them except `canvas` are optional

| Name | Description |
| --- | --- |
| **size** (array[x,y]) | Size of the canvas |
| **padding** (array[x,y]) | space between the canvas edges and the stars, can be negative  |
| **canvas** (DOM element) | Canvas element to draw in |
| **starCount** | Total number of stars |
| **lineCount** | Total number of lines drawn between stars |
| **speed** (object) | Object with speed options for the stars. |
| **speed.active** | Speed when the mouse is moving the stars. |
| **speed.passive** | Speed when the stars are jiggling. |
| **style** (object) | Object with style options |
| **style.starSize** | Size of the stars |
| **style.starColor** | Color of the stars |
| **style.starPadding** | Space between stars and lines |
| **style.lineColor** | Color of the lines |
| **style.lineSize** | Size of the lines |


### Drawing things yourself
For further customization you can also pass an `onDraw` parameter with a number of callbacks. These will allow you to take over the drawing process of the canvas.

    let constellation = Constellation({
        size:[500,800],
        canvas: document.querySelector('canvas'),
        onDraw: {
            afterStar: (ctx,node,style) => {
                ctx.beginPath();
                ctx.arc(
                    node.pos[0], node.pos[1], style.starSize,0, 2 * Math.PI
                );
                ctx.globalCompositeOperation = 'destination-over';
                ctx.fillStyle = 'rgba(0,0,0,0)';
                ctx.shadowColor = '#999';
                ctx.shadowBlur = 20;
                ctx.shadowOffsetX = 0;
                ctx.shadowOffsetY = 15;
                ctx.closePath();
                ctx.fill();
            }
        }
    });

YOu can see how these plug together at `src/class/Canvas.js` but here's a quick chart

| Callback | Description |
| --- | --- |
| **star**(ctx,style,star) | overrides star drawing. `star` contains the coordinates for the star to be drawn |
| **afterStar**(ctx,style,star) | takes place after the default star drawing. `star` contains the coordinates for the star that was drawn |
| **line**(ctx,style,line) | overrides line drawing. `line` contains the coordinates for the line to be drawn |
| **afterLine**(ctx,style,line) | takes place after the default line drawing. `line` contains the coordinates for the line that was drawn |
| **afterFrame**(ctx,style,objects) | takes place after drawing a full frame. `objects` contains all coordinates for stars & lines |



### Advanced

Available callbacks are `star`,`afterStar`,`line`,`afterLine`,`afterFrame`.

`star` & `line` will completely override the default drawing stage while `afterStar`,`afterLine` & `afterFrame` take place after their drawing is complete

There are some extra advanced properties too! `fuzziness` for controlling how reactive to the mouse stars are and `scale`, for drawing the canvas at a different resolution (it's @2x by default). Check out the code (i mean it's like 2? files total) to see how they work.

ALSO!! should you ever need it, `Constellation` will return a promise containing `$constellation`, the canvas DOM object after everything there has been done.

    let constellation = Constellation({
        /*blah*/
    });

    constellationInstance.then(function(data){
        console.log(data.$constellation);
    })


## Usage (legacy)
Consider migrating your codebase

Otherwise, grab the [latest release](https://github.com/lawwrr/constellation/releases) and drop it in as a script tag. `window.constellation` will appear.
