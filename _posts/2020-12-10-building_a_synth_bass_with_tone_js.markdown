---
layout: post
title:      "Building a Synth Bass with Tone.js"
date:       2020-12-10 09:32:29 -0500
permalink:  building_a_synth_bass_with_tone_js
---


As my first main experiment with JavaScript, I built an app to [play around with chord progressions](https://santiagosalazarpavajeau.github.io/chords_beats_frontend/) which allows the user to add different combinations of `chords` to a `song` and experiment with chord progressions to provide a quick and simple 'song-making' experience.

This project has really helped me improve my JS skills and it helps me to keep coding when I need a break from other projects.

So, I have been looking into improving the code and adding more features since the first iteration, and so recently I was able to add a synth bass keyboard with the Tone.js library. Here I explain the process.

## Implementing [Tone.js](https://tonejs.github.io/)

I find this library very interesting because it comes with a synthesizer right out of the box. So all the common properties of sound design like oscillator/wave types, attack, release, frequency, envelope, detune, filter, etc. can be set and played.

It is fairly straightforward to get the 'keyboard' working since a synth can be initialized with:

``` javascript
const synth = new Tone.Synth().toDestination()
```

And different keys can be played by just by passing a note and duration value:

```javascript
synth.triggerAttackRelease("A1", "8n")
```

However, what really excites me about the library is to be able to design the sound of the synthesizer, so I started by adding buttons that can change the wave shape of the oscillator, so it can sound softer(sine) or more robotic(sawtooth). This can be achieved by simply setting:

```javascript
synth.oscillator.type = "sawtooth"
```

[Checkout the method where the Synth Bass is implemented.](https://github.com/SantiagoSalazarPavajeau/chords_beats_frontend/blob/384b012b8389aa3395e8a692b78ecf9c752cc3f0/adapter.js#L67)

## Keydown event listener and switch statement

Now to implement the actual feature of playing the 'synth bass' notes when pressing keyboard keys we can just use event listeners for `keydown`. This type of event has an `event.code` property so the switch statement can tell which key has been pressed trigger a different note with each key.

``` javascript
  document.addEventListener("keydown",  (e) => {

            switch (e.code) {
                case "ShiftLeft":
                  return synth.triggerAttackRelease("A1", "16n")
                case "KeyZ":
                  return synth.triggerAttackRelease("A#1", "16n")
                case "KeyX":
                  return synth.triggerAttackRelease("B1", "16n")
                case "KeyC":
                  return synth.triggerAttackRelease("C2", "16n")
                /// ETC...
                default:
                  return
              }
        })

    }
```

As the code shows, coupling the event listener with a switch statement allows triggering a specific note for each key. Our `e.code` argument changes with each keypress so for example pressing left shift (`ShiftLeft`) will play an 'A1' for the duration of an eight note.

So the synth bass currently plays by pressing keys in between the left and right shift to have all lower 12 notes and from A to Enter for the 12 higher octaves which allows to play higher and lower notes easily.

## Event listener for oscillator wave type

I implemented a basic on click event listener to change the wave type and sound of the Synth Bass.

So I added some buttons to the synth bass card where the type of the wave can be picked to change the sound of the synth bass. The options are sine, sawtooth, triangle, and square. To acheive this we just add buttons and corresponding event listeners that set the type of wave.

``` html
   <button class="waves">Triangle</button>
   <button class="waves">Sawtooth</button>
   <button class="waves">Square</button>
   <button class="waves">Sine</button>
```

```javascript
const wavesButtons = document.querySelectorAll("button.waves")

for( let wavesButton of wavesButtons){
   wavesButton.addEventListener("click", (e) => {
      synth.oscillator.type = e.target.innerText.toLowerCase()
   })
}
```

In this case, I only use the lowercase inner text of the buttons to set the oscillator wave type because these texts correspond to the properties that `synth.oscillator.type` accepts. 

## Looking forward

I am looking to improve the UI/UX layout but haven't been able to decide on a specific idea. However, I am thinking of making the app more accessible implementing some type of mini-modals tutorial. Also looking to add some feature that helps the user decide which chord progressions to use either by showing chord progressions of actual songs or by a 'smart' suggestion' of a good next chord depending on the theme wanted for the song. There are many possibilities of where this project can be headed to which will surely keep me busy building for a long time.

Please feel more than welcome to share any thoughts/ideas and reach out!

[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)
[Twitter](https://twitter.com/santispavajeau)
