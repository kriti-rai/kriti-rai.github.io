---
layout: post
title:      "Understanding ORM"
date:       2018-05-01 23:12:57 -0400
permalink:  understanding_object_relational_mapping
---




> Object-relational mapping (ORM, O/RM, and O/R mapping tool) in computer science is a programming technique for converting data between incompatible type systems using object-oriented programming languages. This creates, in effect, a "virtual object database" that can be used from within the programming language.
> 
> *-Wikipedia*


In other words, ORM is a technique of wrapping a relational database using an object-oriented programming language such as Ruby, Python etc. The object  models are mapped to the database tables whereas the object instances are mapped to the rows in the tables. 

When it comes to database management, SQL is awesome but could easily get complex and tedious. OOP presents a good way to represent these complex queries through object models a.k.a. classes. And, by teaching these objects methods and behaviors, we can omit having to write the same or raw query over and over again, thereby reducing tediousness and chances of making an error(s).

Let me illustrate this with some examples step by step.

Let's say I already have my database connection set up in my config directory.

```
require 'sqlite3'
require_relative '../lib/dog'
 
DB = {:conn => SQLite3::Database.new("db/dogs.db")}
```
Here, I have required the *SQLite3* gem that will allow our Ruby program to interface with *SQLite3* database engine. Then, I have required our ruby file called *dog.rb* in the lib folder of the directory. And, lastly I have equated the constant *DB* to a hash representation of our connection to the database. Now, throughout our *dog.rb* file we can access the connection to the database by simply calling *DB[:conn]*.

Before looking into mapping tables and objects,  let's look at some raw SQL queries that we would use to manipulate data.

For example, I can **CREATE** a table in our database, writing the following query:

```
CREATE TABLE IF NOT EXISTS dogs (
 id INTEGER PRIMARY KEY,
 name TEXT,
 age INTEGER,
 breed TEXT
 );
 ```
 
And to  **INSERT** some information into our table, I'd write the following:
 
```
INSERT INTO dogs (name, age, breed) VALUES ('Luka', 4, 'Husky');
INSERT INTO dogs (name, age, breed) VALUES ('Jackie',10, 'German Shepherd');
INSERT INTO dogs (name, age, breed) VALUES ('Roxy', 11, 'Corgie');
```
I could also write a query asking the database to return us all the information about the lised dogs.

```
SELECT * FROM dogs
```
 This would return:
 ```
 id          name        age         breed
----------  ----------  ----------  ----------
1           Luka         4           Husky
2           Jackie      10           German Shepherd
3           Roxy        11           Corgie
```

Cool! But it's all fun and games until you have to create 50 tables and enter hundreds of records into each of them. That's not it! You also want to do things with the data -- you want to manipulate it, refine your search, maybe group it or arrange it in a certain order. All of that involve complex queries. And, having to write them repeatedly is time-consuming and error-prone. Would not it be easier if we map our database tables to objects and wrap all these queries into methods? Then, everytime we want to run those queries we'd just call the corresponding methods on the objects.

**MAPPING OUR DATABASE TABLE TO A CLASS**

So, why class? If you think of it, it makes sense that class would be equivalent to a table, as it holds information about its instances just like a table holds information about the items listed inside it. Hence, by that logic, it makes sense that we also map each record or row of the table to the instance of the class. Each column of the table then equates to the attribute of the instance.

![table-class](https://i.imgur.com/64YqrZJ.png)

Now that we have our class set up, let's look at an example of how we can create a method to wrap a SQL query inside it. Inserting records seems like a task that is repetitive and would make for a good example. As shown earlier, the SQL query to enter a record would be something like this:

```
"INSERT INTO dogs (name, age, breed) VALUES (dog_name, dog_age, dog_breed);"
```

Let's wrap this into a method called *save* as we are saving the information about the existance of an instance into the table.

```
def save
  sql = "INSERT INTO dogs (name, age, breed) VALUES (?, ?, ?)"
  DB[:conn].execute(sql, @name, @age, @breed)
  @id = DB[:conn].execute("SELECT FROM last_insert_rowid() FROM dogs")[0][0]
end
```
Don't worry about the technicality of the code but for our purposes focus on how the SQL query is wrapped inside the method. Now, everytime you want to insert a new row to the database table, we can simply call *.save* on an instance.

For example:

```
dog = Dog.new('Jamie', 15, 'Labrador')
dog.save #=> enters a new row in the table
```

Similarly, you can create other methods depending on your need, with underlying SQL queries, like the ones that find a dog(s) by name or by breed and so on. 

You might feel like it is an extra work to map classes and tables but to my understanding so far, ORM's appeal really shines when it comes to building a large-scale program where, say, you have 10 classes as opposed to 1. Then, maybe you might consider wrapping the database with OO language, and then further abstracting the methods so they can be shared throughout all 10 classes. This will not only cut down the amount of code, but will also make debugging, organization and update easier.

