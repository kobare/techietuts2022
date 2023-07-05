---
layout: post
title:  "Metaprograming in Ruby"
author: Denis Kobare
date:   2023-07-02 11:45:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### Introduction

Metaprogramming in Ruby empowers developers to write code that can modify itself 
at runtime, opening up endless possibilities for creating dynamic and flexible 
applications. In this section, we'll embark on a journey through the fascinating 
world of metaprogramming in Ruby, exploring key concepts and providing 
implementation examples.


<br>
### 1. Metaclasses: Extending Class Behavior Dynamically

Metaclasses are special classes in Ruby that define the behavior of other classes. 
For example, you can dynamically add a class method, say_hello, to the Person 
class using metaprogramming:


<br>
{% highlight ruby %}

class Person
end

Person.singleton_class.class_eval do
  def say_hello
    puts "Hello, everyone!"
  end
end

Person.say_hello


{% endhighlight %}



<br>
### 2. method_missing: Gracefully Handling Missing Methods

The method_missing method gets called when a method is invoked that doesn't exist 
in an object. You can use method_missing to create a dynamic method dispatcher. 
Here's an example:


<br>
{% highlight ruby %}

class DynamicDispatcher
  def method_missing(method_name, *args)
    if method_name.to_s.start_with?("process_")
      process_dynamic_method(method_name, *args)
    else
      super
    end
  end

  def process_dynamic_method(method_name, *args)
    # Implement custom logic based on the method name
    puts "Processing dynamic method: #{method_name}"
  end
end

dispatcher = DynamicDispatcher.new
dispatcher.process_custom_action # This will invoke method_missing

{% endhighlight %}




<br>
### 3. define_method: Generating Methods at Runtime

The define_method method allows you to define methods dynamically at runtime. 
Here's an example of creating a dynamic attribute setter method:


<br>
{% highlight ruby %}

class DynamicAttributes
  def initialize
    @attributes = {}
  end

  def define_attribute(attribute_name)
    define_method("#{attribute_name}=") do |value|
      @attributes[attribute_name] = value
    end

    define_method(attribute_name) do
      @attributes[attribute_name]
    end
  end
end

obj = DynamicAttributes.new
obj.define_attribute(:name)

obj.name = "John"
puts obj.name # Output: John

{% endhighlight %}



<br>
### 4. Building DSLs: Creating Expressive APIs

Metaprogramming in Ruby is an excellent tool for building Domain-Specific 
Languages (DSLs). Here's an example of creating a simple DSL for defining routes 
in a web framework:


<br>
{% highlight ruby %}

class Router
  def initialize
    @routes = {}
  end

  def route(path, &block)
    @routes[path] = block
  end

  def process_request(path)
    if @routes.key?(path)
      @routes[path].call
    else
      puts "404 Not Found"
    end
  end
end

router = Router.new
router.route("/home") do
  puts "Displaying home page"
end

router.process_request("/home") # Output: Displaying home page

{% endhighlight %}



<br>
### Best Practices and Considerations: Metaprogramming Wisdom

While metaprogramming offers great power, it should be used judiciously. Best 
practices include prioritizing code readability, avoiding excessive complexity, 
and providing proper documentation when employing metaprogramming techniques.



<br>
### Conclusion

Metaprogramming in Ruby unlocks a world of dynamic possibilities, allowing 
developers to create flexible and expressive applications. By exploring metaclasses, 
method_missing, define_method, and practical implementation examples, you have 
the knowledge to harness the true potential of metaprogramming in Ruby. Experiment 
responsibly, embrace best practices, and let the dynamic power of Ruby propel your 
coding skills to new heights.


<br>
*Thanks for reading, see you in the next one!*
