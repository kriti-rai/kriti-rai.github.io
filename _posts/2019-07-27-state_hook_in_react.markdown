---
layout: post
title:      "State Hook in React"
date:       2019-07-27 20:38:14 -0400
permalink:  state_hook_in_react
---

For some time we had been referring to function components as *stateless* components and would have to write a `class` every time we needed to make use of a local state. However, with the introduction of hooks in React 16.8, one can now use the inbuilt hook called `useState` or otherwise called *State Hook* that lets one add local state to function components.

As per [React.js docs](https://reactjs.org/docs/hooks-state.html), 

> State Hooks are basically functions that let you ‘hook into’ React state and lifecycle features from function components

Let’s see how we can rewrite a class component using the state hook. Let's say we have a `Like` component that renders the number of total likes as well as a like button and an unlike button. When  a user clicks on the like button the likes go up by 1 and conversely, when a user clicks on the unlike button the likes go down by 1. 

Since our component needs to remember the number of likes to be able to update and display it, it will need to make use of *state*. 

Prior to the introduction of hooks we'd normally write a `class` in order to use *state*.

```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';

class Like extends Component {
  constructor(props) {
    super(props);
    this.state = { likes: 0 }
  }

  handleLike = (e) => {
    e.preventDefault();
    this.setState({ likes: this.state.likes + 1})
  }

  handleUnlike = (e) => {
    e.preventDefault();
    this.state.likes > 0 ? this.setState({ likes: this.state.likes - 1}): null;
  }

  render () {
    return (
      <div>
        <h4>Likes: { this.state.likes }</h4>
        <button style={{backgroundColor: '#99ccff'}} onClick={ this.handleLike }> Like </button>
        <button style={{backgroundColor: 'red'}} onClick={ this.handleUnlike }> Unlike </button>
      </div>
    )
  }
}

const el = <Like />

ReactDOM.render(el, document.getElementById('root'));

```

This would give us something like this:

![gif](https://media.giphy.com/media/JrYkYH2Jkyw58u9mJs/giphy.gif)

If we zero in on the snippet below, we see that we initialized the `likes`  state to 0 with this line `this.state = { likes: 0 }` in the constructor.

```
 constructor() {
    super();
    this.state = { likes: 0 }
  }

```

Now, with state hooks we can re-write the code above using `useState`.

```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

function Like() {
 const [likes, setLikes] = useState(0);
 ...
 ```

## What’s happening here? 

First, we imported `useState` from React. Then, we converted our class component to a function component `Like()`. Finally, inside the function we have this one liner:
```const [likes, setLikes] = useState(0);```
`useState` returns a pair of values -- the current state and a function that updates it. So, with the  [array destructuring method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring) we are declaring and assigning values to a state variable `likes` and a function `setLikes`, which is similar to `setState()` method in a `class`.  You can also see that `useState()` takes in one argument which is the initial state of the component and that'd be  `0` in this case as we haven't got likes from anyone yet :( 

## Updating State

Since, we have our hands on `setLikes` function that we declared above, we can now directly call the function to update the state. Let's re-write our handler functions `handleLike` and `handleUnlike`.

```
  const handleLike = (e) => {
    e.preventDefault();
    setLikes(likes + 1)
  }

  const handleUnlike = (e) => {
    e.preventDefault();
    likes > 0 ? setLikes(likes - 1): null;
  }

```

See, how we can easily call `setLikes` to update our `likes`? So, instead of writing `this.setState({ likes: this.state.likes + 1})` like we'd do in our `class` we can just write `setLikes(likes + 1)`.

Let's also update the `return` value of our function by replacing `{ this.handleLike }` and `{ this.handleUnlike }` with just  `{ handleLike }` and `{ handleUnlike }` , respectively. Finally, here's our `Like` component re-written using the state hook.
```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

function Like() {
  const [likes, setLikes] = useState(0);

  const handleUpClick = (e) => {
    e.preventDefault();
    setLikes(likes + 1)
  }

  const handleDownClick = (e) => {
    e.preventDefault();
    likes > 0 ? setLikes(likes - 1): null;
  }

  return (
    <div>
      <h4>Likes: { likes }</h4>
      <button style={{backgroundColor: '#99ccff'}} onClick={ handleUpClick }> Like </button>
      <button style={{backgroundColor: 'red'}} onClick={ handleDownClick }> Unlike </button>
    </div>
  )
}

const el = <Like />

ReactDOM.render(el, document.getElementById('root'));

```

So, here you go! With React hooks, function components can now have some state without you having to write those clunky classes. However, this does not mean that you have to go back and convert all your existing class components. And also, hooks are totally optional and there is no intent of them replacing classes. However, from now on you at least have the options to use hooks in case you need to use *state* inside your function components. Note that hooks come with React 16.8, so if you want to use them make sure to upgrade React and ReactDOM. 



