---
layout: post
title:      "Rails Project - Outfits On The Go!"
date:       2018-08-14 23:07:45 -0400
permalink:  rails_project_-_outfits_on_the_go
---


For this project, I decided to build something similar to my Sinatra project but of course with some Rails "magic" and the help of few awesome gems. The app is called [Outfits On The Go](https://github.com/kriti-rai/outfits-on-the-go), with the idea being that a user can log in and create an event-specific board, say *"The Bahamas Trip"* and post images of outfits they have put together or plan to wear for that occasion. 

I started out with this line of code: 

`$ rails g new outfits-on-the-go`

From there on, I built my models  -*User*, *Board* and *Outfit*, the corresponding controllers alongside *SessionsController*, migrations and routes. My model-associations were set up such that:

*1. A user has many boards and has many outfits, through boards*

*2. A board belongs to a user and has many outfits*
		
*3. An outfit belongs to a user and a board*

Given the association, you can tell that my *outfits* table is the join table, as it has two foreign keys.
 
As for my routes, I shallow-nested the main resources as such:

```
resources :users do
    resources :boards , shallow: true do
      resources :outfits, shallow: true
    end
end
```

The nesting somewhat violates the rule -  *Resources should never be nested more than 1 level deep*. However, since I'm using *shallow nesting* and only have two resources nested under one, the paths generated do not get that crazy. Only the collection actions, such as #index, of the nested resource  are scoped under the parent whereas the member actions, such as #edit, are not.  For example,*boards#index* routes to *user_boards_path ('/users/:user_id/boards')* and *boards#edit* routes to *edit_board_path ('/boards/:id/edit')*.

Finally, it was time to test out if my code was doing what I inteded it to do. *Enter views*. For views, I simply created *new* and *edit* first and proceeded to create others as needed. Any opportunity I saw, I used partials ( *love them!!*). In so doing, I figured that I could pass in urls and methods as *locals*. For instance, my *boards#edit* and *boards#new* actions routed to different paths and methods and that needed to be specified to the view, so I passed them in as values for keys - *:url* and *:method* as shown below:


```
# app/views/boards/_form.html.erb

<div class="login-form">
  <%= form_for board, url: url, method: method do |f| %>
  Name: <%=f.text_field :name %><br>
  <%= f.hidden_field :user_id, value: current_user.id %>
  <br>
  <%= f.submit %>
<% end %>
</div>

#app/views/boards/edit.html.erb

<h1>Edit Board</h1>

<%= render partial: 'form', locals: {board: @board, url: board_path(@board), method: "patch"} %>

#app/views/boards/new.html.erb

h1>Create Board</h1>
<%= render partial: 'form', locals: {board: @board, url: user_boards_path, method: "post"} %>
```

One big (and an important) part of my app is that a user gets to post pictures of their outfits. As I had not worked with any media upload feature in the past projects and labs, I was a little unsure but thanks to late-night, frantic googling sesh. I landed on a gem called [Carrierwave](https://github.com/carrierwaveuploader/carrierwave) that provides a simple way to upload files for Ruby apps. I also installed  [minimagick](https://github.com/minimagick/minimagick) to help with resizing images that were uploaded. 

Another really cool gem I found was [simple_hashtag](https://github.com/ralovely/simple_hashtag). At first I hesitated to use it as there were no commits in the last five years but as someone pointed it out it seems like the gem is so simple that it probably did not need much maintenance. So, I decided to give it a try. Now, I can look up outfits by hashtags. I, however, have not been able to do so if I click on the hashtag present on an outfit and rather have to manually type the url (*'/hashtags/:hashtag'*). This is something I plan to look into further and would be really cool to implement.

Needless to say, like all past portfolio projects this involved a lot of going back and forth, guessing and testing, adding and deleting code, procrastinating, *binding.pry*, *raise params.inspect*, and a lot of googling. To be honest, though, I am starting to enjoy the process. And, since I can't stop raving about Google, one more thing I want to mention here is that -- *"No matter what problem you have, someone has probably solved it. Find that person/solution* (not sure who said it, google it!).

[demo](https://www.youtube.com/watch?v=oGe6OHePVyc&t=1s)


