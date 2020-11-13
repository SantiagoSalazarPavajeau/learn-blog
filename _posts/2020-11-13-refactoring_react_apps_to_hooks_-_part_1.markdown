---
layout: post
title:      "Refactoring React apps to Hooks - Part 1"
date:       2020-11-13 16:19:14 +0000
permalink:  refactoring_react_apps_to_hooks_-_part_1
---

React class components can manage data with built in state and props, but when more complex data is used in an application,  Redux can also help manage it. However, Redux is very verbose and so Hooks can help reduce lines of code, which is why it has become the standard way to manage application data in React. The Hooks that I will be exploring are `useState()`, `useEffect()`, [`useSelector()`, and `useDispatch()` Part 2](https://dev.to/santispavajeau/refactoring-apps-from-class-based-react-with-redux-to-functional-hooks-part-2-l14) as using these will allow a working refactor from class-based React with Redux. But lets start with `useState()` and `useEffect()` for now.

### Refactoring from class-based to functional React

The first step to be able to access hooks is to change your components from classes to functions. So a component would change like this:

```javascript
import React, {Component} from 'react';

class App extends Component{

 renderText = () => {
  return <p> I am a React class component </p>
 }

 render(){
  return(
   {this.renderText()}
  )
 }
}
```

to:

```javascript
import React from 'react';

const App = () => {

 const renderText = () => {
  return <p> I am a React arrow function component </p>
 }
 return(
  {renderText()}
 )
}

// or 

function App(){

 function renderText(){
  return <p> I am a React functional component </p>
 }
 return(
  {renderText()}
 )
}
```

So we don't need to inherit from the React Component class and also we don't use the render method but only a return statement containing the HTML tags. Also we don't use `this` anymore and access class methods in functional React, since methods are just defined in the local scope of the functional component.

### useState()

This is probably the most fundamental Hook and it replaces the state object in class-based. The way I use state in class components is by just doing:

```javascript
import React, { Component } from 'react';

class App extends Component{
 state = {
  state1: "some changing value by/for the user",
  state2: "etc.."
 }
 
 handleClick(){
  setState({
   state1: "state1 changed after we clicked on the button"
  })
 }

 render(){
  return(
   <button onClick={this.handleClick}> Click me to change the following text! </button>
  <p> {this.state.state1} </p>
  )
 }
}
```

And the same functionality can be achieved by writing this in functional JS (arrow functions is my preference):

```javascript
import React, { useState } from 'react';

const App = () => {

 const [state1, setState1] = useState("some changing value by/for the user")
 const [state2, setState2] = useState("etc...")
 
 const handleClick = () => {
  setState1("state1 changed after we clicked on the button")
 }

 return(
  <button onClick={handleClick}> Click me to change the following text! </button>
  <p> {state1} </p>
 )
}
```

Note that we don't use `setState()` to change state but we name our own methods (e.g. `setState1` and `setState2`) that can change each specific state attributes.

Using the useState() hook might not reduce the related lines of code significantly but it allows the replacement of the `this.state` class method with the direct names (e.g. `state1`, `state2`) of the attributes we want to manage with local state in the component.

### useEffect()

This hook is a bit more complex and it replaces the lifecycle methods for class-based React. For useEffect() to behave like componentDidMount() we only add an empty array as a second argument to the hook: `useEffect(()=>{},[])`. This allows to call functions that require that the DOM is already rendered, like for example an async fetch to the back-end that renders data on loaded DOM nodes. For it to behave like componentDidUpdate() we add a value to the array that will trigger the callback when the value changes like `useEffect(()=>{},[value])`. Last, for it to behave like componentWillUnmount() we just return the function that undos the side effect.

The componentDidMount() lifecycle method in class-based React would look like:

```javascript
import React, { Component } from 'react';

class App extends Component{

 state = {
  username: ''
 }
 
 componentDidMount(){
  fetch('some.api/data')
   .then(response => response.json())
   .then(data => this.setState({username: data.username})
 }

 render(){
  return(
  <p> {this.state.username} </p>
  )
 }
}
```

And the refactor to hooks would simply look like:

```javascript
import React, { useState } from 'react';

const App = () => {

 const [username, setUsername] = useState("")
 
 useEffect(()=>{
  fetch("some.api/data")
   .then(response => response.json())
   .then(data => setUsername(data.username)
 }, [])

 return(
  <p> {username} </p>
 )
}
```

Here we use the two hooks useState() and useEffect() which are some of the main React Hooks, and allow for a refactoring of components that have state and use lifecycle methods. Further control of useEffect() like activating callbacks with events can be achieved by coupling it with another React hook called useRef(). Also, useCallback() and useMemo() can be used to improve performance for computationally expensive components, but we'll leave those for another series of posts. 

I will be covering Redux Hooks useSelector() and useReducer() on the next blog, which will allow for a full refactor of a React Redux app to Hooks.

Thank you for taking a look!

[Visit an example of a refactor in my project](https://github.com/SantiagoSalazarPavajeau/react-projects/blob/master/src/App.js)

## To connect with me:

[Twitter](https://twitter.com/santispavajeau)
[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)

## Also some of my projects:

* [A project management app or "Team Todos" in React](https://santiagosalazarpavajeau.github.io/react-projects/#/projects)

* [A music-jam app in Vanilla Javascript](santiagosalazarpavajeau.github.io/chords_beats_frontend/)

* [A community for fellow young fathers in Ruby on Rails](https://pure-island-81017.herokuapp.com/)


Some reference articles:
https://leewarrick.com/blog/react-use-effect-explained/
https://medium.com/trabe/react-useeffect-hook-44d8aa7cccd0
https://www.digitalocean.com/community/tutorials/react-converting-to-a-hook
