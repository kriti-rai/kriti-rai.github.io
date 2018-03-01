---
layout: post
title:      "Let's tap in!"
date:       2018-03-01 00:16:40 +0000
permalink:  lets_tap_in
---


Just got done with my Music Library CLI project. After submitting my code, I went onto see how other students had solved the lab. I always try to see what many different ways something can be done. And, after checking out a few of the students' work, I saw that a couple of them had used  ```tap```. I got intrigued as the method seemed to make the code shorter and more elegant. So I ended up doing a little research. Let's tap in (sorry for the cheesy pun but I had to do it)!!

![tap_clipart](http://www.clker.com/cliparts/1/o/n/b/h/j/sun-faucet-md.png)



**What does it do?**

The ```tap``` method yields the object to a block and returns the object itself .In Ruby, it is coded as follows: 
```
class Object
  def tap
	yield self
	self
	end
end
	 
```

**Why is it useful?**

Per [Ruby Doc](https://ruby-doc.org/core-2.5.0/Object.html#method-i-tap), the primary purpose of the ```tap``` method is to "tap into" a method chain, in order to perform operations on intermediate results within the chain. Let's look at the example Ruby Doc provides:

Say you have a line of code like this. With methods chained like this, it is easy to have a bug and now you need to debug the code.
``` 
(1..10).to_a.select {|x| x.even? }.map {|x| x*x }
```
You can use `pry` but that is repetitive and tedious (and we are lazy). So, let's look at how we can use `tap` to debug it.
```
(1..10).tap {|x| puts "original: #{x}" }    #=>original: 1..10
  .to_a.tap {|x| puts "array:    #{x}" }    #=>array:    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .select {|x| x.even? } .tap {|x| puts "evens:    #{x}" }    #=>evens:    [2, 4, 6, 8, 10]
  .map {|x| x*x } .tap {|x| puts "squares:  #{x}" }    #=>squares:  [4, 16, 36, 64, 100]
	
=> [4, 16, 36, 64, 100]   #return
```

Clearly, with `tap` we were instantly able to see what each method in the chain does, making it easier for us to debug the line of code.

Besides debugging method chains, `tap` also cuts out extra, dangling variables, making the code look more elegant and readable. Let's look at one of the Genre or Artist class methods in the Music Library labs, ```self.create_by_name(name)```.

The traditional and the way I did it:

```
def self.create_by_name(name)
    obj = self.new
    obj.name = name
	obj
 end
end
 ```
 Using `tap`:
 ```
 def class.create_by_name(name)
  self.new.tap do {|obj| obj.name = name}
 end

```
Notice how elegant the second, one-liner method is? From now on, I will definitely be keeping an eye out for situations like this where I need to do something with an object and return the object itself and see if I can use `tap`. This method sure goes into my "sugar" toolkit box.
