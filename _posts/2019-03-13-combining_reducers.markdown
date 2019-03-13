---
layout: post
title:      "Combining Reducers"
date:       2019-03-13 22:52:11 +0000
permalink:  combining_reducers
---


I was recently working on the [CRUD lab](https://github.com/learn-co-students/crud-lab-v-000) in building a [Yelp](https://www.yelp.com/)-like application that uses [React](https://reactjs.org/) and [Redux](https://redux.js.org/) to add and delete restaurants and reviews. 

![app gif](https://media.giphy.com/media/g07ZDk5uiij7mLoKvB/giphy.gif)

While working my way through the lab I found that my reducer function, `manageRestaurants`, was dense. So, I naturally sought to split my giant reducer function into two children reducer functions so that each function was responsible for only one resource's state. Using [combineReducers](https://github.com/reduxjs/redux/blob/master/docs/api/combineReducers.md), I then combined the children reducers in one parent reducer function, `rootReducer`, which is what gets passed to the [store](https://redux.js.org/api/store). This not only made my code cleaner but also much easier to debug.

Finally, I got the app working in the browser just as the lab wanted and before I could take that big sigh of relief I found that the tests were failing. The lab just wanted us to create one reducer function and put all reducer logic in there. Ugh!

![office space](https://media.giphy.com/media/c453ypM8rqm1a/giphy.gif)

Regardless, I decided to create a separate [branch](https://github.com/kriti-rai/crud-lab-v-000/tree/combine-reducers) and push up my clean and amazing code there and reverted my `master` branch to the old way to pass the tests. In so doing, however, I realized that now I have a greater understanding of how `combineReducers` work. Additionally, now that I had seen both scenarios I could use that knowledge and experience to decide when I could use `combineReducers`. If you are just working with one or two resources, maybe you don't quite need to use this helper function. However, imagine a big app with multiple resources and soon enough you will find yourself tangled up in a number of [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) statements and a big, fat state with multiple key-value pairs.  

## Refactoring with combineReducers

All talks aside, lets look at my giant reducer `manageRestaurants` first, which is maintaining the state of both restaurants and reviews. 

![reducer](https://i.imgur.com/m8hAvrk.png?1)

Now, let's split our giant reducer into two child reducer functions, say `restaurantReducer` and `reviewReducer`. The former manages the state of the restaurants whereas the latter manages the state of the reviews. 

### restaurantReducer

![restaurantReducer](https://i.imgur.com/WciVwon.png?1)

### reviewReducer

![reviewReducer](https://i.imgur.com/2brKdeA.png?1)

Now,  here's our `rootReducer`, where we will call our children reducer functions. Notice, we imported `combineReducers` from `redux`.

### rootReducer
![rootReducers](https://i.imgur.com/nrx0Mr5.png?1)

This is equivalent to writing:

```
function rootReducer(state = {}, action) {
  return {
    restaurants: restaurantReducer(state.restaurants, action),
    reviews: reviewReducer(state.reviews, action),
  };
};

```

This basically produces the same giant reducer function as `manageRestaurants` but in a much more cleaner and abstract way. 

## Conclusion

If your app is big and has more than one or two resources, you might be better off splitting up the state object into slices and using a separate child reducer to operate on each slice of the state. The slices of state can then be combined using `combineReducers`, a helper utility lent by *Redux*, to create a parent reducer, conventionally named `rootReducer`.  Keep in mind that using `combineReducer` might not be helpful if one is intending to learn what is going under the hood as it abstracts the way reducers are being combined and working together. So, try to play around with both scenarios to get a better understanding of how reducers work and when to use `combineReducers`.

