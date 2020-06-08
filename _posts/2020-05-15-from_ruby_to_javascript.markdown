---
layout: post
title:      "From Ruby on Rails to Javascript"
date:       2020-05-15 09:27:09 -0400
permalink:  from_ruby_to_javascript
---

This project was born after narrowing down from several project ideas that I liked. First I was considering another blog like app like my previous project [Young Papas Hobbies](https://santiagosalazarpavajeau.github.io/ruby_on_rails_project_-_young_papas_hobbies),  and even a restaurnt recipe web app. However after seeing other projects in Javascript I realized the type of web apps that could be built compared to Rails (like games and other interactive single page applications), so I decided to build a music app. Also music is a topic I am passionate about and I have been involved in music ever since I have memory due to my grandmother being a violin player and teacher. Also I have been recording music with my friends since I was a kid. But lately I have been looking into some new music gear like synths which I made some sounds on the app from. I also have been very curious about what goes into chord progessions lately which is the reason why I chose to create this specific app, and had fun building the chords.

This project is meant to be a music web application that lets the user "jam" and create simple songs with a few clicks. It allows to add chords to a "track" that plays a beat. The variety of chords allows you to play almost any chord progression.  All the sounds are stored in the local directory, and they are accessed through an *audio* html tag. We create the audio tags and accompanying buttons to play the sounds, by using a Javascript class called *Chord*. This class creates all the functionality of the audios, including playing the sounds on a click event, adding the sound to the track and removing the sounds from the track. 

All this functionality is done through the ability of JS to *manipulate the DOM*  (change the web page's behavior from the users perspective).

## Organizing a Javascript App

In rails, applications are organized in a very specific way with controllers, router, models, views. However, when we work with plain Javascript for the front end of our app, we have to create our own design to keep our app organized. For this we can use Modules to create different classes that have specific responsabilities. In my case, I saw some of the opportunites to the code into around mid-way on in my project but ended up choosing the following:

* App: launches the code.
* Adapter: defines all the fetch/AJAX requests and manipulates the DOM.
* Song: creates song object.
* Chord: creates chord object.

### Troubleshooting accesing contexts of different classes

I tried to further separate the DOM manipulation responsibilities of the Adapter class into a new "UserInterface" class that exclusively had these methods/functions. However I realized that while this works to get data from the database into the front end it is more complicated to use it when sending data to the database (POST requests, etc.).

So I found myself having to set the Post request method from the UserInterface class and not the Adapter, because the user input was obtained in the UserInterface class. That defeated my assumption that the Adapter could be exclusively responsible for AJAX requests and the UserInterface for DOM manipulation. Thus my Adapter class by itself ended up having to handle all of the fetch requests and DOM manipulation with methods in its own scope/context, in order for methods to have access to the data.

### Interaction between modules

In a nutshell, the App class declares the Adapter which has all of the methods to load the app like loading the songs from the database, loading the chords and the beats.

The songs from the database are created in the front end by passing the json into a the renderSongButton method, which creates a button for the song that plays all of the chords belonging to the song when clicking on it. 

Then, a new song object is created on load for the user to interact with and save into the database.

The chords are pushed into a song into three Song methods, chords, to hold all the Chord objects, audios to store the chord audios, and files, to reference the .wav files on the front end directory.

## Main Adapter Methods:

### Editing the song in the track card

So the goal was to be able to edit a song in the track card by adding/removing chords. This was a very challenging issue because of the structure of the chord and song objects. Initially I considered joining the chord .wav sounds into a new .wav file to create a song, however, I found I could use collections of chords to make the song and edit the collections to acheive the editing of the song. Each chord has an "audio" HTML tag that can be played on the browser.

This brought a new level of complexity because both classes had to deal with audio html tags in order to play sound, on top of their other attributes.

The process to delete a chord from a song involved creating a random id property called *edit_id* on the chords so that the audios on the track could be filtered by that edit_id. This attribute is assigned on the creation of a chord object, and its also assigned to the corresponsing audio html tag. For the audios in a song to have the same edit_id than the chords in the song they have to be created simultaneusly, however I encountered a bug where I was not creating them in the same instance so I was getting an edit_id that was not the same on the audio tag and the chord attribute. So my filter method was not finding the correct id to delete the chord and audio from the song.

However, after making sure I wasn't creating two different random numbers when they needed to be the same the following method acheived the correct editing of the song:

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

As far as "playing one audio after another" on the audios array in a song, there is a couple of ways to accomplish this, the two main basic methods I found were to with either an "onended" event listener or with a setInterval callback. 

I was able to implement playing the audios on sequence with the setInterval option by creating a callback that would play the next audio every 2 seconds. Since the chords might need to be replayed in the same song, I pause the audios and reset them to the start. Then beat audio stops and the interval is cleared when the index is greater than the number of chords.

```
playSong(song){
        
        let allAudios = document.querySelectorAll("audio")
            
        for(let audio of allAudios){
            audio.pause()
            audio.currentTime = 0
        }
        
        let playAudio = function(index){
                            return function(){
                                if (index < song.audios.length -1 ){
                                    song.audios[index].currentTime = 0
                                    song.audios[index].pause()
                                    index += 1
                                    song.audios[index].play()
                                } else{
                                    clearInterval(playInterval)
                                    
                                    song.beat.pause()
                                    song.beat.currentTime = 0;
                                    const songButtons = document.getElementsByClassName("button btn-dark song")
                                    document.getElementById("play").disabled = false
                                    for(let songButton of songButtons){
                                        songButton.disabled = false
                                    }
                                    

                              
                                }
                                
                            }
                        }
            
        
        song.audios[0].play()
        song.beat.play()
        
        let i = 0
        const playInterval = setInterval(playAudio(i), 2000)
        this.intervals.push(playInterval)

    }
```

After having a functioning but basic MVP with plain JS, it is worth mentioning the standard libraries for music apps would be [ Web Audio API](https://en.wikipedia.org/wiki/HTML5_audio#Web_Audio_API_and_MediaStream_Processing_API), [WebMIDI](https://www.w3.org/TR/webmidi/), and [Tone.js](https://tonejs.github.io/.).





## Transitioning from Ruby to JS

I just wanted to mention some of the basics of moving to JS. Learning Ruby as a first language spoils you in a sense because it is really simple. There are many built in methods that will allow you to do things easily like the .each method which is almost equivalent to a .forEach of .map in Javascript.

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
 
Also Ruby uses *binding.pry* as a debugger, which stops the code in the scope of the execution with the methods and variables defined and available to access in the terminal, while Javascript uses *debbuger* which sends you to the browser where almost the same usability can be found (mostly the downside being that the console in the browser can be by default smaller than a terminal). Also in the learn.co platform when tests are ran with *learn --f-f* Javascript does not exit the test at the first failure but keeps running all the tests. But in the end it is a matter of getting used to the change and continously learning new ways.

Other big difference that Javascript brings is that many of the built-in functional methods may be limited or difficult to implement but with libraries like [ramda](https://ramdajs.com/) the methods in Javascript can become similar in simplicity to those found in Ruby. 

Ruby is a great language to start programming and get a grasp of concepts without having to have a deep knowledge of technical syntax other programming languages use like memory management syntax or even curly braces.

Concepts that loosely translate from Ruby to Javascript also include .self (this), object attribute accessors (constructors) and many more. However, in the end they may act different and so it is important to settle into new concepts and syntax properly.

### Connection between Rails API and a Javascript front end

My Ruby on Rails backend for this project was fairly simple, with the Chord and Song classes being parallel to the front end. I render json for each song and use the Fast Json API serializer to send the nested chord instances to the Adapter on the JS side. The serializer class to produce our service with our data looks like this:

```
class SongSerializer
  include FastJsonapi::ObjectSerializer
  attributes :name, :chords

  def chords
    object.chords
  end
end

```

The chords method, allows for our chords attribute on the serializer to access all the chord objects (and their data structure) belonging to the song.

Also to create a new song I recieve a JSON from the Adapter with the song name and chords and save the song and its chord instances into the database using the params hash. The same process is applied to delete a song by using the params hash to find the song and delete each of its chord instances from the database. The difference between a POST and DELETE request is that the latter does not need a body paramaterer in the configuration object, only a reference to the song id on the URL where our request is directed to, so the id can be accessed from the params hash on the Rails destoy action in the Song controller.

```
saveSong(song){ 
        let postObj = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            },
            body: JSON.stringify({
                song: {
                    name: song.name, // returns name string from input
                    chords_attributes: song.chords //returns array of chord objects //need only an array of files
                }
            })
        }
        return fetch(`${this.baseURL}/songs`, postObj)
            .then(resp => resp.json())
            .then(json=> {
                let chordObjs=[]

                for(let chord of json.data.attributes.chords){
                    chordObjs.push(new Chord(chord.name, chord.file))
                }
                let song = new Song(json.data.attributes.name, chordObjs, json.data.id)
                this.allSongs.push(song)

                this.renderSongButton(song)
            }) 
            .catch(error => alert(`Cant render new song and ${error}`))
    }
		
		deleteSong(song){
        let deleteObj = {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            },
  
        }
        return fetch(`${this.baseURL}/songs/${song.id}`, deleteObj)
            .then(resp => resp.json())
            .then(()=> {
                alert("song deleted")
            }) 
            .catch(error => alert(`Couldn't delete song and ${error}`))
    }

```

## Conclusion

This has been a very rewarding experience, and it has been fun. It was very interesting to have a working app and being able to deploy it to the web through [heroku and Github Pages](https://santiagosalazarpavajeau.github.io/chords_beats_frontend/).

Github Repos:
* [Front End](https://github.com/SantiagoSalazarPavajeau/chords_beats_frontend)
* [Back End](https://github.com/SantiagoSalazarPavajeau/chords_beats_backend)
* [Original before Postgress implementation](https://github.com/SantiagoSalazarPavajeau/chords_and_beats)

