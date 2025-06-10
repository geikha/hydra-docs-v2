---
title: modulation
date: 2025-06-06
weight: 34
---
# modulation

While ***blend*** functions combine the colors from two visual sources, ***modulate*** functions use the colors from one source to affect the ***geometry*** of the second source. This creates a sort of warping or distorting effect. An analogy in the real world would be looking through a texture glass window.
`modulate()` does not change color or luminosity but distorts one visual source using another visual source. Note how modulation is to geometry what blending is to color.

```hydra
s0.initCam()

src(s0).out(o0) // webcam to o0
osc(10).out(o1) // oscillator to o1

src(o0).modulate(o1).out(o2) // o1 distorts o0
// lighter areas from o1 distort o2,
// moving it towards the upper left

render()
```

All ***geometry*** transformations have corresponding ***modulate*** functions that allow you to use one source to warp another source. For example, `.modulateRotate()` is similar to `.rotate()`, but it allows you to apply different amounts of rotation to different parts of the visual source. See [the function reference](https://hydra.ojack.xyz/api/) for more examples.

---

## Some modulation transforms

### **modulate**

Related to `scroll`, it will warp the image moving it upwards and leftwards in modulator's bright areas. You can also set the `amount` of warping. Negative values will have move towards the opposite direction.

`modulate( texture, amount = 0.1 ) `
{.center}

```hydra
osc().modulate(noise(1),0.4).out()
```

### **modulateRotate**

Related to `rotate`, it will rotate the image more or less according to the modulator. You can set the amount of warping and a rotation offset.

`modulateRotate( texture, multiple = 1, offset )`
{.center}

```hydra
osc().modulateRotate(osc(3).rotate(),1).out()
```

### **modulateScrollX**

Related to `scrollX`, scrolls the image horizontally according to the modulator. You can set the amount of warping and an independent scrolling speed.

`modulateScrollX( texture, scrollX = 0.5, speed )`
{.center}

```hydra
voronoi(25,0,0)
  .modulateScrollX(osc(10),0.5)
  .out(o0)
```

### **modulateScrollY**

Related to `scrollY`, scrolls the image vertically according to the modulator. You can set the amount of warping and an independent scrolling speed.

`modulateScrollY( texture, scrollY = 0.5, speed )`
{.center}

```hydra
voronoi(25,0,0)
  .modulateScrollY(osc(10),0.5)
  .out(o0)
```

### **modulateScale**

Related to `scale`, zooms in (or out) of the image according to the modulator. You can set the amount of zooming and a scaling offset. Negative values will down to -1 will scale down.

`modulateScale( texture, multiple = 1, offset = 1 )`
{.center}

```hydra
// cosmic radiation
gradient(5).repeat(50,50).kaleid([3,5,7,9].fast(0.5))
  .modulateScale(osc(4,-0.5,0).kaleid(50).scale(0.5),15,0)
  .out(o0)
```

### **modulateHue**

Modulate hue is special. It will warp the image according to the relation between the channels of the modulator. To be precise, it will warp horizontally according to the difference between green and red, and vertically according to the difference between blue and green. This isn't intuitive but it's an easy way of creating complex motion using colorful modulators.

`modulateHue( texture, amount = 1 )`
{.center}

```hydra
src(o0)
  .modulateHue(src(o0).scale(1.01),1)
  .layer(osc(4,0.5,2).mask(shape(4,0.5,0.001)))
  .out(o0)
```

## Much more

Check the modulation section in the [interactive function reference](https://hydra.ojack.xyz/api) for a glossary of all the modulation functions.

---

## Examples using the camera

```hydra
s0.initCam() //loads a camera

shape().modulate(src(s0)).out() //shape modulated by a camera
```
```hydra
s0.initCam() //loads a camera

src(s0).modulate(shape()).out() //camera modulated by a shape
```

```hydra

noise().out(o1)
shape().out(o3)

src(o1).add(src(o3)).out(o2) //additive light. Color only gets brighter

render()
```

```hydra
osc(10).out(o0)

shape().out(o1)

src(o0).diff(o1).out(o2) // combines different signals by color difference (color negative/inverted/opposite).  

render()
```
```hydra
osc().mult(src(o1)).out() // multiplies the sources together,
shape(5).out(o1)

```
