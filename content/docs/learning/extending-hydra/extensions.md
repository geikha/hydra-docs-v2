---
title: extensions
weight: 70
---

# extensions

The Hydra community has put together many `hydra-synth` extensions which you can load into your sketches. This can be done using the `loadScript` function. Some of them add more sources, transforms, others have more intricate functionalities. There's also a [repository of extensions](https://github.com/hydra-synth/hydra-extensions) which you can load easily inside the editor by pressing the puzzle piece icon. Here we present some of the most used extensions:

---

## hydra-midi

##### by Arnoson

`hydra-midi` is one of the most used extensions for Hydra. It allows you to easily incorporate MIDI devices into your patches. These can be MIDI interfaces, controllers, virtual cables, etc. It can use both notes and CC values. For more information visit the [hydra-midi repository](https://github.com/arnoson/hydra-midi).

```javascript
await loadScript('https://cdn.jsdelivr.net/npm/hydra-midi@latest/dist/index.js')

await midi.start().show()

osc(30,.01).invert(note('C4')).out()
osc().rotate(cc(45).range(0,Math.PI*2)).out(o1)
```

---

## hyper-hydra

##### by Geikha

`hyper-hydra` is a pack of diverse "hyper-usable hydra extensions". The functionalities are quite varied, here's a list of the most important ones. Learn more about it in [its repository](https://github.com/geikha/hyper-hydra).

|Name|Description|
|---|---|
|[hydra-arithmetics](https://github.com/geikha/hyper-hydra#hydra-arithmetics)|All the functions you need to make complex visual arithmetics.|
|[hydra-arrays](https://github.com/geikha/hyper-hydra#hydra-arrays)|Extends the functionality of arrays in Hydra, letting you operate between different arrays and generate new ones.|
|[hydra-blend](https://github.com/geikha/hyper-hydra#hydra-blend)|Adds most blending modes you know from raster image softwares. Ideal for compositing.|
|[hydra-colorspaces](https://github.com/geikha/hyper-hydra#hydra-colorspaces)|Work with color in different colorspaces such as CMYK, HSV, YUV, etc.|
|[hydra-convolutions](https://github.com/geikha/hyper-hydra#hydra-convolutions)|Adds many convolutions such as sharpen, blur, sobel and more, as well as functions for creating your own.|
|[hydra-gif](https://github.com/geikha/hyper-hydra#hydra-gif)|Load GIFs inside Hydra!|
|[hydra-glsl](https://github.com/geikha/hyper-hydra#hydra-glsl)|Write GLSL code directly between patches|
|[hydra-outputs](https://github.com/geikha/hyper-hydra#hydra-outputs)|Change the properties of Hydra's outputs' framebuffers such as interpolation.|
|[hydra-src](https://github.com/geikha/hyper-hydra#hydra-src)|Adds alternative `src` functions which take into account the source's aspect ratio or absolute size. Also adds `rotateRel` to rotate without distortion.|
|[hydra-text](https://github.com/geikha/hyper-hydra#hydra-text)|Adds a simple text generator to Hydra!|


---

## Extra shaders for Hydra

##### by Thomas Jourdan

Thomas' own extension pack is widely used since it adds many useful functions to Hydra. You can learn more about it on [its repository](https://gitlab.com/metagrowing/extra-shaders-for-hydra).

|Name|Description|
|---|---|
|[Op-art patterns](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/pattern/README.md)|New source functions inspired by op-art patterns.|
|[Soft patterns](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/softpattern/README.md)|New source function which generate patterns with smooth transitions.|
|[Color filters](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/color/README.md)|Additional filters for mixing or manipulating colors.|
|[Noise](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/color/README.md)|Adds more types of noise to Hydra, including static, multi-octave perlin noise, domain-warping noise, and more.|
|[Screen space filters](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/screenspace/README.md)|Used only in the last stage of the filter pipeline, they add typical renderpass effects such as blurring and dithering. There is also the related `hydra-convolutions` extension for a different code approach.|
|[Basic periodic functions](https://gitlab.com/metagrowing/extra-shaders-for-hydra/-/blob/main/gallery/wave/README.md)|Adds more simple periodic patterns (oscillators), such as a triangle wave, a square wave and a sawtooth wave.|
