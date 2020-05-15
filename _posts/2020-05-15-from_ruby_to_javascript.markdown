---
layout: post
title:      "From Ruby to Javascript"
date:       2020-05-15 13:27:08 +0000
permalink:  from_ruby_to_javascript
---


Learning Ruby as a first language spoils you in a sense because it is really nice and simple. There are many built in methods that will allow you to do things easily like the .each method which is almost equivalent to a .forEach in Javascript.

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
 
Also Ruby uses *binding.pry* as a debugger, which stops the code in the scope of the execution with the methods and variables defined and available to access in the terminal, while Javascript uses *debbuger* which sends you to the browser where almost the same usability can be found (mostly the downside being that the console in the browser can be by default smaller than a terminal). But in the end it is a matter of getting used to change and learning.

Other big difference that Javascript brings is that many of the built-in functional methods may be limited or difficult to implement but with libraries like [ramda](https://ramdajs.com/) the methods in Javascript can become similar in simplicity to those found in Ruby. 

Ruby is a great language to start programming and get a grasp of concepts without having to have a deep knowledge of technical syntax other programming languages use. 

Concepts that loosely translate from Ruby to Javascript also include .self (this),  object attribute accessors (constructors) and many more. However, in the end they may act different and so it is important to settle into new concepts and syntax properly.
