---
layout: post
title:      "Rendering Partials "
date:       2018-07-24 23:00:01 -0400
permalink:  rendering_partials
---


As developers we should always aspire to keep our code DRY, that is assuming that your code works in the first place. Apparently, there is  a code refactoring rule of thumb called [Rule of three](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)) that states that whenever you catch yourself repeating the same lines of code three times, you should be wrapping it up in a method. The same rule is applicable to forms. 

With every CRUD-based app, especially, we are consistently building forms to *create*, *edit* and *show*. Most of the time *new* and *edit* forms look alike and sometimes you might have a chunk of code in one view file that you want to share in three other view files. How do we do that without repeating the code?

**Enter Partials**

Let's take an example of a blog app that has posts and authors. Here we have a form to submit a new post.

```
#app/views/posts/new.html.erb

<h1>New Post</h1>

<%= form_for @post do |f| %>
  <%=f.label :title %>
	<%=f.text_field :title %>
	<%=f.label :content %>
	<%=f.text_area :content %>
	<%=f.submit %>
<% end %>
```
And, here is my *edit* form.

```
#app/views/posts/edit.html.erb

<h1>Edit Post</h1>

<%= form_for @post do |f| %>
  <%=f.label :title %>
	<%=f.text_field :title %>
	<%=f.label :content %>
	<%=f.text_area :content %>
	<%=f.submit %>
<% end %>
```
You notice that both forms look exactly the same except for the title wrapped in `<h1>` tags. 

**Rendering Partials**

So, in this scenario we could create a partial form *app/views/posts/_form.html.erb*. By convention, the name of the form is prefixed with an underscore ( _ ) and can be named anything, but for now we'll name it "*form*". We can then move the repetitive code from *new* and *edit* forms to our partial.

```
#app/views/posts/_form.html.erb

<%= form_for @post do |f| %>
  <%=f.label :title %>
	<%=f.text_field :title %>
	<%=f.label :content %>
	<%=f.text_area :content %>
	
	<%=f.submit %>
<% end %>
```
Now, let's revise our *new* and *edit* forms.

```
#app/views/posts/new.html.erb

<h1> New Post </h1>

<%= render 'form' %>
```

```
#app/views/posts/new.html.erb

<h1>Edit Post </h1>

<%= render 'form' %>
```

Notice how we replaced lines of code with a single line -- ```<%= render 'form' %>```? Not only do the forms look much cleaner, they are also much easier to maintain. Say in future we want to change something in the code that's being shared by the two forms. We will only have to update the partial.

**Rendering Partials from another directory**

The partial you want to render does not need to be in the same directory as the form. You will just have to refer to it slightly differently -- include the parent directory alongside the form's name. For instance, say we have a partial *author* to show an author's information.

```
#app/views/authors/_author.html.erb

<ul>
  <li> @author.name </li>
  <li> @author.biography</li>
</ul
```
We can render this partial in a post's show page to show it's author's info as follows:


```
#app/views/posts/post.html.erb

<h1>@post.title</h1>
<p>@post.content</p>
<%= render 'authors/author' %>
``````

Simple!

**Rendering Collections**

While dealing with collections, there's an easy way to render partials. Let's look at our author's *index *page that needs to iterate over a collection (i.e. @authors) to show each author's info. We can simply write: 

```
*app/views/authors/index.html.erb*

<h1>Authors</h1>

<%= render partial: 'author', collection: @authors %>

```
This will run the code inside the partial for each item in the collection.


**Passing Local Variables**

Partials become more powerful when used with local variables which is equivalent of passing arguments in methods. 

Let's work with our *author* partial again, but this time we'll replace `@author` with a local variable named`author`, like this:

```
#app/views/authors/_author.html.erb

<ul>
  <li> author.name </li>
  <li> author.biography</li>
</ul>
```
Now, let's render the partial in authors' *index* and show page as we did above, but only now we will be declaring a value for the local variable, namely `author`, in each case.

```
*app/views/authors/index.html.erb*

<h1>Authors</h1>

<% @authors.each do |a| %>
  <%= render partial: 'author', locals: {author: a} %>
<% end %>
```

```
*app/views/posts/show.html.erb*

<h1>@post.title</h1>

<p><%= @post.content %>

<h6> render partial: 'authors/author', locals: {author: @post.author} %>
```

You see what we did here? We are passing in a local variable named  *author* to the partial which points to the value of *a* in the *index* page and *@post.author* in the post's *show* page. By passing in locals, our partial now has become much more flexible. Very cool!




