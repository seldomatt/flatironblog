---
layout: post
title: "Blocks vs. Procs vs. Lambdas: Ruby Closure Showdown"
date: 2012-10-14 21:19
comments: true
categories: 
---

<p>Early on in your quest to learn Ruby, you're going to encounter 'blocks'.  These little packets of code that can be passed to methods may make you sweat a little at first, but soon enough, passing blocks to Enumerable methods like 'each' and 'map' will be like second nature.  Then, one day, you'll encounter a (gulp), 'proc', or a (bigger gulp), 'lambda', and you'll question why you ever got into this coding business in the first place.  Luckily, you can cast those doubts aside, becuase despite the intimidating sounding names, procs and lambdas are really just blocks stored as variables.  Let's take a minute to break this love triangle apart and examine the subtle differences between blocks, procs and lambdas.</p>
<p>Blocks are packets of code stored between 'do...end' or '{}'.  They're a way of dynamically performing an action on a value returned by a method.
``` ruby Define a Method that Yields to a Block
def my_method
	yield
end

my_method {"yielded to the block"}
```
</p>