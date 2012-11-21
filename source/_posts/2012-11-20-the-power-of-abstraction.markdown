---
layout: post
title: "The Power of Abstraction"
date: 2012-11-20 12:19
comments: true
categories: 
---
{% img center /images/metzartwork.jpg 'power of abstraction' %}
*'Objects are smarter when they know less.'* Objects should know *what*, and trust other objects to figure out *how.*  Nuggets of wisdom like these pepper the pages of [Sandi Metz's 'Practical Object-Oriented Design in Ruby'](http://www.poodr.info/ "Sandi Metz POODR").  These principles are as abstract as the code they aim to inspire, and, consequentially, as powerful.  In art and music, the concept that abstraction enables a greater range of interpretation and deeper meaning is widely acknowledged, but it seems perhaps counter-intuitive that this same principle would apply to code.  However, as Metz states, the most important feature of well designed code is it's ability to adapt to the changes that will inevitably come.  With this in mind, it becomes easier to understand how less explicit objects can reduce dependencies and allow for a greater range of application.  Let's take a look at an example from POODR.

##Duck Typing##

In Chapter 5 of her book, Metz explores *duck typing*, or defining public interfaces (messages passed from one object to another) that are not tied to any explicit class.  For me, this is an especially compelling example of the ways in which abstracting away details can make your objects more powerful.  The book's example uses a Trip object that sends the message 'prepare' to various other objects, including a Mechanic and a TripCoordinator.
``` ruby Metz Trip Prepare Example
class Trip
  attr_reader :bicycles, :customers, :vehicle

  def prepare(preparers)
    preparers.each {|preparer|
      case preparer 
      when Mechanic
        preparer.prepare_bicyles(bicycles)
      when TripCoordinator
        preparer.buy_food(customers)
      when Driver
        preparer.gas_up(vehicle)  
        preparer.fill_water_tank(vehicle)
      end
    }
  end
end

class Mechanic
  def prepare_bicycles(bicycles)
    #prepare bicycles implementation
  end
end

class TripCoordinator
  def buy_food(customers)
    #buy food implementation
  end
end

class Driver
  def gas_up(vehicle)
    #gas up implementation
  end

  def fill_water_tank(vehicle)
    #fill water tank implementation
  end
end
```
In this implementation, which contains very little abstraction, there are a dangerous number of dependencies.  Through it's 'prepare' message, Trip knows not only *which* objects will receive this message (Mechanic, TripCoordinator, Driver), but *how* those objects prepare for a trip (prepare_bicycles, buy_food, gas_up, etc.).  If in the future, we decide that there must be some other object helping to prepare for a trip (perhaps a 'Mom' object that packs a paper bag lunch ; ), we would need to explicitly add this class to the case statement inside 'prepare' and include it's 'pack_lunch' method call, all in the Trip object.  Furthermore, if the Driver, who's getting quite snobby, decides it no longer will 'gas_up' vehicles, but 'replenish_fuel_source_through_use_of_nozzle_guided_device', it will break Trip's 'prepare' method, which explicitly calls Driver's 'gas_up' method.

##Refactor##
We can see that by being too tied to specific classes, we've made our code extremely specific and stubbornly opposed to change.  Let's take a look at Metz's example of the above code refactored to use duck typing:

``` ruby Metz Trip Prepare with Duck Typing
class Trip
  attr_reader :bicycles, :customers, :vehicle

  def prepare(preparers)
    preparers.each {|preparer|
      preparer.prepare_trip(self)}
  end
end

class Mechanic
  def prepare_trip(trip)
    trip.bicycles.each {|bicycle|
      prepare_bicycles(bicycle)}
  end
end

class TripCoordinator
  def prepare_trip(trip)
    buy_food(trip.customers)
  end
end

class Driver 
  def prepare_trip(trip)
    vehicle = trip.vehicle
    gas_up(vehicle)
    fill_water_tank(vehicle)
  end
end
```
Trip's 'prepare' message, by not calling any specific classes, can now be used with any object that responds to a 'prepare_trip' message.  If we were to add a 'Mom' object to pack a lunch, a 'Chaperone' object to watch over customers, or a 'SpiritGuide' object to help customers through the toughest parts of the journey, NOTHING needs to change about Trip's 'prepare' message.  All we have to do is ensure that all additional preparer objects have a 'prepare_trip' method, which will encapsulate their class specific implementation of that call (make_lunch, supervise_customers, um, guide_spirit?).  By making Trip's public interface more about *what* then *how*, we've reduced dependencies and made our code much more receptive to change.  

##CONCLUSION##

By making our objects more abstract, we transform them from a Michelangelo to a Picasso, traversing centuries of artistic progress with a single keystroke.  While the David is beautiful, it's a concrete representation of a single immutable thing.  Abstraction, on the other hand, allows the viewer to project their own interpetations onto a piece, allowing for an almost infinite number of meanings.  In this same way, abstract code that relies more on *what* is happening then *how* it happens, and is duck typed to eliminate class dependencies, can be used in any number of ways, and fulfills the adaptability requirement of good design.

*(NOTE: To anyone who finds these topics compelling, is interested in doing any sort of Object Oriented coding, or just likes reading books that make you smarter, follow the link in the first paragraph and check out Sandi Metz's 'Practical Object Oriented Design in Ruby')*






















