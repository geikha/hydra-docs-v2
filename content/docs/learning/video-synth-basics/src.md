---
title: sources
date: 2025-06-06
weight: 30
---
# sources

Sources are functions that generate video signals. These are the fundamental pieces of our visuals, we connect them to transforms and mix them with other functions in order to create our visuals. Using a musical analogy: sources are like instruments (guitars, oscillators, keyboards) and we connect them to transforms (effects) in order to create cool stuff.

## Some source functions

### **osc**
A visual oscillator, goes from black to white smoothly. The `frequency` defines how many oscillations are made along the screen. The `sync` how fast they move. The `offset` offsets each color channel (Red, Green and Blue).

`osc( frequency = 60, sync = 0.1, offset )`
{.center}

```hydra
// changing frequency with a list
osc([2,30,60,90],0.1,0.2).out(o0)
```

### **solid**
Outputs a solid color that covers the whole screen. You can set the value for each color channel (Red, Green, Blue and Alpha).

`solid( r, g, b, a = 1 )`
{.center}

```hydra
// red, green and magenta
solid(1,0,0,1).out(o0)
solid(0,1,0,1).out(o1)
solid(1,0,1,1).out(o2)

render()
```

### **noise**
Generates 2D slices of [Perlin noise](https://en.wikipedia.org/wiki/Perlin_noise). You can set the both the `scale` and how fast the noise changes using the first two arguments.

`noise( scale = 10, offset = 0.1 )`
{.center}

```hydra
// default
noise(10,0.1).out(o0)
```

##### Please note that the `noise` generator outputs both positive and negative values.

### **voronoi**
Generates [Voronoi shapes](https://en.wikipedia.org/wiki/Voronoi_diagram). You can set the scale, the speed at which they change and their blending.

`voronoi( scale = 5, speed = 0.3, blending = 0.3 )`
{.center}

```hydra
// default
voronoi(5,0.3,0.3).out(o0)
```

### **shape**
Generates simple polygons, You can set how many sides they have, the size radius and how smooth the edges are.

`shape( sides = 3, radius = 0.3, smoothing = 0.01 )`
{.center}

```hydra
// triangle
shape(3,0.5,0.001).out(o0)
```

### **gradient**

Outputs a simple gradient sequence. You can set the speed at which the blue channel oscillates.

`gradient( speed )`
{.center}

```hydra
// gradient sequence at speeds of 1, 2 & 4
gradient([1,2,4]).out(o0)
```

## A special src

### **src**
Accepts a texture (framebuffer) and shows it on screen, such as external sources (s0, s1, s2, s3) or a Hydra output (o0, o1, o2, o3). This allows us to include external images in our patch or to connect patches with each other.

`src( tex )`
{.center}

```hydra
// feedback
src(o0).modulate(noise(3),0.005).blend(shape(4),0.01).out(o0)
```

## Outputting

Following the musical analogy from above, all instruments (sources), regardless of how many effects (transforms) we add need to be outputted to some speakers (output). The special Hydra function that allows us to output our visuals to a framebuffer is called `out`.

### **out**

`out( output = o0 )`
{.center}

```hydra
osc().out(o1)
src(o1).out(o0)
```

---

See the [interactive function reference](https://hydra.ojack.xyz/api) for a glossary of all the functions.
