---
title: "midi"
draft: false
weight: 62
---


# midi

## hydra-midi

`hydra-midi` is one of the most used extensions for Hydra. It allows you to easily incorporate MIDI devices into your patches. These can be MIDI interfaces, controllers, virtual cables, etc. It can use both notes and CC values. For more information visit the [hydra-midi repository](https://github.com/arnoson/hydra-midi)

### Examples

#### Start-up

```javascript
await loadScript('https://cdn.jsdelivr.net/npm/hydra-midi@latest/dist/index.js')

// Use midi messages from all channels of all inputs.
await midi.start().show()

// Use specific inputs or channels
seaboard = midi.input(1).channel('*')
faderfox = midi.input(2)
```

#### Notes

```javascript
// Get a 0 when the note C4 isn't pressed, 1 when pressed
osc(30,.01).invert(note('C4')).out()

// Trigger an ADSR envelope each time the note C3 is played
// and scale the value to a range between 20 and 50.
osc(note('C3').adsr(300, 200, 1, 300).range(20, 50), 0, 0).out(o1)

// Get a 2 when the note D3 isn't pressed in the seaboard, 8 when pressed
noise(seaboard.note('D3').range(2,8)).out(o2)

// Get the latest velocity played for the note E3
voronoi().color(note('E3').velocity(),1,1).out(o3)
```

#### CC

```javascript
// Get the value from CC 45 ranged from 0 to 2PI
osc().rotate(cc(45).range(0,Math.PI*2)).out(o0)
```

Remember to visit the [hydra-midi repository](https://github.com/arnoson/hydra-midi) for more documentation.

---

## Custom scripts

You can also use your own custom scripts using [Web MIDI](https://webaudio.github.io/web-midi-api/).

The following is a generic script that doesn't care what Midi Channel you're broadcasting on and maps a normalized value 0.0-1.0 into an array named cc.
This portion should be ran in the console & will register Web MIDI & map the incoming CC data to a set of parameters.  For simplicity, these
parameters are named to match the CC number.  The CC values are normally in a range from 0-127, but we've also normalized them to be in a range of 0.0-1.0.

```javascript
// register WebMIDI
navigator.requestMIDIAccess()
    .then(onMIDISuccess, onMIDIFailure);

function onMIDISuccess(midiAccess) {
    console.log(midiAccess);
    var inputs = midiAccess.inputs;
    var outputs = midiAccess.outputs;
    for (var input of midiAccess.inputs.values()){
        input.onmidimessage = getMIDIMessage;
    }
}

function onMIDIFailure() {
    console.log('Could not access your MIDI devices.');
}

//create an array to hold our cc values and init to a normalized value
var cc=Array(128).fill(0.5)

getMIDIMessage = function(midiMessage) {
    var arr = midiMessage.data    
    var index = arr[1]
    //console.log('Midi received on cc#' + index + ' value:' + arr[2])    // uncomment to monitor incoming Midi
    var val = (arr[2]+1)/128.0  // normalize CC values to 0.0 - 1.0
    cc[index]=val
}
```

```javascript
// example midi mappings - Korg NanoKontrol2 CCs
// color controls with first three knobs

noise(4).color( ()=>cc[16], ()=>cc[17], ()=>cc[18] ).out()

// rotate & scale with first two faders
osc(10,0.2,0.5).rotate( ()=>(cc[0]*6.28)-3.14 ).scale( ()=>(cc[1]) ).out()

```
