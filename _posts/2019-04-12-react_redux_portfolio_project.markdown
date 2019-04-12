---
layout: post
title:      "React/Redux portfolio project"
date:       2019-04-12 06:35:52 +0000
permalink:  react_redux_portfolio_project
---


Finally, the day has come!! I'm getting ready to submit my final project for assessment and phew! what a journey it has been -- the whole program and the project like all other ones. So, let's talk about my final project. For this assignment, I built an app called ***[Trailista](https://github.com/kriti-rai/trailista)***, which pulls a list of trails from [The Hiking Project Data API](https://www.hikingproject.com/data) by location. 

It's definitely a WIP but so far here are the available featues:

* ###  One can search for trails by city and country

  ![search](https://media.giphy.com/media/VGhqaaqcnNNfghL4P3/giphy.gif)

* ### One can login or sign up

  ![registration](https://media.giphy.com/media/IdgJsLzXBbQza0nbdW/giphy.gif)
		
* ### One can add trails to their favorites list and then delete them as they wish

     ![favorites](https://media.giphy.com/media/XcRAxM6W0AM2h3mOXb/giphy.gif)
	 
## The way it works

* My app pulls trails from *The Hiking Project API* endpoint that looks something like this

> www.hikingproject.com/data/get-trails?lat=40.0274&lon=-105.2519&maxDistance=10&key=APP_KEY

* The client supplies the values for *lat*, *lon* and *maxDistance* alongside a *key* that is provided by *The Hiking Project*

* The app then takes the user inputs and plug them into the `fetch` request and asks for a response back

It is pretty straighforward except that the user supplies the app with an address, e.g. a place's name, whereas *The Hiking Project* API endpoint wants the address in the form of GPS coordinates, i.e. the lattitude and the logitude. So I had to find a way to convert the location input to GPS coordinates. After rummaging through *The Hiking Project*' FAQs forum for some time, I  found a thread where someone had mentioned using *[MapBox](https://docs.mapbox.com/api/search/#geocoding)* to carry out, what they apparently call, "forward geo-coding", i.e. the process of converting an address to coordinates (the opposite would be *reverse geo-coding*). I also looked into [Google maps API](https://developers.google.com/places/web-service/intro) but apparently they charge? So, I decided to stick with MapBox.

And, boy was this fun? Yes, it involved some hair-pulling episodes but I'm fairly proud of this part the way I got it working. My two functions the heroes of my app, `getCoordinates()` and `fetchHikes()`, work in coordination to make this happen. 


- `getCoordinates()`  takes the user input and sends a `fetch` request to the *MapBox* API, like so:

   ```
getCoordinates= (e) => { 
    e.preventDefault();
    axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${this.state.city}.json?country=${this.state.countryCode}&access_token=APP_KEY`)
      .then(resp => {
        this.setState({
          longitude: resp.data.features[0].geometry.coordinates[0],
          latitude: resp.data.features[0].geometry.coordinates[1]
        })
				...
				```

- The response data is then used to set the values of `longitude` and `latitude` in the state, which is then used to make a `fetch` request to *The Hiking Project's* API via the dispatch action `fetchHikes()`

  ```
getCoordinates= (e) => {
...
   this.props.fetchHikes(this.state.latitude, this.state.longitude, this.state.maxDistance, this.state.maxResults)
  })
}
```

*Disclaimer: I am aware that this is not a requirement for the project but I felt like without the location feature my app would be very dull and I wanted to make it interesting or at least do something besides the simple login/logout and CRUD stuff.*

## The Set Up

This [article](https://www.fullstackreact.com/articles/how-to-get-create-react-app-to-work-with-your-rails-api/) that is included in the project instructions does a great job of guiding one through setting up a React app with Rails API. I had a rough idea of what resources I will be working with so I started with my models and controllers first and then set up (API) routes (in *'/config/routes.rb'*) to which I could send `fetch` or `post` request. I then moved onto my *client* side, i.e. the front-end part. Also, before pulling data from *The Hiking Project*, I created some seed data to test how the data is rendered on my front-end.

### Routes
I set up my routes in `App.js`. One interesting thing I found on the internet was that I could create a helper `PrivateRoute` to lock down a route if one is not logged in, like e.g. my '*/user*' route. This [article](https://reacttraining.com/react-router/web/example/auth-workflow) talks about how one can set up a `PrivateRoute`.

### Components

I started with presentational components that were mostly stateless and had DOM markups. As I progressed further I ended up adding props, state, connecting to the store and so on, which all led me to think about dividing up my components based on their functionality - containers and presentational. Containers are concerned with *how things work*, contain logic and the most of the times are stateful. The latter, on the other hand, are just "dumb" components that are concerned with *how things look* and has no logic or knowledge of state whatsoever. 

However, as Dan Abramov points out in this [article](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0), this pattern should not be enforced as a hard rule and if there is no necessity. So, you may see that some of my components act as both container and presentational or some of my presentational components have logic inside them and are not purely presentational. 

### Reducers

Speaking of separation of concerns, instead of having one giant reducer, I decided to have one reducer managing only one independent slice of the state.  So, I ended up with three reducers, `alertsReducer`, `hikesReducer` and `userReducer`. They are then all combined in the `rootReducer`, using `combineReducers` helper function provided by `redux`, which combines all the reducer functions into one reducer and passes it to `createStore`. 
# Conclusion

I had a lot of fun building this application. And, like with all other projects, this one came with frustration but that's what makes the accomplishment at the end so much sweeter. However, this is not a full accomplishment as I still would like to work on the app, adding some  more features like comments, users being able to rate and create hikes and so on. For all I know, sky is the limit and there is always room for improvement. As of now, I'm happy that I'm ready for my final assessment. The journey has just begun! 

