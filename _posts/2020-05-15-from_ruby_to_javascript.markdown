---
layout: post
title:      "From Ruby on Rails to Javascript"
date:       2020-05-15 09:27:09 -0400
permalink:  from_ruby_to_javascript
---


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

In a nutshell, the App class declares the Adapder and the Adapter handles all of the rest of the methods to load the app like loading the songs from the database, and the chords. New chord and song objects are created with the response from fetch. 

The songs from the database are created in the front end by passing the json into a the renderSongButton method, which creates a button for the song that plays all of the chords belonging to the song in sequence. 

Then, a new song object is created on load for the user to interact with and save into the database.

## Editing the song in the track card

So the goal was to be able to edit a song in the track card by adding/removing chords. This was a very challenging issue because of the structure of the chord and song objects. Both classes have to deal with audio html tags in order to play sound, on top of their other attributes.

## Transitioning from Ruby to JS

Learning Ruby as a first language spoils you in a sense because it is really simple. There are many built in methods that will allow you to do things easily like the .each method which is almost equivalent to a .forEach in Javascript.

```
# Ruby:

collection.each do |element|
 puts "Hello!"
end

// Javascript

collection.forEach(function(element){
 console.log("Hello!")
})
```
 
 While Ruby has only a block that will be executed in the iteration, Javascript makes an explicit call to a function. Ruby uses the puts method to print a string to the terminal and Javascript uses the console.log to print to the console on the browser. 
 
Also Ruby uses *binding.pry* as a debugger, which stops the code in the scope of the execution with the methods and variables defined and available to access in the terminal, while Javascript uses *debbuger* which sends you to the browser where almost the same usability can be found (mostly the downside being that the console in the browser can be by default smaller than a terminal). Also in the learn.co platform when tests are ran with *learn --f-f* Javascript does not exit the test at the first failure but keeps running all the tests. But in the end it is a matter of getting used to change and learning.

Other big difference that Javascript brings is that many of the built-in functional methods may be limited or difficult to implement but with libraries like [ramda](https://ramdajs.com/) the methods in Javascript can become similar in simplicity to those found in Ruby. 

Ruby is a great language to start programming and get a grasp of concepts without having to have a deep knowledge of technical syntax other programming languages use. 

Concepts that loosely translate from Ruby to Javascript also include .self (this),  object attribute accessors (constructors) and many more. However, in the end they may act different and so it is important to settle into new concepts and syntax properly.
