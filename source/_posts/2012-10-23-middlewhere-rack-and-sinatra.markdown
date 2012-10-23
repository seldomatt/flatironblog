---
layout: post
title: "MiddleWHERE? Rack and Sinatra"
date: 2012-10-23 09:21
comments: true
categories: 
---
In this post we'll be discussing **Rack** and **Sinatra**, two Ruby libraries for processing HTTP requests.  Though Rack is somewhat the bumbling robot to Sinatra's martini drinking cad, they're both very simple-yet-powerful gems for allowing your Ruby app to interact with web requests.  

{% img center /images/robotandsinatra.jpg 600 600 'rack and sinatra' %}

##HTTP REQUESTS##

Though most of us have been browsing the internet since we were young, the actual mechanics of how your browser grabs Justin Beiber's latest tweet and outputs it to your screen are a complete mystery (why you would *want* to do this is another mystery...).  In very basic terms, the browser submits a request for information, which travels through a web server and up to an application, where that request is processed and a response is sent back down the chain to the browser.

{% img center /images/sinatra1.jpg 500 500 'web requests' %}

This seems simple enough, but for your application, communicating with web servers can be a complicated process.  For Ruby web apps, there are numerous different web servers (Mongrel, Thin, WEBrick, etc.), all of which process the requests coming from web browsers differently, and none of which your app will inherently know what to do with. 

##RACK##

Rack, (called middleware because it goes in the **'middle'** of your app and a web server), aims to simplify this process.  It filters and wraps the HTTP requests coming from the web server and formats them in a way that your app can understand.

{% img center /images/sinatra2.jpg 500 500 'rack middleware' %}

 A Rack application will respond to a **'call'** method, which takes as it's single argument a hash that contains information about the HTTP **'request'**.  The call method returns an array containing the response status code (ex. 401, ouch), response headers, and response body as an array of strings:
``` ruby Simple Rack App
require 'rack'

class RackApp
  def call(env)
    headers = { "Content-Type" => "text/html" }
    [200, headers, ["OK"]]
  end
end

run RackApp.new
```

Rack acts as a butler of sorts for your application, answering the door and translating requests into a language your app can understand.  It also communicates your responses to the outside world, in this case a range of web servers with varying functionality, in a format they can **ALL** understand, which is pretty sweet.  

##SINATRA##

Though it delivers some great functionality, you may have noticed Rack isn't the suavest of communicators.  Here, ol' Blue Eyes can really smooth out the edges.

{% img center /images/sinatra3.jpg 500 500 'sinatra' %}

**Sinatra**, a DSL (domain specific language) for creating web apps in Ruby, strolls in, cigarette in hand, to abstract away messy HTTP request processing with a stupid simple syntax for interacting with Rack and Web Servers...
``` ruby Simple Sinatra App
require 'sinatra'

get '/' do 
  "OK"
end
```

The above code block will give us the same functionality as our Rack app, but as you can see, the syntax is much easier to understand.  With Sinatra, setting up your app to handle various web requests (so far we've only covered 'get', but there are also 'post', 'put', 'delete' and others) is a breeze.  Furthermore, for those apps that perhaps don't need the robust functionality of Rails, it can be a great framework to get a simple web app up and running quickly and efficiently.  

##CONCLUSION 

Sinatra and Rack Middleware allow your app to interact with the web in a clean and efficient way, letting you concentrate on building out the real functionality of your product while they take care of the dirty work.

*NOTE: This post is highly indebted to an amazingly simple, funny, and informative Rack/Sinatra talk given by Tom Black. [Go check it out](http://www.blacktm.com/docs/talks/building_web_apps_with_rack_and_sinatra "Tom Black Rack/Sinatra Talk"), now.* 
