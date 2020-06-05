---
layout: post
title:      "From Ruby on Rails to Javascript"
date:       2020-05-15 09:27:09 -0400
permalink:  from_ruby_to_javascript
---

This project was born after I was having again a hard time finding an idea that I liked. First I was considering another blog like app like my previous project [Young Papas Hobbies](https://santiagosalazarpavajeau.github.io/ruby_on_rails_project_-_young_papas_hobbies), however after seeing other projects in Javascript and what it could be done compared to Rails (like games and interactive single page applications), I went for a music app. Also music is a topic I am passionate about since I have been involved in music since I have memory due to my grandmother being a violin player and teacher. Also I recorded an album by myself when I was in highschool around 10 years ago, and I have been gearing up with some music gear lately. Specially proud of my analog synth, which made the chord sounds for this app, I also have been very curious about chord progessions lately which is the reason why I chose to create this specific app.

This project is meant to be a music web application that lets the user "jam" and create simple songs with a few clicks. It allows to add chords to a track and plays a beat. The variety of chords allows you to play almost any chord progression.  These sounds are stored in the local directory, and they are accessed through an *audio* html tag. We create the audio tags and accompanying buttons to play the sounds, by using a Javascript class called *Chord*. This class creates all the functionality of the audios, including playing the sounds on a click event, adding the sound to the track and removing the sounds from the track. 

All this functionality is done through the ability of JS to *manipulate the DOM*  (the web page's behavior).

## Organizing a Javascript App

In rails applications are organized in a very clear way with controllers, router, models, views. However, when we work with plain Javascript for the front end of our app we have to create our own design to keep our app organized. For this we can use Modules to create different classes that have specific responsabilities. In my case, I saw some of the opportunites to separate concerns later on in my project but ended up choosing the following:

* App: launches the code.
* Adapter: defines all the fetch/AJAX requests and manipulates the DOM.
* Song: creates song object.
* Chord: creates chord object.

### Troubleshooting accesing contexts of different classes

I tried to further separate the responsibilities of the Adapter class into a new "UI" class that exclusively manipulated the DOM, however while this works to get data from the database into the front end it is more complicated to use the same lgic when sending POST requests. I found myself having to initialize a request from "UI" class but would have to set a new Adapter on the constructor and then on the constructor set a new UI class. This generates an infinite loop and a stack overload. Thus my Adapter class by itself ended up having to handle all of the fetch requests and DOM manipulation with methods in its own scope/context.

### Interaction between modules

In a nutshell, the App class declares the Adapter which handles all of the rest of the methods to load the app like loading the songs from the database, and the chords. So new chord and song objects are created with the response from fetch. 

The songs from the database are created in the front end by passing the json into a the renderSongButton method, which creates a button for the song that plays all of the chords belonging to the song when clicking on it. 

Then, a new song object is created on load for the user to interact with and save into the database.

## Editing the song in the track card

So the goal was to be able to edit a song in the track card by adding/removing chords. This was a very challenging issue because of the structure of the chord and song objects. Initially I considered creating new .wav files to create a song but to acheive the editing ended up using an array of html audio tags as the song. 

This brings a new level of complexity because both classes have to deal with audio html tags in order to play sound, on top of their other attributes. And in a song there is a chords attribute that contains the chord objects but also the audios attribute which is a collection of html tags that is iterated over in order to play each audio.

A refactoring could be to only access the audios from the chord objects in the song, so that there are no audios on the song but only chord objects.

The process to delete a chord from a song involved creating a random id property called *edit_id* on the chords so that the audios on the track could be filtered by that edit_id. This attribute is assigned on the creation of a chord object, and its also assigned to the corresponsing audio html tag. For the audios in a song to have the same edit_id than the chords in the song they have to be created simultaneusly, however I encountered a bug where I was not creating them in the same instance so I was getting an edit_id that was not the same. So my filter method was not finding the correct id to delete the chord and audio from the song.

```
chordButtonTrack.addEventListener("click", (e)=>{
             
 this.newSong.chords = this.newSong.chords.filter((chord)=>{return chord.edit_id !== newSongChord.edit_id})
 this.newSong.audios  = this.newSong.audios.filter((audio)=> {return parseInt(audio.id) !== newSongChord.edit_id})
 newSongChord.audio().pause()
 newSongChord.audio().currentTime = 0
 chordButtonTrack.parentNode.removeChild(chordButtonTrack)
                
})
```
### Playing an array of html audio tags

This was one of the most unexpected issues of the project, I thought it was going to be straight forward playing one audio after another. However the complexity starts at the fact that the song tempo (beat) and the duration of the audio files have to be in sync. Which is an issue mostly outside javascript when just using external audio files. For that reason I created some chord audio files that were at the synchronized with the beat. 

As far as "playing one audio after another" on the audios array in a song, there is a couple of ways to accomplish this, the two main basic methods I found were to with either an "onended" event listener or with a setInterval calback. 

I was able to implement playing the audios on sequence with the setInterval option by creating a callback that would play the next audio every 2 seconds. Since the chords might need to be replayed in the same song, I pause the audios and reset them to the start. On the other hand, to modify the length of the beat and the song while its edited we create a condition to stop the intervals when we finish playing all the audios (array) in the song.

```
playSong(song) {
        
        let allAudios = document.querySelectorAll("audio")
            
        for(let audio of allAudios){
            audio.pause()
            audio.currentTime = 0
        }
        
        let playAudio = function(index){
                            return function(){
                                if (index < song.audios.length -1 ){
                                    song.audios[index].currentTime = 0
                                    index += 1
                                    song.audios[index].play()
                                } else{
                                    clearInterval(playInterval)
                                    clearInterval(stopInterval)
                                    
                                    song.beat.pause()
                                    song.beat.currentTime = 0;
                              
                                }
                                
                            }
                        }
            
        let stopAudio = function(index){
                            return function(){
                                if (index < song.audios.length){
                                    song.audios[index].pause()
                                    song.audios[index].currentTime = 0;
                                
                                } 
                                
                            }
                        }
        song.audios[0].play()
        song.beat.play()
        
        let i = 0
        const playInterval = setInterval(playAudio(i), 2000)
        const stopInterval = setInterval(stopAudio(i), 1500)
        
    }
```

After having a functioning MVP with plain JS, it is worth mentioning the standard libraries for music apps would be [ Web Audio API](https://en.wikipedia.org/wiki/HTML5_audio#Web_Audio_API_and_MediaStream_Processing_API), [WebMIDI](https://www.w3.org/TR/webmidi/), and [Tone.js](https://tonejs.github.io/.).





## Transitioning from Ruby to JS

I just wanted to mention some of the basics of moving to JS. Learning Ruby as a first language spoils you in a sense because it is really simple. There are many built in methods that will allow you to do things easily like the .each method which is almost equivalent to a .forEach in Javascript.

```
# Ruby:

collection.each do |element|

end

// Javascript

collection.forEach(function(element){
})

also for specific uses:

collection.map(function(element){
})

collection.reduce(function(element){
}) // to return one aggregated value

collection.filter(function(element){
}) // to return elements of our choice


```
 
 While Ruby has only a block that will be executed in the iteration, Javascript makes an explicit call to a function. Ruby uses the puts method to print a string to the terminal and Javascript uses the console.log to print to the console on the browser. 
 
Also Ruby uses *binding.pry* as a debugger, which stops the code in the scope of the execution with the methods and variables defined and available to access in the terminal, while Javascript uses *debbuger* which sends you to the browser where almost the same usability can be found (mostly the downside being that the console in the browser can be by default smaller than a terminal). Also in the learn.co platform when tests are ran with *learn --f-f* Javascript does not exit the test at the first failure but keeps running all the tests. But in the end it is a matter of getting used to change and learning.

Other big difference that Javascript brings is that many of the built-in functional methods may be limited or difficult to implement but with libraries like [ramda](https://ramdajs.com/) the methods in Javascript can become similar in simplicity to those found in Ruby. 

Ruby is a great language to start programming and get a grasp of concepts without having to have a deep knowledge of technical syntax other programming languages use. 

Concepts that loosely translate from Ruby to Javascript also include .self (this),  object attribute accessors (constructors) and many more. However, in the end they may act different and so it is important to settle into new concepts and syntax properly.
