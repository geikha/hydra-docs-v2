---
title: blending
date: 2025-06-06
weight: 33
---

# blending

Blend transforms allow you to combine the colors of two visual sources. There are many ways of operating with them. There are multiple [blend modes](https://en.wikipedia.org/wiki/Blend_modes) in Hydra, similar to the blend modes you might find in raster graphics programs such as Photoshop or GNU-IMP. All blend functions take in a texture which can be a source, a patch or a framebuffer (such as o0, o1, s0, etc).

---

## Some blend transforms

### **blend**

The simple `blend` functions mixes the colors from two sources to create a third source. You can also specify how much of the second texture you mix in. Inputting `0` will output the original texture unaffected, `0.5` (the default) mixes them 50%/50%, and `1` will only show the secondary texture. Going outside this range will create broken mixes, which can be interesting too.

`blend( texture, amount = 0.5 )`
{.center}

```hydra
s0.initCam()

src(s0).out(o0) // render the webcam to output o0

osc(10).out(o1) // render an oscillator to output o1

src(o0).blend(o1).out(o2) // start with o0, mix it with o1, and send it out to o2

render() // render all four outputs at once
```

### **add**

Adds the color channels of two images. You can multiply the second image with the `amount` argument, so you can add more or less from it. Using negative amounts will effectively subtract values instead of adding.

`add( texture, amount = 1 )`
{.center}

```hydra
shape().scale(0.5).add(shape(4),[0,0.25,0.5,0.75,1]).out(o0)
```

### **sub**

Subtracts the color channels of one image to another.

`sub( texture, amount = 1 )`
{.center}

```hydra
osc().sub(osc(6)).out(o0)
```

### **mult**

Multiplies the original image by the incoming one.

`mult( texture, amount = 1 )`
{.center}

```hydra
osc(9,0.1,2).mult(osc(13,0.5,5)).out()
```

### **diff**

Shows the difference between two textures. In other words: `abs(img1 - img2)`.

`diff( texture )`
{.center}

```hydra
osc(9,0.1,1).diff(osc(13,0.5,5)).out(o0)
```

### **layer**

Places a texture over another one using its transparency values.

`layer( texture )`
{.center}

```hydra
solid(1,0,0,1).layer(shape(4).color(0,1,0,()=>Math.sin(time*2))).out(o0)
```

### **mask**

Uses a texture as a mask for the visual. Works in the same way a mask works in Photoshop or other raster graphics software: bright parts will make the image opace, while dark parts will make it more transparent.

`mask( texture )`
{.center}

```hydra
gradient(5).mask(voronoi(),3,0.5).invert([0,1]).out(o0)
```

## Reference

Remember you can check the [interactive function reference](https://hydra.ojack.xyz/api) for a quick glossary of all the functions.
