---
layout: post
title:      "Rails w/ JS project, at last!"
date:       2019-01-08 06:54:01 +0000
permalink:  rails_w_js_project_at_last
---


For this project, I worked with my rails app, [Outfits On the Go](https://github.com/kriti-rai/outfits-on-the-go-with-js), integrating JS to it. The app allows users to create boards on which they can pin (create) outfits and each outfit can be sorted by (hash)tags. To read about the app and its basic set up in Rails, [go here](https://github.com/kriti-rai/outfits-on-the-go).

![demo](https://media.giphy.com/media/i3ofY7z8yX9WcbdL7F/giphy.gif)

### Taking the first step

As JS section was challenging and took me a while to finish, I found myself going back and reviewing a lot of the lessons before starting the project. I started off by watching the video reviews by Avi. Still overwhelmed and confused, I decided to start small - I made a copy of the repo, updated its title and README.md to simply state that I will be integrating JS into the project. Simply doing that made me feel like I was going somewhere.

The next step was to install Active Model Serializer to generate serializers for my models. Seeing my models talk to me in JSON gave me that extra boost to keep going and now all I needed was to find ways to talk to the URL's to give me back that data so I could render them. Insert AJAX revision and debugger (LOADS AND LOADS of it). 

### Debugging

When it came to debugging, my very first thought was "*Oh God, I miss [pry](https://github.com/pry/pry)*". I tried to find ways to avoid using [Chrome Dev Tools —Debugger](https://developers.google.com/web/tools/chrome-devtools/javascript/) but *console.log*-ing everything was just tiresome and tedious, although it's a quick and easy way to check the value of a variable. As I begrudgingly resorted to using *debugger*, I quickly understood that the only reason I did not like it was because I did not understand it. With practice, I got more used to the tool and its features. The error messages are pretty informative to know what went wrong and when or where. If your code is not doing what you want it to do you can add breakpoints and drop in that moment in time to inspect the scope and the variables. Sometimes I would not find anything wrong my code, or so I would think, and yet the request would not seem to go through, in which case I found the [Chrome Dev Tools —Network Tab](https://developers.google.com/web/tools/chrome-devtools/network-performance/) helpful in checking the status code, headers, etc.

### Working with HTML Elements

As I worked my way through selecting elements to capture data and modifying the DOM, I realized the importance of adding `class` and/or `id` to the elements and the containers, so it is easier to target them. It also makes your life easier if you give those classes/id's more descriptive names. For example, say you have forms for creating and editing an object. Instead of giving them an `id` of `#form-1` and `#form-2`, how about `#new-form` and `#edit-form`? So when time comes to target the form to create a new object, you don't have to go scrolling through your HTML page to find the tag name or if you come across it you know what it's for.

I also found myself using [data-](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes) attributes a lot to store extra information inside the elements that could be extracted later where required. For example, I could have an attribute called `data-url` inside a button like this:

`<p><button data-url="/users/${user.id}/next" id="nextUser" class="button">Next User</button></p>`

Then, I can access the attribute, i.e. the url, later as such:

`this.dataset.url`

where `this` is the button element.

### The Real Fun
I decided to start off by working with *users* first followed by *boards* and then *outfits* as the flow demanded. For readability, I later separated out the functionalities that dealt with board and outfits, into their own files `boards.js` and `outfits.js`, respectively.
I started with the GET requests first leaving the more complicated POST, PATCH and DELETE for later. This meant working with controller actions such as `#index, #show, #new` and `#edit` first and leaving `#create, #update` and `#destroy` for later. I began by updating the controller actions to render data in JSON. From there on, the following was my action-plan:
* make a request
* get back data 
* hijack the event
* do something with the data

Once I got a fairly working app, it was all about DRY-ing up the code. There were couple of functions that I could make more dynamic which would allow me to re-use them throughout the resources or, say, I could just define a function `clear()` to clear up the *DOM* instead of calling `('some_selector').empty()` every time, and so on. I quickly learned that what I was doing was recognizing the pattern and it made me realize how important it is for both problem-solving and cleaning up the code.


### What  I Learned
Besides the much needed revision on the material, the project taught me a great deal about thinking big. With my simple rails app, it became evident that it seriously lacked in organization in the front-end aspect as soon as I integrated JS to it. I found myself going back and forth and adding a `class` or an `id` to an element in order to target it. Thus, I can only conclude that planning is essential and while doing so have the big picture in mind. It might take some time to sit down and plan out how you are going to structure your app, but imagine how much time  and headache it will save you in future if your app were to expand or change. 

Secondly, IMO debugging is a major part of problemsolving and, dare I say, holds a real life implication. Yes, you will find solutions on the internet that you can just copy-paste to fix the bug, but if you expect your code to work a certain way and and really want to figure out why it 's not doing so, get used to debugging. Again, think of it as real-life problems. There isn't always a hack for those. *What then?*

Lastly, the biggest takeaway is that any task no matter how big can be broken down into tiny bits - work your way up from there. I simply started by serializing my data and just seeing the data in JSON format gave me some hope and confidence. So in other words, begin with the simplest problem and move towards the more challenging one.

*Happy coding and keep learning!*

You can watch my walk-through video [here](https://youtu.be/I4HUxO_Z-Y4).


