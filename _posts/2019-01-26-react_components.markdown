---
layout: post
title:      "React Components"
date:       2019-01-26 05:55:37 +0000
permalink:  react_components
---

![](https://i.imgur.com/d5dUPiV.png?1)

React components are like JavaScript functions. They accept arbitrary inputs called *props*, short for properties, and return something that looks similar to HTML element.

They can be defined in two ways:

### 1. Using ES6 class
```
class Person extends React.Component {
 render () {
  return <p>Hi, my name is {this.props.name}.</p>;
 }
}
```
### 2. As a function
```
function Person(props) { 
 return <p>Hi, my name is {this.props.name}.</p>;
} 
```
		 
Now, we can render an element using `Person` component as following: 

```
const element = <Person name="Crusoe" />

ReactDOM.render(
 element,
 document.getElementById('root')
);
```
which renders `<p>Hi, my name is Crusoe</p>` element.

So, what's happening here? We passed in a value for the `name` attribute and `Person` component assigned it to the `name` key in the props. So, something like this happened:

```
props = {
   name : "Crusoe"
}
```

Similarly, if we were to write:

```
const element = <Person name="Crusoe" age="32" hometown="Highgate" />
```

This is what would happen:

```
props = {
  name : "Crusoe",
  age: 32,
  hometown: "Highgate"
}
```
Now, let's modify our `Person` component so it makes use of those additional attributes, `age` and `hometown` just so we can see how user-defined arguments are passed along via *props*.

```
class Person extends React.Component {
 render () {
  return (
   <p>
    Hi, my name is {this.props.name}. I am {this.props.age} years old and from  {this.props.hometown}
   </p>
  )
 }
}
```
So, now appending our `element` to the DOM would result in ``<p>Hi, my name is Crusoe. I am 32 years old and from Highgate.</p>`.

We can also call component(s) inside another component. For example, let's say we have an `App` component on the top of the view hierarchy of our application as it typically is. We can call  `Person` inside `App` as so:

```
class App extends React.Component {
 render () {
  return (
	 <div>
      <Person name="Crusoe" age="32" hometown="Highgate"/>
      <Person name="Tom" age="26 hometown="Bronx"/>
      <Person name="Ella" age="25" hometown="Minneapolis"/>
      <Person name="Leia" age="23" hometown="Savannah"/>
    </div>
  )
 }
}

ReactDOM.render(<App />, document.getElementById('root'));
	    
```
Now, on the page you would see:

```
Hi, my name is Crusoe. I am 32 years old and from Highgate.

Hi, my name is Tom. I am 26 years old and from Bronx.

Hi, my name is Ella. I am 25 years old and from Minneapolis.

Hi, my name is Leia. I am 23 years old and from Savannah.
```

Isn't this cool? Components are reusable and with their ability to take in arguments are dynamic and powerful. 

However, (*uh oh!*) what happens when no argument is provided and/or the property attribute is not defined? Like in the following case:

```
<Person name="Crusoe" age="32"/>
```

This would render `<p>Hi, my name is  Crusoe. I am 32 years old and from .` And, we don't want that. We want to be prepared for scenarious when data is missing or undefined.

### *Enter default props!!!*

We add `defaultProps` property to the component so that whenever a *prop* is missing or undefined, the component uses the default value provided in `defaultProps` instead. For example, to fix the issue above we can do this:

```
class Person extends Component {
  render() {
    return (
         <p>Hi, my name is {this.props.name}. I am {this.props.age} years old and from {this.props.hometown}.</p>
    );
  }
}

Person.defaultProps = {
  hometown: 'XYZ'
}

```

This would result in `<p>Hi, my name is  Crusoe. I am 32 years old and from XYZ .`

## Conclusion

React components are basically JavaScript functions and can be defined using ES6 class. They contain:
* props, *short for properties*
* data
* state (*not discussed here*)
* markup (*delivered by `render()`*)

What make components  powerful and dynamic is their reusability and their ability to take in user-defined arguments. However, in case an argument is missing or not defined, the resulting element might look funky. In order to prevent scenarios such as that, it's best practice to define`defaultProps` setting default value(s) for the component's *prop(s)*. should be used. 


