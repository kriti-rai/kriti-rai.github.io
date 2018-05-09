---
layout: post
title:      "Being Lazy with Active Record"
date:       2018-05-09 19:44:11 -0400
permalink:  being_lazy_with_active_record
---

![Source: Imgur](https://i.imgur.com/pTR2jpI.jpg)

In my last [blog](http://icodeyounot.com/understanding_object_relational_mapping) I talked about ORM and how it wraps database into OO classes or models. Active Record is a form of ORM that works as a layer between your database and Ruby objects, lending lazy programmers a ton of built-in functionalities and methods that make building web applications smooth and easy.

Let's look at some of those said functionalities, starting with creating a class, say ```Dog```. 

```
class dog < ActiveRecord::Base
end
```
Here my class inherits from ActiveRecord::Base module. With this inheritance, my model ```Dog``` now is "magically" mapped to its corresponding database table ```dogs``` , if exists, where each column is mapped to the  corresponding attribute of the model. Let's say my table has four columns --*id*, *name, age* and *gender*. Now, my model would automatically inherit attributes corresponding to each of those columns. Hence, there is no need for me to define ```attr_accessors``` but my module would be aware of the reader and writer methods for each of its attributes. For instance, I can instantiate a dog and set its name through ```#name=``` methods and then ask for its name through ```#name``` method.

```
luka = Dog.new
luka.name = "Luka"
luka.name
#=> "Luka"
```

Furthermore, Active Record provides a bunch of other dynamic methods to retrieve and update the data to my model. These methods are called **CRUD** methods, which stands for ```create, read, update``` and ```delete```.
 
 **Create**
 
An instance of the class can be created calling either ```new``` or ```create```. While both methods return the instance of the object created, they slightly differ in the way they work. The ```new``` method only instantiates the object but does not persist it to the database wherease ```create``` does. In other words, if you want to commit the creation of an instance to the database after instantiating it using ```new``` you will have to ```save``` it afterwards. But, with ```create``` the object is automatically saved.

Using #new method:

```
dog = Dog.new
#only instantiates the object
dog.save
#to persist data
```
Using #create method :

```
dog = Dog.create
#instantiates and saves the object in the database
```

For both ```new``` and ```create```, attributes of the object being created can be passed in as a hash or a block as argument(s) or can be set manually after creation.

```
luka = Dog.create(name: "Luka", age = 2, gender = "M")
#using create
luka = Dog.new(name: "Luka", age = 2, gender = "M")
#using new

OR

Dog.create do |dog|
    dog.name = "Luka"
	dog.age = 2
	dog.gender = "M"
end

OR

dog = Dog.new
dog.name = "Luka"
dog.age = 2
dog.gender = "M"

```
 
 **Read**
 
 Active Record lends a rich collection of reader methods which come handy while retreiving data. For example, if we want to see *all* dogs in the table, we can simply call ```all``` method. There are also *finder* methods that can find the instance by *id* or attribute(s). For example, if I have to find a dog by the *id* 2, I'd simply type ```Dog.find(2)```. There are many more [here](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html) that you might find interesting and helpful in retreiving data.
 
 **Update**
 
Once an Active Record object has been retrieved using one of the finder methods, its attributes can be modified and saved in the database. For instance, I can ```find``` the dog named "Luka" and set its name to "Luke" through ```name=``` method and ```save``` it, thereby persisting the data.
 
 ```
 luka = Dog.find(name: "Luka")
 #finds Luka
 luka.name = "Luke"
 #sets Luka's name to Luke
 luka.save
 #updates Luka's record in the table
 ```
 
 **Delete**
 
 Similarly, once the instance has been retrieved, it can also be destroyed or deleted using the ```destroy``` method. For instance:
 
 ```
 luke = Dog.find(name: "Luke")
 luke.destroy
 #deletes Luke from the database
 
 or
 
 Dog.destroya_all
 #deletes all the dogs from the database
 ```
 
 **No such thing as magic**
 
This is just the basics of Active Record and its "magic". Once you get into associating your models and writing migrations, you will experience more of Active Record's magical side. However, all this that seems like magic is just a heavy use of metaprogramming. For instance, when we called ```all``` to find out all the instances in our class, there was simply an SQL query being fired which would be something like ``` SELECT * FROM dogs;```. Similarly, when we were searching for a dog by its *id* through ```find(some_id)```, we were simply evoking ```SELECT * FROM dogs  WHERE id = ?;```. But, I guess that is why Active Record is so appealing to programmers as it comes with easy-to-use and semantically sound methods for talking to the database without having to write complex and redundant SQL queries.
	 

