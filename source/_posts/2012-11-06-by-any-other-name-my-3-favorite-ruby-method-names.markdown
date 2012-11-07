---
layout: post
title: "By Any Other Name: My 3 Favorite Ruby Method Names"
date: 2012-11-06 13:36
comments: true
categories: 
---
{% img center /images/methnames1.jpg 'ruby method names' %}
**'Slice'**.  **'Pop'**. **'Chunk'**. **'Squeeze'**.  While you might think these words were picked from a list of old comic book sound effects, (and they likely could've been), they're actually just a few of Ruby's wildly evocative method names.  As a language built on expressivity and with it's fair share of syntactic sugar, Ruby boasts a number of methods whose descriptive names act as metaphors for the sometimes abstract operations they perform.  Not only does this aid in remembering methods when you need them, *(hmm, I wonder which method will let me 'pop' that last element out of the array...)*, but it also turns you, the programmer, into a carpenter or sorts, manipulating and using objects in a so-close-to-real-you-can-almost-smell-the-sawdust kind of way.  In this post, we'll take a look at my three favorite Ruby method names: **Tap**, **Inject**, and **Send**.

##Tap##

{% img center /images/methnames2.jpg 'tap method' %}

When called on an object, 'tap' yields that object to a block where other operations can be performed on it, and then returns the object to the method chain.  
```ruby Simple Use of Tap 
cool_method = "pat".reverse.tap{|object| object << "!"}.upcase
```
While Ruby's API says the method is so-named because it's used to 'tap into' a method chain, for me, tap brings to mind a game of pool, where the receiver is the cue ball.  Calling 'tap' does just that - it gives the cue ball (receiver object) a light tap, sending it to the block where operations are carried out.  It's then sent along to the next method in the chain, or in our metaphor, the next billiard ball.
While I'm no pool shark, this helps me to understand the way in which tap momentarily changes the trajectory of an object in a method chain, sending it to the block for processing before it returns to the chain, like pool balls banking off a felted rail.

##Inject##

{% img center /images/methnames3.jpg 600 600 'inject method' %}

Inject is an enumerable method that will combine all the elements in the enum object by applying an operation specified by a block.  When given a block, for each element in the enumerable, the block is passed both the accumulator value (or memo) and the element.  A simple example would be using inject to sum all elements in an array.
```ruby Simple Use of Inject
sum = (1..5).inject{|memo, element| memo + element}
```
[While some people prefer the similar but much less evocatively named 'each_with_object' method](http://kcurtin.github.com/blog/2012/10/30/rubys-each-with-object/ "Kevin Curtin Each With Object post")(and I might be inclined to agree with them), based on name alone, 'inject' is the most expressive representation of what the operation is actually doing.  In essence, the block 'injects' the result of it's expression into the memo each time it's invoked, allowing for the powerful functionality of being able to act on a dynamically generated memo value. 

##Send##
{% img center /images/methnames4.jpg 600 600 'send method' %}

'Send' is used to call a method on an object in a more abstract way then simply calling the method.  It takes the method being called as an argument, and any arguments for that method as additional arguments to send.
When you send a method to an object, it's as if you're putting that method into an envelope and mailing it to the receiver.  What's powerful about 'send' is that the contents of that letter (the method call and arguments) can be dynamically generated.
```ruby Simple Use of Send
["upcase"].map do |method|
  method.send(method.to_sym)
end
```
There are lot's of ways to send a message.  Flowers on a doorstep.  Graffiti on a wall.  Dead fish in a newspaper.  In all cases, sending a message is a more abstract way of telling someone what you want, similar to the way in which 'send' is a more dynamic and abstract way of calling a method on an object.

##CONCLUSION##
These are just a few of the methods whose names can paint a vivid picture of the operations they perform.  Names like these can go a long way in making programmers emotionally invested in the sometimes esoteric tasks being performed.  So next time you use the 'tap' method, don't be alarmed if you find yourself rubbing the chalk off your fingers....
