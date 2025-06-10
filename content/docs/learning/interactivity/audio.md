---
title: "audio"
draft: false
weight: 61
---

# audio

The current version of Hydra can use the default microphone as an input, it uses [Meyda](https://github.com/meyda/meyda) in order to analyze the sound and get values for audio reactivity. This works internally using the FFT algorithm.

---

## the **a** object

The a object gives you access to all of the audio functionality. Please follow along on the Hydra editor:

### **a.show**
Show the FFT bins near the canvas.
```javascript
a.show()
```

### **a.setBins**

Set the number of FFT bins. A bin is a part of the audible spectrum Hydra will analyze on the mic input.
```javascript
a.setBins(6)
```

### **a.fft[]**
You can access the value of the leftmost (lowest frequency) bin by calling the first element in the `a.fft` array. Using higher numbers will get you the higher frequency bins.
```javascript
a.fft[0]
```

#### Usage

These should be used inside arrow functions, this way Hydra will check their value each time a frame is rendered, making the audio reactivity happen.
```javascript
osc(10, 0, () => a.fft[0]*4)
  .out()
```

### **a.setCutoff**
It is possible to calibrate the responsiveness by changing the minimum and maximum value detected. (Represented by blur lines over the fft). To set minimum value detected:
```javascript
a.setCutoff(4)
```

### **a.setScale**
Setting the scale changes the range that is detected. The `a.fft[]` array will return a value between 0 and 1, where 0 represents the cutoff and 1 corresponds to the maximum.
```javascript
a.setScale(2)
```

### **a.setSmooth**
You can set smoothing between audio level readings (values between 0 and 1). 0 corresponds to no smoothing (more jumpy, faster reaction time), while 1 means that the value will never change.
```javascript
a.setSmooth(0.8)
```

### **a.hide**
To hide the FFT bins:
```javascript
a.hide()
```

---

## Example
```javascript
a.setBins(5) // amount of bins (bands) to separate the audio spectrum

noise(2)
	.modulate(o0,()=>a.fft[1]*.5) // listening to the 2nd band
	.out()

a.setSmooth(.8) // audio reactivity smoothness from 0 to 1, uses linear interpolation
a.setScale(8)    // loudness upper limit (maps to 0)
a.setCutoff(0.1)   // loudness from which to start listening to (maps to 0)

a.show() // show what hydra's listening to
// a.hide()

render(o0)
```
