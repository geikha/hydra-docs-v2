---
title: transforms
date: 2025-06-06
weight: 32
---

# transforms

Transforms are the functions that we can call over `sources` using dots. They are "effects" that we can apply to visuals and chain together. There are different type of transforms, the most basic ones being **color** and **geometry**.

|**Color** transform|**Geometry** transform|
|--------------------|-----------------------|
|Changes the content of pixels (its colors and their transparency)|Changes the placement of pixels|

## Some color transforms

Color transforms change the colors (the RGBA values) for all pixels in a texture.

### **invert**

Inverts the color of any visual. The `amount` argument allows us to bypass the effect by inputting 0, or exaggerate the effect using numbers higher than 1.

`invert( amount = 1 )`
{.center}

```hydra
// default
solid(1,1,1).invert([0,1]).out(o0)
```

### **color**

Allows you to modify (multiply) the color channels of a visual. Using negative values will invert only that channel (it doesn't output negative values).

`color( r = 1, g = 1, b = 1, a = 1 )`
{.center}

```hydra
// default
osc().color(1,0,[3,-1]).out(o0)
```

### **brightness**

Changes the brightness of the image by some `amount`.

`brightness( amount = 0.4 )`
{.center}

```hydra
// default
osc(20,0,2)
  .brightness( () => Math.sin(time) )
  .out(o0)
```

### **luma**

Applies a luma key. This means it will remove any parts of the image with a [luma](https://en.wikipedia.org/wiki/Luma_(video)) (brightness) lower than the `threshold` value.

`luma( threshold = 0.5, tolerance = 0.1 )`
{.center}

```hydra
// default
osc(10,.1,1).luma(0.5,0.1).out(o0)
```


### **hue**
Rotates the [hue](https://en.wikipedia.org/wiki/Hue) of colors in a visual. Inputting `1` (or any integer) will have unnoticeable effects, since each unit represents a whole turn in the color wheel. For example `0.5` will output complementary colors.

`hue( hue = 0.4 )`
{.center}

```hydra
// default
osc(30,0.1,1).hue(() => Math.sin(time)).out(o0)
```

### Much more

Check the color section in the [interactive function reference](https://hydra.ojack.xyz/api) for a glossary of all the color functions.

---

## Some geometry transforms

Geometry transforms will affect how the texture is displayed on screen, allowing us to do geometric changes such as rotating, scaling, positioning, and much more. They don't affect the colors of the visuals but rather where they appear.

### **rotate**

Rotates a texture by some `amount` of [radians](https://en.wikipedia.org/wiki/Radian). Most people just eyeball rotations on Hydra, but you can be precise by using `Math.PI`. You can also set a `speed` for automatic rotation.

`rotate( angle = 10, speed )`
{.center}

```hydra
// constant rotation
osc(50).rotate(() => time).out(o0)
```

### **scale**

With `scale` you can zoom in or out of your visuals by some `amount`. You can also scale only horizontally or vertically. Inputting 1 won't do anything, inputting `0.5` will be half the size, `2` double the size, and so on. The `offset` arguments let you place the center of scaling other than the center of the screen, in case you need that.

`scale( amount = 1.5, xMult = 1, yMult = 1, offsetX = 0.5, offsetY = 0.5 )`
{.center}

```hydra
shape().scale(1.5,1,1).out(o0)
```

### **scroll**

Let's you move textures left and right, up or down.

`scroll( scrollX = 0.5, scrollY = 0.5, speedX, speedY )`
{.center}

```hydra
shape(3).scroll(0.2,0.3).out(o0)
```

### **kaleid**

Kaleidoscope effect with `nSides` repetition.

`kaleid( nSides = 4 )`
{.center}

```hydra
osc(25,-0.1,0.5).kaleid([4,8,50]).out(o0)
```

### Much more

Check the geometry section in the [interactive function reference](https://hydra.ojack.xyz/api) for a glossary of all the geometry functions.
