---
title: "mouse"
draft: false
weight: 60
---

# mouse

You can have your visuals react to the position of your mouse (or finger, in touch devices). Hydra has an object called `mouse` which stores and keeps track of the position of your mouse on the webpage.

#### Important note

All of the examples using mouse position to move stuff on the canvas won't work well here, since the canvas doesn't occupy the full size of the screen. Take this into account when using `mouse`, that the positions are relative to the full webpage and not the canvas. This also means that as you scroll down this guide the `y` value will get higher and higher. Open the following examples in the editor pressing the rocket emoji.

---

## **mouse.x** & **mouse.y**

```hydra
gradient()
	.hue(()=>mouse.x/3000)
	.scale(1,1,()=>mouse.y/1000)
	.out()
```

You can refer to the pixel position of your mouse by calling `mouse.x` and `mouse.y`, each one corresponding to the horizontal and vertical coordinates respectively. When we say 'pixel position', this means that the values you'll find stored in both x and y are represented in pixels. So for `mouse.x`, this means the amount of pixels from the left edge of your window to the position of your mouse. For `mouse.y`, this means the amount of pixels between the top end of your screen and the position of your mouse.

Many times it will be most useful to use values relative to the size of the screen. And also to have values that exist between ranges more reasonable to the hydra functions you're using. For example [-0.5; 0.5] for scrollX and scrollY, [0; 2pi] for rotation, or [0; 1] for general purposes.

---

### Getting values from 0 to 1

On Hydra, most values used are pretty small. So it will be way more useful to have the position of the mouse as values from 0 and 1:

```hydra
x = () => mouse.x/width // 0→1
y = () => mouse.y/height // 0→1
osc()
        .scale(()=>1+x()*2)
        .modulate(noise(4),()=>y()/4)
        .out()
```

### Getting values from 0 to 2pi

It can be useful to have values from 0 to (two times pi), since 2pi is a full rotatio in radians, equivalent to 360 degrees:

```hydra
x = () => mouse.x/width // 0→1
y = () => mouse.y/height // 0→1
osc()
        .rotate(()=>x()*2*Math.PI) // 0->2pi
        .out()
```

---

## Follow your mouse

On Hydra, things are placed between 0.5 and -0.5 (left to right, top to bottom). In order for anything to follow your mouse, you'll need to get the position of your mouse between that range.

### Getting values from 0 to ±0.5 from the center

```hydra
x = () => (-mouse.x/width)+.5 // 0.5→-0.5
y = () => (-mouse.y/height)+.5 // 0.5→-0.5
solid(255)
    .diff(
        shape(4,.1)
        .scroll(()=>x(),()=>y())
    )
    .out()
```

---

## More

If you want to explore the mouse further, you can extend your usage of the mouse to using clicks. Or you can even use your keyboard as an interactive element! Learn more about this on the [clicks & keys guide](../../guides/event-listeners).
