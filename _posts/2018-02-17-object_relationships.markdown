---
layout: post
title:      "Object relationships"
date:       2018-02-17 23:36:00 -0500
permalink:  object_relationships
---

I recently finished the chapter on object relationships in Ruby, a very important concept in understanding Object Oriented Programming in my opinion. In my previous post, I talked about creating objects, classifying them and assigning them attributes and methods to make them behave certain ways. But apparently, that was just a tip of the iceberg and it took me a couple of labs and frustrating nights to wrap my head around the concept of object relationships. I am not saying I have excelled in understanding the concept, but here is what I have understood so far.

Let's start by revisiting the ubiquitous quote in Ruby-land -- *"everything in Ruby is an object"*. These objects possess attributes and methods that make them behave in certain ways. Shovelling in an instance of a class into an array is not the same as shovelling in the name of that instance. Likewise, these objects can also talk to each other and have relationships with one another. Whaaa......?

**1. Has many**

An object can have many objects of another type. For example, an artist (an instance of a class, Artist)  has many songs (a collection of an instance of Song class). The *:songs* attribute in the code snippet below demonstrates the relationship between an artist and its has-many relationship with songs.
 
 ```
class Artist
    attr_accessor :name, :songs
    def initialize(name)
      @name = name
      @songs = []         
    end
end
 
```
Then later with a little help of other methods and adjustments we can simply ask the artist for its songs.
```
bonobo = Artist.new("Bonobo")
bonobo.songs  #=> gives an array of Bonobo's songs.
```


**2. Belongs to**

Now, an artists has many songs but a song can belong to only one artist (given that we are talking about single-artist songs). So to establish that belongs-to relationship we give Song an *:artist* attribute.

```
class Song
    attr_accessor :name, :artist
    def initialize(name)
	    @name = name
			@artist = artist
	end
		
end
```
Like for artists, we would also be able to ask an instance of Song for its artist. Again, we will need some helper methods  but we would be able to ask a song for its artist as follows:
```
song.artist  #=> returns an artist with all the attached attributes
```

**3. Has many through**

Now for the fun part, an object can also have a has-many relationship with other objects through the objects it already has a primary relationship with. Working with the above artist-song example, we know that a song belongs to a genre and an artist. Hence, the artist of that song automatically gets a genre through the song. Since an artist has many songs, it therefore has many genres through its songs. So, Genre class gets both *:songs* and *:artists* as attributes.

```
class Genre
  attr_accessor :name, :songs, :artists

  def initialize(name)
    @name = name
		@artists = []
		@songs = []
  end
	
end
```
Again, with the help of some methods and adding a *:genres* attribute to Artist we would simply be able to ask an artist for its genres, a song for its genre and ask the genre for the artists and songs listed inside it. Pretty cool!!!





