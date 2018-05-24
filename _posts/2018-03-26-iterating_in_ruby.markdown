---
layout: post
title:      "Iterating In Ruby"
date:       2018-03-26 14:40:24 -0400
permalink:  iterating_in_ruby
---


The concept of iterating is very important in the Ruby lanuage. Iterating allows you to apply the exact same method to all of the items in a collection.  For example, if I have an array of words and I want to know the length of each word I would have to count the number of letters of each word. For example: 

> my_array =  [car, train, airplane]
> 
> The length of each word in my_array is 3, 5, and 8.

 But Ruby makes that simple. Ruby uses iterators to go over each element of the array and perform whatever method on each item I would like.  Our Flatiron curriculum defines iterators as:

> "... methods that you can call on a collection, like an array, to loop over each member of that collection and do something to or with that member of the collection."
 
So let's apply that to my_array above.  This is how to determine the length of each word of my_array using a Ruby iterator:

```
my_array.each do |item|
puts item.length
end
```
Now, let's break down what's happening here. I'm using the iterator 'each' on my_array. The word 'do' begins my block, or the block of  instructions that I want to perform on 'each' element in the array. The word 'end' completes the block. So everything within the block will be applied to the elements of the array. 

The 'item' is the element of the array being passed into the block. So, first, 'item' will be 'car' and 'car' will be passed in the block and the block will determine its length. The next 'item' is 'train' which will be passed into the block and have the .length method performed on it. Finally, 'airplane' is the 'item' and this will be passed into the block and have the .length method applied.  The output (using the "puts" method) will be the length of each item in my_array, 3, 5, and 8. Now this may not seem important or even useful for such a small array. However, once you're dealing with arrays containg thousands of items, using iterators becomes indespensible.

There are several different iterators in Ruby that can be used in this way with very different outcomes.The purpose, though, is the same.  To perform identical functions over large collections of data.




