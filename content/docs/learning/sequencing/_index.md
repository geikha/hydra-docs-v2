---
title: "sequencing"
draft: false
weight: 5
bookCollapseSection: true
---

# sequencing

Hydra allows you to use dynamic inputs. So, anywhere you would write a number on a function, be it a source or a transform, you can make that value change other time. These changes through time can be one of both:

## [arrays](./arrays)

Arrays are lists of numbers. Passing them as arguments in functions will create strict periodical sequences. Hydra will automatically go from one value to the next and wrap around when finished over and over. Read more about them on the [array page](./arrays).

```hydra
osc().rotate([-1,1]).out()
```

## [arrow functions](./arrow-functions)

You can create functions of time and pass them as arguments. When you pass a function as an argument, Hydra will evaluate it each frame and use its return as the value during the next frame. Read more about this on the [arrow functions page](./arrow-functions).
