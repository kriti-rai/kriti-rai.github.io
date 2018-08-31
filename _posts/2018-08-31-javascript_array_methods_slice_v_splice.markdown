---
layout: post
title:      "JavaScript Array Methods: slice() v splice()"
date:       2018-08-31 12:52:39 -0400
permalink:  javascript_array_methods_slice_v_splice
---

# SLICE
Slice is a way of selecting a chunk of elements in an array without modifying the original array. 

Let's say, for example, you go to a pizza joint and see a large pizza on display. 

![whole pizza](https://i.imgur.com/bvqiQ2P.jpg?1)

This pizza in particular has six slices, each slice named 1 through 6, consecutively. So, basically we we are looking at an array  like `myPizza = [1, 2 , 3, 4, 5, 6]` or `["Slice 1", ..."Slice 2"]`, where each element represents a slice. The numbers written outside the pizza drawing represent the corresponding index for each slice. For example, our first slice is at index 0, the second one is at index 1...and so on. Now, with `slice()` tool at your disposal you can apply it on this collection of pizza slices, i.e. *myPizza*, to obtain a collection of select slices. 

## 1. SLICE()

When you call slice() on an array without any arguments it basically returns a clone of the array. Not much fun there! So, with our pizza if you call `slice()` on it, it's basically going to return a whole pizza exactly like the original one. *( Please note the highlight in the pizza on the left is to show that all slices were selected.)*

![whole pizza](https://i.imgur.com/SAv4jQV.jpg?1)

## 2. SLICE(begin)

Now, things start getting interesting. When you provide `slice()` with an argument i.e. a valid index in the array, slicing begins from the given index returning the elements starting at the given index through the end of the array. For example, in our pizza below we call `slice(2)` and it returns us all the slices from 3 through 6.

![slice(2)](https://i.imgur.com/rYqk1kp.jpg)

*Note: the pizza being returned is the modified version of the clone of the original. So, the original pizza remains untouched, i.e. it still has all 6 slices intact.*

## 3. slice(begin,end)

Now let's spice things up tad bit more. What if you want to begin slicing at an index and want to stop at another, which is not the last index? You can then provide a second argument, i.e. to tell `slice` when to stop. Check out the image of our pizza below, when we call `slice(2,5)`:

![slice(2,5)](https://i.imgur.com/iV9DAxj.jpg)

*Note: slice(2,5) returns slices 3 i.e. at index 2 through 5 i.e. at index 4. So, to conclude, the element at the ending index (in this case slice 6) is not included in the resulting array.*

## 4. NEGATIVE INDEX

For a moment, let's forget how many slices there are in `myPizza` and we want to return all the slices starting at the second slice through the one right before the last one. You can do that with `myPizza.slice(1, myPizza.length -1)` but that just looks verbose and not so pretty. Fortunately, we have a nicer way to write that: 

```
> myPizza.slice(1, -1)
// => [2, 3, 4, 5]
```

![slice(1,-1)](https://i.imgur.com/b8LTrY0.jpg)

Similary, say we only want the last two slices, i.e. `[5,6]`. We can do so by writing `myPizza.slice(-2)`.



# SPLICE
Although it sounds similar to `slice()`, `splice()` is destructive in that  it modifies the original array. Also, it can be used to add and remove elements in an array. Let's look at the different ways we can use `splice()`. Unfortunately, no more pizza examples on this one (darn!).

## 1. SPLICE(arg1)

This will destructively remove a chunk of array beginning at the provided 'start' index and continuing to the end of the array. In this case, we can agree that `splice()` works like `slice()` besides the destructive part. For example, check out the code below:
		
```
> var arr = [ 'a', 'e', 'i', 'o', 'u']
> arr.splice(3)
// => ["o", "u"]   
		
> arr    
// => ["a", "e", "i"]      // Notice the original array (arr) has been modified
```
*Note: If arg1 is NaN, it is treated as if it were 0. If it is greater than the array’s length, it will use the array’s length.*		

## 2. SPLICE(arg1, deleteCount)

The second argument, i.e. *deleteCount*, is the number of items to be removed at the position stated by *arg1*. For example:

```
> var arr = [ 'a', 'e', 'i', 'o', 'u']
> arr.splice(2, 1)    
// => ["i"]       // starts with the element at position 2, i.e. 'i', counts 1 item as stated by *deleteCount* and removes it

> arr
// =>["a", "e", "o", "u"]    // Note the original array has been modified

> arr.splice(2, 0)
// => []     //starts with the element at position 2, i.e. 'o', counts 0 item as stated by *deleteCount* and returns it, i.e. nothing

> arr
// => ["a", "e", "o", "u"]     // Note the array remains the same since nothing was removed by the last slice
```
*Note: If arg2 is NaN, it is treated as if it were 0. If it is greater than the array’s length, it will use the array’s length.*

## 3. SPLICE(start, deleteCount, item1, item2...)

Any number of arguments after the second argument represents the new elements to be added.

Working with our previous array which is currently `["a", "e", "o", "u"]`, let's start at position 1, and add 'i' without removing anything from the position.

```
> arr 
// =>  ["a", "e", "o", "u"]
> arr.splice(2, 0, 'i')
// =>[]    // => Empty because the second arg states nothing was removed
> arr
// => ["a", "e", "i",  "o", "u"]   // 'i', the third arg, was added at position 1
```

Let's pick a position to start our splicing, remove 1 item starting from that position, and add two new elements, *"hello", "world"*, to it.

```
> arr 
// =>  ["a", "e", 'i', "o", "u"]
> arr.splice(1, 1, "hello", "world")
// =>["e"]    // 'e', was removed from position 1, as stated by first two args
> arr
// => ["a", "hello", "world", "i", "o", "u"]  // The last two args were added in place of 'e'
```
Now, let's remove the last added args and put 'e' back in our collection of vowels

```
> arr.splice(1, 2, "e")
// => ["hello", "world"]
> arr
// => "a", "e", "i", "o", "u"]

```

# CONCLUSION

Both *slice* and *splice* are useful in selecting a chunk of an array or string. What differs is the way the two methods do so. While *slice* is non-destructive, *splice* is destructive in that it modifies the original array. Both methods can take arguments and *splice()* has an added ability to add and remove items in the array. In any case, should you use *splice()* but do not want to modify the original, you can always opt to create a copy of the array and use *splice()* on the copy. 





