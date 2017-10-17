# HTML player installation
## Static URL
Or you can use the script file from here:
https://cdnjs.com/libraries/bodymovin

## From Extension
Or get it directly from the AE plugin clicking on Get Player

## Bodymovin light
The extension includes `bodymovin_light.js` which will play animations exported as svgs.

## NPM
```bash
npm install bodymovin
```

## Bower
```bash
bower install bodymovin
```

### HTML
- get the bodymovin.js file from the build/player/ folder for the latest build
- include the .js file on your html (remember to gzip it for production)
```html
<script src="js/bodymovin.js" type="text/javascript"></script>
```

You can call bodymovin.loadAnimation() to start an animation.
It takes an object as a unique param with:
- animationData: an Object with the exported animation data.
- path: the relative path to the animation object. (animationData and path are mutually exclusive)
- loop: true / false / number
- autoplay: true / false it will start playing as soon as it is ready
- name: animation name for future reference
- renderer: 'svg' / 'canvas' / 'html' to set the renderer
- container: the dom element on which to render the animation


It returns the animation instance you can control with play, pause, setSpeed, etc.

```js
bodymovin.loadAnimation({
  container: element, // the dom element that will contain the animation
  renderer: 'svg',
  loop: true,
  autoplay: true,
  path: 'data.json' // the path to the animation json
});
```

## Usage
animation instances have these main methods:
**anim.play()** <br/>
**anim.stop()** <br/>
**anim.pause()** <br/>
**anim.setLocationHref(locationHref)** -- one param usually pass as `location.href`. Its useful when you experience mask issue in safari where your url does not have `#` symbol. <br/>
**anim.setSpeed(speed)** -- one param speed (1 is normal speed) <br/>
**anim.goToAndStop(value, isFrame)** first param is a numeric value. second param is a boolean that defines time or frames for first param <br/>
**anim.goToAndPlay(value, isFrame)** first param is a numeric value. second param is a boolean that defines time or frames for first param <br/>
**anim.setDirection(direction)** -- one param direction (1 is normal direction.) <br/>
**anim.playSegments(segments, forceFlag)** -- first param is a single array or multiple arrays of two values each(fromFrame,toFrame), second param is a boolean for forcing the new segment right away<br/>
**anim.setSubframe(flag)** -- If false, it will respect the original AE fps. If true, it will update as much as possible. (true by default)<br/>
**anim.destroy()**<br/>

bodymovin has 8 main methods:
**bodymovin.play()** -- with 1 optional parameter **name** to target a specific animation <br/>
**bodymovin.stop()** -- with 1 optional parameter **name** to target a specific animation <br/>
**bodymovin.setSpeed()** -- first param speed (1 is normal speed) -- with 1 optional parameter **name** to target a specific animation <br/>
**bodymovin.setDirection()** -- first param direction (1 is normal direction.) -- with 1 optional parameter **name** to target a specific animation <br/>
**bodymovin.searchAnimations()** -- looks for elements with class "bodymovin" <br/>
**bodymovin.loadAnimation()** -- Explained above. returns an animation instance to control individually. <br/>
**bodymovin.destroy()** -- To destroy and release resources. The DOM element will be emptied.<br />
**bodymovin.registerAnimation()** -- you can register an element directly with registerAnimation. It must have the "data-animation-path" attribute pointing at the data.json url<br />
**bodymovin.setQuality()** -- default 'high', set 'high','medium','low', or a number > 1 to improve player performance. In some animations as low as 2 won't show any difference.<br />

## Events
- onComplete
- onLoopComplete
- onEnterFrame
- onSegmentStart

you can also use addEventListener with the following events:
- complete
- loopComplete
- enterFrame
- segmentStart
- config_ready (when initial config is done)
- data_ready (when all parts of the animation have been loaded)
- DOMLoaded (when elements have been added to the DOM)
- destroy

#### Other loading options
- if you want to use an existing canvas to draw, you can pass an extra object: 'renderer' with the following configuration:
```js
bodymovin.loadAnimation({
  container: element, // the dom element
  renderer: 'svg',
  loop: true,
  autoplay: true,
  animationData: animationData, // the animation data
  rendererSettings: {
    context: canvasContext, // the canvas context
    scaleMode: 'noScale',
    clearCanvas: false,
    progressiveLoad: false, // Boolean, only svg renderer, loads dom elements when needed. Might speed up initialization for large number of elements.
    hideOnTransparent: true //Boolean, only svg renderer, hides elements when opacity reaches 0 (defaults to true)
  }
});
```
Doing this you will have to handle the canvas clearing after each frame
<br/>
Another way to load animations is adding specific attributes to a dom element.
You have to include a div and set it's class to bodymovin.
If you do it before page load, it will automatically search for all tags with the class "bodymovin".
Or you can call bodymovin.searchAnimations() after page load and it will search all elements with the class "bodymovin".
<br/>
- add the data.json to a folder relative to the html
- create a div that will contain the animation.
<br/>
 **Required**
 <br/>
 . a class called "bodymovin"
 . a "data-animation-path" attribute with relative path to the data.json
 <br/>
**Optional**
<br/>
 . a "data-anim-loop" attribute
 . a "data-name" attribute to specify a name to target play controls specifically
 <br/>
 **Example**
 <br/>
```html
 <div style="width:1067px;height:600px" class="bodymovin" data-animation-path="animation/" data-anim-loop="true" data-name="ninja"></div>
```
<br/>