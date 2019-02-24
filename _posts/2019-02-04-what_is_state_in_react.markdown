---
layout: post
title:      "What is State in React?"
date:       2019-02-04 16:23:52 -0500
permalink:  what_is_state_in_react
---


In English, ***state*** refers to "the particular condition that someone or something is in at a specific time" and that holds true for state in React Component as well. Each component can maintain its own state, which can be accessed through `this.state`. 

## **Why use state?**

If you have a static app, **don't use state**. However, if you want your app to be interactive, like for example a clock widget that shows and updates time at a set interval or an app where one can log in and out, add, delete and update resources - it will involve state. In other words, state allows components to incapsulate and modify information and keep track of the changes. 

**But, wait a minute don't we use props to store data in components?** Yes, but the crucial difference here is that *props* are immutable in that the components can not change their props as they are passed down from a parent component. In contrast, component has a full control over its state and can change it as needed. 

## **Action!**

Let's look at an example to see how *state* works.

We will build a  simple `Countdown` component that renders the final countdown to the New Year's day. Keep in mind,

> *state* is a feature only availabe in *classes*

So,  let's start by building an ES6 class for our component and write some pseudo code inside to show what it should do.

```
import React from 'react'
import ReactDOM from 'react-dom';

export default class Countdown extends React.Component {
   
 timer () {
 // some function that updates the  countdown every second
 }
	 
 render () {
  return ( 
  // shows the countdown 10 through 1 and renders the message HAPPY NEW YEAR!!
  )
 }
}

const element = <Countdown />

ReactDOM.render(element, document.getElementById('root'));
```
Now, in order to manipulate the state, you ought to have something to begin with, right? Yup, an **initial state.** So, let's do that - let's  declare the intial state of the component and give it an attribute of `secondsLeft`. Now, where's the best place to declare the initial state? Constructor function it is! Because that's what gets called first when the component class is created and that's where we should be initializing everthing including state. Let's add the following block inside our component class.

```
constructor() {
  super();
  this.state = { secondsLeft: 10}
}
```
Note that we are hardcoding the value for the initial state but we can also pass in *props* as an argument to the constructor function if we are going to use it to set the initial state.

Now, let's work on our `timer()` function that updates our component's state of `secondsLeft` until it hits 0. We use `this.setState()` to update a component's local state rather than directly setting the state to some value. This way React knows that there has been an update and re-renders the component.
	
```
timer = () => {
 if (this.state.secondsLeft > 0) {
  this.setState({ secondsLeft: this.state.secondsLeft - 1 })
 }
}
```

Notice that I used an arrow function above. This is so that we don't have to bind `this` when we call it. 

Moving on, let's add a lifecycle method `componentDidMount()` which will run after the component output has been rendered in the DOM. This is also a good place to call `timer()`.

```
componentDidMount() {
 setInterval(
  () => this.timer(),
   1000
   );
 }
	
```
	
Finally, here's our overall code:

```
import React from 'react';
import ReactDOM from 'react-dom';

export default class Countdown extends React.Component {
  constructor() {
    super();
    this.state = { secondsLeft: 10 }
  }

  componentDidMount() {
    setInterval(
      () => this.timer(),
      1000
    );
  }

 timer = () => {
  if (this.state.secondsLeft > 0) {
     this.setState({ secondsLeft: this.state.secondsLeft - 1 })
  }
 }

  render() {
    const message =  (this.state.secondsLeft === 0 )? <font color="red">Happy New Year!!!</font> : this.state.secondsLeft 
    return <h1>{ message }</h1>
  }
}

const el = <Countdown  />

ReactDOM.render(el, document.getElementById('root'));

```

...and, let's see it in action - tadah!!!

![](https://media.giphy.com/media/u0ag71wII1yBjMAOAc/giphy.gif)

## ***TL;DR***
* If you want interactive components use state
* React maintains state as an object which can be accessed through `this.state`
* State is similar to props, but is private and fully controlled by the component and can not be accessed and modified outside the component (*think encapsulation*)
* Don't set the state directly like `this.state = someValue` but use `setState()` instead


### Resources:

* [Props and State](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)
* [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)
* [Components and Props](https://reactjs.org/docs/components-and-props.html)
* [State and Lifecycle](https://reactjs.org/docs/state-and-lifecycle.html)
