---
title: settings & state
date: 2025-06-06
draft: false
weight: 35
---
# synth settings
Functions, variables, and settings that affect overall hydra behavior and rendering.

## Global output

### render

Chooses which output to show. If you don't specify one, it will show all 4 outputs.

`render( texture = all )`
{.center}

```hydra
// default
osc(30,0.1,1.5).out(o0)
noise().out(o1)
solid(1).out(o2)
gradient().out(o3)
render()
```

### hush

Empties all outputs.

`hush()`
{.center}

```hydra
// clear the buffers
osc().out(o0)
hush()
```

## Time-related functions

### time

Tells you the amount of time that has passed since the synth started working. Usually called inside arrow functions.

`time`
{.center}

```hydra
shape(2,0.8).kaleid(()=>6+Math.sin(time)*4).out(o0)
```

### speed

Makes time run faster or slower inside Hydra.

`speed = 1`
{.center}

```hydra
// change overall speed
speed = 3
osc(60,0.1,[0,1.5]).out(o0)
```

### bpm

Changes the rate at which Hydra goes through lists.

`bpm = 30`
{.center}

```hydra
// change array speed
bpm = 60
osc(60,0.1,[0,1.5]).out(o0)
```

## Resolution

### setResolution

Sets the working resolution for the synth.

`setResolution( width, height )`
{.center}

```javascript
// make the canvas small (100 pixel x 100 pixel)
setResolution(100,100)
osc().out(o0)
```

### width

Gets the current working width.

`width`
{.center}

```hydra
shape(99).scrollX(() => -mouse.x / width).out(o0)
```

### height

Gets the current working height.

`height`
{.center}

```hydra
shape(99).scrollY(() => -mouse.y / height).out(o0)
```

## Interactivity

### mouse

Gets the position of the mouse on the webpage.

`mouse = { x, y }`
{.center}

```hydra
shape(99).scroll(
  () => -mouse.x / width,
  () => -mouse.y / height)
  .out(o0)
```

See the interactivity area for more information.

## Transform definition

### setFunction

Allows you to declare your own GLSL function for usage within Hydra.

`setFunction( options )`
{.center}

```hydra
// from https://www.shadertoy.com/view/XsfGzn
setFunction({
  name: 'chroma',
  type: 'color',
  inputs: [
    ],
  glsl: `
   float maxrb = max( _c0.r, _c0.b );
   float k = clamp( (_c0.g-maxrb)*5.0, 0.0, 1.0 );
   float dg = _c0.g;
   _c0.g = min( _c0.g, maxrb*0.8 );
   _c0 += vec4(dg - _c0.g);
   return vec4(_c0.rgb, 1.0 - k);
`})
osc(60,0.1,1.5).chroma().out(o0)
```

See the custom GLSL page for more information.

## Frame rendering

### update

`update` is a function that runs every time a frame is rendered. A bit like p5's `draw`, only that this one isn't crutial at all. It can also take in an argument, generally called `dt`, which contains the amount of time that passed between the previous and current frame.

`update( dt )`
{.center}

```hydra
// update is called every frame
b = 0
update = () => b += 0.01 * Math.sin(time)
shape().scrollX(()=>b).out(o0)
```
