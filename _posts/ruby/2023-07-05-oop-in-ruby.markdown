---
layout: post
title:  "Object-Oriented Programming in Ruby"
author: Denis Kobare
date:   2023-07-05 14:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### Introduction

Ruby is renowned for its simplicity and elegance, and one of its standout features 
is its support for object-oriented programming (OOP). In this article, we will 
delve deeper into the world of object-oriented programming in Ruby, exploring 
advanced concepts and techniques that will empower you to write more robust and modular code.



<br>
### Understanding Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm that organizes data 
and behaviors into reusable structures called objects. The fundamental concepts 
of OOP are encapsulation, inheritance, and polymorphism. Let's explore these 
concepts in detail:


<br>
### 1. Encapsulation
Encapsulation is the process of bundling data and related behaviors (methods) 
together within an object. It provides access to data through well-defined 
interfaces while hiding the internal implementation details. In Ruby, 
encapsulation is achieved using instance variables (prefixed with @) and 
accessor methods (attr_reader, attr_writer, or attr_accessor).


<br>
{% highlight ruby %}

class BankAccount
  attr_accessor :balance
  
  def initialize(balance)
    @balance = balance
  end
  
  def deposit(amount)
    @balance += amount
  end
  
  def withdraw(amount)
    if amount <= @balance
      @balance -= amount
    else
      puts "Insufficient funds."
    end
  end
end

# Creating an object
account = BankAccount.new(1000)
account.deposit(500)
account.withdraw(200)
puts "Current balance: $#{account.balance}"

{% endhighlight %}



<br>
### 2. Inheritance

Inheritance is a mechanism that allows classes to inherit attributes and behaviors 
from a parent class. The child class, also known as the subclass or derived class, 
can extend or override the functionality inherited from the parent class. 
In Ruby, single inheritance is supported, meaning a class can inherit from only 
one parent class.


<br>
{% highlight ruby %}

class Animal
  def speak
    puts "The animal makes a sound."
  end
end

class Dog < Animal
  def speak
    puts "The dog barks."
  end
end

class Cat < Animal
  def speak
    puts "The cat meows."
  end
end

# Creating objects
animal = Animal.new
animal.speak   # Output: The animal makes a sound.

dog = Dog.new
dog.speak      # Output: The dog barks.

cat = Cat.new
cat.speak      # Output: The cat meows.

{% endhighlight %}




<br>
### 3. Polymorphism

Polymorphism allows objects of different classes to be treated as if they belong 
to a common superclass. It enables the use of a single interface to represent 
multiple types. Polymorphism can be achieved through method overriding and 
method overloading.


<br>
{% highlight ruby %}

class Vehicle
  def start_engine
    puts "Engine starting..."
  end
end

class Car < Vehicle
  def start_engine
    puts "Car engine starting..."
  end
end

vehicle = Vehicle.new
vehicle.start_engine   # Output: Engine starting...

car = Car.new
car.start_engine       # Output: Car engine starting...

{% endhighlight %}



<br>
### Advanced Object-Oriented Concepts:


#### i). Method Overriding

Method overriding occurs when a subclass provides its own implementation of 
a method that is already defined in its superclass. The overridden method in 
the subclass is invoked instead of the superclass method when the method is 
called on an object of the subclass. This allows for specialized behavior in 
subclasses.

<br>
#### ii). Modules and Mixins

Modules are a way to package together methods, constants, and other module 
declarations. They serve as a mechanism for code reuse and provide a means 
of multiple inheritance in Ruby. Modules can be included in classes using 
the include keyword, enabling the class to access the methods defined in the 
module. Mixins are a specific use case of modules, allowing multiple classes 
to share a common set of methods.
    
    
<br>
{% highlight ruby %}

module Greetings
  def greet
    puts "Hello!"
  end
end

class Person
  include Greetings
end

class Robot
  include Greetings
end

person = Person.new
person.greet    # Output: Hello!

robot = Robot.new
robot.greet     # Output: Hello!

{% endhighlight %}




<br>
### Conclusion

Object-Oriented Programming in Ruby empowers you to write modular, maintainable, 
and reusable code. By mastering concepts such as encapsulation, inheritance, 
polymorphism, method overriding, modules, abstract classes, and class methods, 
you can take full advantage of Ruby's object-oriented capabilities. Experiment 
with these concepts and techniques to build elegant and powerful applications.


<br>
*Thanks for reading, see you in the next one!*
