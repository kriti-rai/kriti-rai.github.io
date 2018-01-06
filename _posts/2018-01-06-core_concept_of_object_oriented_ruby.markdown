---
layout: post
title:      "Core Concept of Object Oriented Ruby"
date:       2018-01-06 01:44:21 -0500
permalink:  core_concept_of_object_oriented_ruby
---

*Everything is an object in Ruby*. 

Just like real-life objects, Ruby objects can have atributes (instance variables) and behaviors (methods). We can talk to these objects and make them behave certain ways that is characteristic to them. Let's say we create an object, dog. We do not want to repeat our code every time we create a new dog though, so we start by creating a class (type), *Dog*, that has certain characteristics as a real-life dog would, like a name, gender, breed and age, and maybe also the ability to bark. 

We do this as follows:
```
class Dog

   attr_accessor :name, :gender, :breed, :age, :bark

end
```
Now that we have these attributes or, in other words, instance variables set up we can pass them into methods such that every time we call these methods our dog behaves in ways defined by the code inside the methods. Unlike local variables, these instance variables a.k.a attributes live in the object itself and can be used throughout the scope of the object. They can be used in several methods without having to create unique variables for each method and passing them as  arguments. For example, let's define a method to create a new object in Dog class and see how instance variables (preceded by '@') are passed in.

```
def initialize(name)
    @name = name
end

def name
    @name
end		

def name=new_name
    @name = new_name
end
```
In the above example, we defined three methods *#initialize*, *#name and *#name=*. *#initialize* is an interesting concept in that it is automatically called the instant a new object in that class is born  by the *#new* method with a name passed in as an argument. We do not see the #new method defined anywhere in the body fo the class but we can still call it because another cool thing about Ruby objects is that they are born with certain methods that are characterisitic of their type. In other words, every class in Ruby is a subclass and the object inside it "inherits" methods from the Object class. For example, say you have an object that is a string. So, this object inherits all the methods from the Object class in Ruby and can do things that all other strings can do, like *.inspect, .upcase, .downcase,* and so on.

Anyways, going back to instantiating a new dog, here is an example: 

```
shiba = Dog.new("Shiba")
=>#<Dog:0x0055adebd05cc0 @name="Shiba">  

shiba.name
=>"Shiba"

shiba.name="Tommy"
=>#<Dog:0x00559687c8bc98 @name="Tommy">
```


The second line of the example above shows that a new dog named "Shiba" has been created. We can then ask the object for its name and it returns "Shiba", just like in real life. Pretty cool! This demonstrates how information are stored in objects and we have methods that when called the objects disclose the information. However, not all information stored in an object are publicly accessible or can be written over. In the above example, name has both attribute reader(*#name*) and attribute writer (*#name=*). Hence, we can call #name on *shiba* and ask for its name and through #name= change its name to "Tommy" later. However, say we have a method for the dog's breed, then what? By common sense, one can not change the dog's breed.  In that case, breed would just have an attribute reader,* #breed*, but not #breed= to change the breed. Similarly, say you have a secret password coded inside Shiba that allows him to let you iniside the house. Then, we don't want a home intruder to just be able to ask Shiba for the password and chane it or get into your house. In that case, the attribute would just have an attribute writer method.

Hence, this concept of Ruby objects encapsulating data and knowledge unique to the objects themselves, some publicly accessible and some not, is somewhat metaphorical to our real lives. We hold information, experiences and knowledge and they are all different; some we expose publicly and some we choose to hold onto; and, this is what creates our unique identity in public.
     




