---
layout: post
title: "Blocks vs. Procs vs. Lambdas: Ruby Closure Showdown"
date: 2012-10-14 21:19
comments: true
categories: 
---

<p>Early on in your quest to learn Ruby, you're going to encounter <strong>'blocks'</strong>.  These little packets of code that can be passed to methods may make you sweat a little at first, but soon enough, passing blocks to Enumerable instance methods like 'each' and 'map' will be second nature.  Then, one day, you'll encounter (gulp), a <strong>'proc'</strong>, or (bigger gulp), a <strong>'lambda'</strong>, and you'll question why you ever got into this coding business in the first place.  Luckily, you can cast those doubts aside, because despite the intimidating names, procs and lambdas are really just blocks stored as variables.  Let's take a minute to break this love triangle apart and examine the subtle differences between blocks, procs and lambdas.</p>
<h2>Blocks</h2>
<p>Blocks are packets of code stored between <strong>'do...end'</strong> or <strong>'{}'</strong>.  They're a way of performing an action on a value returned by a method.
``` ruby Define and call a method that yields to a block
def my_method
	yield
end

my_method {"This is a block!"}
=> "This is a block!"
```
The example above returns <strong>"This is a block!"</strong>.  The method <strong>'my_method'</strong> yields to the block, which then executes it's code.</p>
<em>(It actually returns 'nil' because of the puts statement, but we're going to ignore that in this and future examples)</em>

<h2>Procs</h2>
<p>A Proc is just a block that is stored in a variable for repeated use.  If we store a proc in a variable, we can pass that variable to any method that expects a proc.
``` ruby Store a block in a variable as a Proc 
def proc_method(a_proc)
	puts a_proc.call
end

my_proc = Proc.new{"This is a proc!"}

proc_method(my_proc)
=> "This is a proc!"
```
If we want to reuse the block of code stored in the <strong>my_proc</strong> variable, we can simply define another method that takes a proc and pass in <strong>my_proc</strong>.</br></br>
``` ruby Pass the same proc to another method
def another_proc_method(a_proc)
	puts a_proc.call + " - reused!"
end

another_proc_method(my_proc)
=> "This is a proc! -  reused!"
```
</p>
<h2>Lambdas</h2>
<p>
Lambdas are very similar to procs, only with a slightly different syntax.
``` ruby Store a block in a variable as a Lambda
def lambda_method(a_lambda)
	puts a_lambda.call
end

my_lambda = lambda {"This is a lambda!"}

lambda_method(my_lambda)
=> "This is a lambda!"
```
So, lambdas are like procs.  In fact, if we run <strong>my_lambda.class</strong>, we'll see that it returns <strong>'Proc'</strong> - that's right, lambdas are instances of the <strong>Proc</strong> class.  Now pick your jaw up off the floor and let's go over the differences between the two.
</p>
<h2>Procs vs. Lambdas</h2>
<p>Procs and lambdas behave in slightly different ways with regards to <strong>a) argument handling</strong> and <strong>b) return context</strong>.  Lambdas, sticklers that they are, are very particular with arguments, and will raise an error if passed less or more arguments than were set when they were defined.  Procs, on the other hand, couldn't give two bits how many arguments you pass them - a proc will set all unused arguments to nil.
``` ruby Parameter handling with procs and lambdas
def parameter_handling(proc_or_lambda)
	proc_or_lambda.call(1)
end

my_proc = Proc.new {|a,b| "Procs aren't sticklers about args..."}
my_lambda = lambda {|a,b| "...but Lambdas are!"}

parameter_handling(my_proc)
=> "Procs aren't sticklers about args..."
parameter_handling(my_lambda)
=> <ArgumentError: wrong number of arguments (1 for 2)>
```
So, if you want your reusable block to <strong>require specific</strong> amount of arguments, you should use a lambda instead of a proc.</p>
<p>The other way procs and lambdas differ is in <strong>return context</strong>.  When a proc encounters a return statement, it will execute the proc and exit the wrapping method.  On the other hand, lambdas, ever the anal retentive counterpart to the freewheeling proc, will execute the lambda and then politely continue to run the wrapping method.  Let's take a look at an example.
```ruby Return context with procs and lambdas
def return_test
	puts "beginning"
	Proc.new{return puts "middle"}.call
	puts "end"
end

return_test
=> "beginning"
	 "middle"

def return_test
	puts "beginning"
	lambda {return puts "middle"}.call
	puts "end"
end

return_test
=> "beginning"
	 "middle"
	 "end"
```
As you can see, when returning our proc, we exited the wrapping method (hence the absence of an "end"), while in returning our lambda, we executed the lambda code and then continued executing the wrapping method call.</p>
<h2>Conclusion</h2>
<p>
So procs and lambdas are really just two ways of storing blocks in variables for repeated use.  Admittedly, our examples were pretty elementary, but blocks, procs and lambdas can be actually be incredibly powerful.  When passed to a method call, they can act on dynamically generated variables (!), making them all 'Closures', which is a topic way outside the scope of this post.  For now, just think of procs and lambdas as the the odd couple spawn of the all mighty block.
</p>
<p><em>(Note: This post was inspired by a <a href="https://speakerdeck.com/u/episko/p/blocks-procs-and-lambdas" target="_blank">short and very informative</a> presentation on the subject available on speakerdeck. Highly recommended for a more in depth look at use cases of procs and lambdas.  Many thanks to the author ; )</em></p>








