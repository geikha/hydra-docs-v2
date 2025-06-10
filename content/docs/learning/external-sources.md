---
title: "external sources"
date: 2025-06-06
draft: false
weight: 4
---

# external sources
<!-- by [geikha](https://github.com/geikha) and [olivia](https://ojack.xyz) -->

Hydra allows you to load external image sources easily using different browser functionalities. Some of the possible internal sources are: static images, videos, webcam, screen-sharing, and other possibilities. You load each of these to the different source variables (s0, s1, s2 and s3), which can be used in the same way [outputs](../video-synth-basics/outputs) can via the usage of the `src` function. Sources need to be initialized with some key info in order to work. We do this by calling an `init` function on them. You'll see them in action below.

---

## webcam

### **initCam**

We can activate the webcam inside a source variable using the `initCam` function. When run, you should see the light on your webcam light up. However, you will still not see any image on the screen. You need to use it within the `src()` function to access the video signal. If you have multiple webcams, you can access each camera by indicating an index number inside `initCam`, for example `s0.initCam(1)` or `s0.initCam(2)`.

`s0.initCam( index = 0, params )`
{.center}

```hydra
s0.initCam() // init webcam on s0
src(s0).out(o0) // show s0 on o0
```

You can add transformations of color and geometry to these just as any other source:

```hydra
s0.initCam()
src(s0).color(-1, 1).kaleid().out()
```

---

## images

### **initImage**

Load an image into a source object:

`s0.initImage( url = "", params )`
{.center}

```hydra
// load an image into a source object
s0.initImage("https://upload.wikimedia.org/wikipedia/commons/2/25/Hydra-Foto.jpg")

// show the image on the screen
src(s0).out()
```

### Usage in pulsar

When running Hydra inside Pulsar, or any other local manner, you can load local files referring to them by URI:

```javascript
s0.initImage("file:///home/user/Images/image.png")
```

### Supported image formats

You can load `.jpeg`, `.png`, and `.bmp` as well as `.gif` and `.webp` (although webp animation won't work).

---

## videos

### **initVideo**

Loading videos is extremely similar to loading an image, you just need to use the `initVideo` function.

`s0.initVideo( url = "", params )`
{.center}

```hydra
s0.initVideo("https://media.giphy.com/media/AS9LIFttYzkc0/giphy.mp4")
src(s0).out()
```

### Supported video formats

You can load `.mp4`, `.ogg` and `.webm` videos.

### HTML Video properties

You can access all of the [HTML Video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) functions when a video is loaded to a Source via `s0.src`. Some useful properties are:

```javascript
s0.src.playbackRate = 2 // double the speed at which the video plays
s0.src.currentTime = 10 // seek to the 10th second
s0.src.loop = false // don't loop the video
```

### Loading videos from YouTube, Vimeo, etc

Some users may be tempted to try and load some video they liked on YouTube, for example, and run something suchlike:

```javascript
s0.initVideo('https://www.youtube.com/watch?v=dQw4w9WgXcQ') // doesn't work
```

This will not work. The same goes for Vimeo and other video streaming services. When you use such an URL, it is not returning a video, it is returning the website where you can watch the video! The URL you pass to `initVideo` has to go directly to a video file. In other words, the URL should (usually) end in `.mp4`, `.webm` or `.ogg`. And, even if you did get a URL directly to the video with a tool such as youtube-dl, you'll run into CORS problems.

#### Workaround

The most common workarounds are:

* Run Hydra locally (on Atom for example) and load local video files
* Have the video run on its own window and use `initScreen` to capture it
* Run a local file server

### Tutorial

{{< youtube bavUH1cv_v0 >}}

---

## screen sharing

### **initScreen**

You can also use the screen-sharing capabilities of most modern browsers as a video source. It works in the same way sharing your screen on video conferences work!

`s0.initScreen( params )`
{.center}

```hydra
// s0.initScreen() // uncomment and run
src(s0).out()
```

### Tutorial

{{< youtube mPH8hpbEs3o >}}

---

## canvas (HTML)

### **init**
This is a more generic function for loading any external source into hydra. This is especially useful when you are using an [HTML canvas element](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) as an input, or loading an existing resource as a source into hydra. Valid input types are documented in the [regl texture documentation](https://github.com/regl-project/regl/blob/main/API.md#textures).

`s0.init( opts, params )`
{.center}

```hydra
myCanvas = document.createElement('canvas')
ctx = myCanvas.getContext('2d')
ctx.font = "30px Arial"
ctx.fillStyle = "red";
ctx.fillText("Hello World", 10, 50)   

s0.init({ src: myCanvas, dynamic: false })

src(s0).diff(osc(2, 0.1, 1.2)).out()
```

### Dynamic inputs

Use the `dynamic` parameter to indicate whether the source will be updated, or remain static!

```javascript
s0.init({src: animatedCanvas, dynamic: true})
s1.init({src: staticImageCanvas, dynamic: false})
```

---

## streaming

{{< hint danger >}}
note: initStream() is currently broken in hydra editor due to server issues
{{< /hint >}}

### **initStream**

The Hydra editor also has built-in streaming. You can stream the output of your Hydra to someone else using Hydra and vice-versa. This is done in a similar fashion to using images and videos, using external sources. Everyone using Hydra has a session, of which they can set the name. Streaming-in video from someone else into your patches is as simple as initiating and passing the name of the session you want to see. Streaming-out your Hydra to someone else requires you to set the name of your session and share it with someone else.

`s0.initStream( streamName, params )`
{.center}

```javascript
// naming my session so he can use it
pb.setName('myverycoolsession')
// loading my friend's session
s0.initStream('myfriendsverycoolsession')

src(s0)
    .out()
```

###  The pb object

On your Hydra editor, you can find a pre-defined object called `pb` (as in patch-bay). This object basically represents the connection of your Hydra editor instance to all others hosted on the same server. When you want to share your stream to someone else you'll have to give your Hydra session a name. Do this using the `pb.setName()` function and by passing in some string as the name. For example: `pb.setName('myverycoolsession')`. If you want someone else to stream to you, ask them to set a name as such and share it with you. You can see online sessions using the function `pb.list()`, which will return an Array of names.

---

## params

Any external sources loaded into Hydra are using [regl's texture constructor](https://github.com/regl-project/regl/blob/master/API.md#textures) in the background. There are many properties you can set when loading a texture, Hydra and regl handle the important ones for you. To set any of these properties yourself you can pass an object containing them to any of the init functions. For example:

```javascript
s0.initCam(0,{mag: 'linear'})
```

`mag` & `min` are the most used parameters, since using `linear` interpolation will resize textures in a smooth way. The default for both is `nearest`.

---

## CORS problems

If you try to load images (or videos) from some websites (most of them, really), sometimes nothing shows up on the screen. Opening the browser's console might reveal a message similar to this one:

{{< hint danger >}}
```
Access to image at '...' from origin 'https://hydra.ojack.xyz'
has been blocked by CORS policy:
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
{{< /hint >}}

The CORS in CORS policy stands for 'Cross-origin resource sharing'. This refers to the action of calling resources (such as images) from one website to another. For example, asking for an image hosted on other website from inside the Hydra editor. This error message is basically telling us _"hey, the website you're trying to ask for an image doesn't allow other websites to use their resources, so I can't let you have that picture"_.

In order to circumvent this error, you can try re-uploading the images you want to use to some image hosting service that allows cross-origin sharing such as imgur, where you can also load short videos. You can also run a local server for your files. There are also some sites which allow cross-origin resource sharing such as [Wikimedia Commons](https://commons.wikimedia.org/), which is great for video.
