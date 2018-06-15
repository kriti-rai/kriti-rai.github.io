---
layout: post
title:      "WanderList - My Sinatra Project"
date:       2018-06-15 04:22:43 +0000
permalink:  wanderlist_-_my_sinatra_project
---



The idea behind my [second project](https://github.com/kriti-rai/wander-list) is to allow users to create boards where they can pin trips from the list that the app scrapes from [here](http://www.bbc.com/travel/destinations).

![homepage](https://i.imgur.com/0V0bz9H.png?3)

**Set-up**

I started out by creating a repository in github and then cloned it in my IDE terminal. Following what we had seen in the previous labs and lessons, I then created *Gemfile* with the dependencies my app would require, followed by *Rakefile*, *config.ru*, *README.md* and *spec.md* in the root of the directory. Next, I created an *app* folder for my MVC,  a *config* folder for my environment and a *db* folder for migrations. Way later, I decided to also created a *lib* folder where my scraper file would reside. 


**Models**

 I had four models for my app -- *User, Trip , Board *and *BoardTrip*. The associations between my models were such that:
 
 1. A user **has many** boards and **has many** trips **through** boards
 2. A board **belongs to** a user, **has many** board_trips and **has many** trips **through** board_trips 
 3. A trip **has many** board_trips, **has many** boards **through** board_trips, **has many** users **through** boards
 4. A boardtrip **belongs to** a trip and also **belongs to** a board

I then wrote migrations to create corresponding tables for each of my models alongside a join table for the many-to-many relationship between Board and Trip.  So, I had tables named *users*, *boards*, *trips* and *board_trips*.

**Controllers**

I had four controllers for my app each with their own corresponding *erb* files in the **view** folder.

1. **ApplicationController < Sinatra::Base**
    - The main Controller class with enabled session and route for the homepage.
    
2. **UserController < ApplicationController**
    - Controller class containing the routes for user login/logout, registration, index and individual user's page.
    
3. **BoardController < ApplicationController**
    - Controller class containing the routes for creating, editing, deleting and showing boards.
    
4. **TripController < ApplicationController**
   - Controller class containing the route to view all trips.

**Validations**

I had few validations to prevent bad inputs.

1. **User(s) can't sign up unless they fill out all required fields, i.e. username,     
      email and password**

      The above requirement was met by putting the following line of codes in *User*   
			class:

      ```
      validates :username, :email, :password, presence: true
      validates :username, uniqueness: { case_sensitive: false }
      validates :email, uniqueness: true
      ```
      The following lines of code in *UserController* raises an error if the above    
			validation fails:
			
     ```	    
     post '/signup do
     ...
     elsif !!params[:username].empty? || !!params[:email].empty? ||        
		    !!params[:password].empty?
        flash[:message] = "All fields are required."
        redirect to '/signup'
     end
     ```
2. **Usernames and email addresses are unique** 

     The *uniqueness* part in the validation code above also makes sure that  the        
		 usernames and email addresses are unique and if not raises an error.

     ```
     else
        flash[:message] = @user.errors.full_messages[0]
        redirect to '/signup'
     end
     ```

3. **User(s) cannot create a board without a name**

     Similarly, I added the following lines of code in *Board* class to make sure that one      can not create a board without a name. 

     ```
     validates :name, presence: true
     ```
     Then, I added the following condition in `post '/boards` route in *BoardController*:

     ```
     if !@board.save
     flash[:message] = @board.errors.full_messages[0]
     redirect to '/boards/new'
     ...
     ```

**Conclusion**
 
This project was not as intimidating to start as was my first project. I had the confidence to start with a clean directory and without tests to guide me. I also told myself that I would not take more than a week to finish this and setting up a deadline for myself sort of helped. Drawing out what I wanted my controllers and forms to do helped me visualize how my app should work and therefore helped me write the code accordingly. Overall, the sinatra section of the course was a big chunk to swallow and at times I felt like I was moving too fast without taking a moment to make sure I had retained the material. However, the last few labs and the portfolio project really pushed me to go back and revise and use up online tools to reach my goal. 


Here's the  [link](https://www.youtube.com/watch?v=VMWHzbSXqKQ&t=3s) to my video walkthrough of the app if you'd like to check out.
