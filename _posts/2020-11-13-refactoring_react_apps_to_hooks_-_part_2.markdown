---
layout: post
title:      "Refactoring React apps to Hooks - Part 2"
date:       2020-11-13 16:19:48 +0000
permalink:  refactoring_react_apps_to_hooks_-_part_2
---


On the [first part](https://dev.to/santispavajeau/refactoring-apps-from-class-based-react-with-redux-to-functional-hooks-n58) we looked at a simple example in refactoring class-based React to functional React with Hooks through `useState()` and `useEffect()` hooks . This blog will be focusing on `useSelector()` and `useDispatch()` to finally make Redux to be compatible with functional React with Hooks. First, to clarify, the hooks that we are going to look into in this blog are part of 'Redux Hooks'. So they are actually imported as follows:

```javascript
import {useReducer, useSelector} from "react-redux";
```

and the documentation is found at: https://react-redux.js.org/api/hooks.

Getting straight to the code the index.js where we launch the basic structure of the app and connect the React DOM to the document will look the same on a class-based app with redux or a functional components with Hooks app.

```javascript
// ./index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore,  applyMiddleware} from 'redux';
import { Provider } from 'react-redux';
import thunk from 'redux-thunk';
import reducer from './reducers/index';
import App from './App';

export const store = createStore(reducer, 
  applyMiddleware(thunk)
  );

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

Here we connect Redux to React through the `createStore()` method, by passing the custom reducer of the app, and this will stay the same in our refactor. So the changes happen when we access the store from our container components. The Redux syntax that is verbose and dreaded uses `connect`, with its intimidating arguments `mapStateToProps`, and `mapDispatchToProps` but this is exactly what we can get rid of with Hooks.

```javascript
import React, {Component} from 'react';
import { connect } from 'react-redux';

class App extends Component{
 state = {
  notifications: false
 }

 handleNotifications = () => {
  this.setState({
    notifications: !this.state.notifications
  })
 }

 handleSaveSettings = () => {
  this.props.saveSettings(this.state)
 }

 render(){
  return(
    <>
    {this.state.notifications ? "ON" : "OFF"}
    <br/>
    <button onClick={this.handleNotifications}> Notifications 
    <br/>
    <br/>
    <button onClick={this.handleSaveSettings}>SAVE</button>
   </> 
  )
 }
}

const mapStateToProps = state => {
  return {
    notifications: state.notifications,
  }
}

const mapDispatchToProps = dispatch => {
 return {
   saveSettings: (settings) => dispatch({type: "SAVE_SETTINGS", settings})
 }
}

export default connect(mapStateToProps, mapDispatchToProps)(App);
```

Here we are saving notification and email settings by clicking on the buttons and this could be connected to a backend using thunk.

So as we see a class component with only one action and two attributes on Redux state is already very verbose. If the same thing can be done with less lines of code then it should be done, so we will get rid of the `connect()` function and its arguments and only call `useDispatch()` and `useSelector()` hooks. 


```javascript
import React, {useState} from 'react';
import { useDispatch, useSelector } from 'react-redux';

function App(){

 const [notifications, setNotifications] = useState(false)

 const dispatch = useDispatch()
 const props = useSelector(state => state)

 const handleSaveSettings = () => {
  dispatch({type: "SAVE_SETTINGS", notifications})
 }

 return(
    <>
    {notifications ? "ON:" : "OFF:"}
    {props.notifications ? "(Currently activated in store)" : "(Currently deactivated in store)"}
    <br/>
    <button onClick={() => setNotifications(!notifications)}> Notifications 
</button>
    <br/>
    <br/>
    <button onClick={handleSaveSettings}>SAVE SETTINGS</button>
   </> 
  )
}

export default App;
```

Also on the above hooks example we could have a simple one-action reducer that would look like this:

```javascript
// reducer.js
export default function reducer(state, action) {
  if (typeof state === "undefined") {
    state = {notifications: false}  ; // Redux state brought from database in real life
  }

  if (action.type === "SAVE_SETTINGS") {
    let notifications = action.notifications;
    return {...state, notifications};
  } else {
    return state;
}
```

So there are around 20 less lines of code for the same functionality with React-Redux and that is for a simple container component. 

On Hooks we simply reference `dispatch` directly on the scope of the component after initializing it. Whereas in class-based  `mapDispatchToProps` has to be defined and then passed into `connect` as an argument.

```javascript
const mapDispatchToProps = dispatch => {
 return {
   saveSettings: (settings) => dispatch({type: "SAVE_SETTINGS", settings})
 }
}

export default connect(mapStateToProps, mapDispatchToProps)(App);
```

Then we had to call dispatch as follows:

```javascript
 this.props.saveSettings(this.state)
```

But with hooks all we do is define dispatch:

```javascript
 const dispatch = useDispatch()
```

And call dispatch with our action on an event callback:

```javascript
 dispatch({type: "SAVE_SETTINGS", notifications})
```

Which is a less verbose way of executing actions for our reducer and there is no need for `connect`.

With Hooks we can also access Redux state directly with the `useSelector()` method, and our Redux state will only update when we send a valid action through the `dispatch` method.

So we can replace the following code :

```javascript
const mapStateToProps = state => {
  return {
    notifications: state.notifications,
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(App);
```

With simply:

```javascript
const props = useSelector(state => state)
```

Inside the scope of the functional component and access the Redux state as:

```javascript
{props.notifications ? "(Currently activated in store)" : "(Currently deactivated in store)"}
```

When we click on the "save settings" button we dispatch an action to Redux State and update the props variable that contains this layer of data we just saved.

[Sandbox code for this example](https://codesandbox.io/s/refactoring-class-based-react-redux-to-hooks-ohg0d?file=/src/reducer.js)

<iframe src="https://codesandbox.io/embed/refactoring-class-based-react-redux-to-hooks-ohg0d?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Refactoring class-based React Redux to Hooks"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

So that  would be a complete refactor from class-based React and Redux to functional React with Hooks. On the [first article](https://dev.to/santispavajeau/refactoring-apps-from-class-based-react-with-redux-to-functional-hooks-n58) we looked at `useState()` and `useEffect()` and on this article we looked at using `useDispatch()` and `useSelector()`. There are more Hooks, to further optimize applications but a working refactor can be possible with these four hooks.

[Visit an example of a refactor in my project](https://github.com/SantiagoSalazarPavajeau/react-projects/blob/master/src/App.js)

[Follow me on Twitter](https://twitter.com/santispavajeau)
[Connect with me on Linkedin](https://www.linkedin.com/in/santiago-salazar-pavajeau/)

## Checkout some of my projects:

* [A project management app or "Team Todos" in React](https://santiagosalazarpavajeau.github.io/react-projects/#/projects)
* [A music-jam app in Vanilla Javascript](santiagosalazarpavajeau.github.io/chords_beats_frontend/)
* [A community for fellow young fathers in Ruby on Rails](https://pure-island-81017.herokuapp.com/)
* [Algorithms](https://github.com/SantiagoSalazarPavajeau/coding_challenges)

Thank you for taking a look!




