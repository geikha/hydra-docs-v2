---
title: "arrow functions"
date: 2025-06-06
draft: false
weight: 51
---

# arrow functions

## **()=> time**

You can use dynamic inputs in your sketches by passing functions as arguments. When Hydra takes a function as an argument, what it will do is evaluate it every time it renders a frame. The return of the function will be used as the value for that parameter during that frame render. So you can use a function to simply keep track of a value that you know will change over time, for example, mouse position (which we'll see later).

```hydra
voronoi(5,.1,()=>Math.sin(time*4))
	.out()
```

The `time` variable seen there is a variable pre-declared by Hydra, that stores how much time passed since Hydra started in seconds.

Functions used in Hydra don't need to be arrow functions, any no-argument function will do! Make sure your function is returning a Number to avoid errors.

## **time**

When you use functions that can take numerical arguments, `time` will allow you to have their values evolve through... time. If you multiply time by some value it's as if time goes faster, while dividing while act as making time go slower. For example `Math.sin(time*4)` will go 4 times faster than `Math.sin(time)`.

Those users more familiar with mathematics might see this as:

* `y(t) = t` : `()=>time`
* `y(t) = A sin(f t + ph)` : `()=>amplitude*Math.sin(freq*time + phase)`

We recommend getting familiar with some of the methods in the JS built-in `Math` object. Learn more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

## **speed**

You can either slow down or fasten the rate at with `time` increases via changing the `speed` variable:

```javascript
speed = 1  // default
speed = 2  // twice as fast
speed = .5 // half as fast
speed = 0  // freezed
```
