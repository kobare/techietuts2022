---
layout: post
title:  "Refactoring Ruby Code: Example 1"
author: Denis Kobare
date:   2022-03-13 14:15:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---

### Definition

Code refactoring is the process of restructuring existing code without changing its functionality, in order to improve the design, structure and implementation of the program.

Refactoring ultimately makes software easier to understand, improves performance and helps us find bugs faster.

<br>
### Refactoring Step by Step

Have a look at this code which we'll be refactoring in this example:

{% highlight ruby %}

class OrdersReport
  def initialize(orders, start_date, end_date)
    @orders = orders
    @start_date = start_date
    @end_date = end_date
  end
  
  def total_sales_within_date_range
    orders_within_range = @orders.select { |order| order.placed_at >= @start_date && order.placed_at <= @end_date }
    
    orders_within_range.map(&:amount).inject(0) { |sum, amount| amount + sum }
  end  
end  

class Order < OpenStruct

end

{% endhighlight %} 

<br>
1 . Extract this into a private method:

{% highlight ruby %}
orders_within_range = @orders.select { |order| order.placed_at >= @start_date && order.placed_at <= @end_date }
{% endhighlight %}  

This is more likely reusable than when clamped in another method and also creates some level of abstraction as compared to the original code.
{% highlight ruby %}
# ...
  def total_sales_within_date_range
    orders_within_range.map(&:amount).inject(0) { |sum, amount| amount + sum }
  end  

  private
  
  def orders_within_range
    @orders.select { |order| order.placed_at >= @start_date && order.placed_at <= @end_date }
  end

{% endhighlight %}  

<br>
2 . The "Tell Dont Ask" principle: Its better to send an object a message and have it perform work rather than ask that object about its internal states and decide what to do on its behalf. Essentially, we dont want the internal details of an object leaking out into code surrounding it.

This code violates the above principle since it is exposing the internal states of the order object, which can be abstracted. It is asking orders object if there are orders placed at a given start_date and end_date: 

{% highlight ruby %}
  private
  
  def orders_within_range
    @orders.select { |order| order.placed_at >= @start_date && order.placed_at <= @end_date }
  end

{% endhighlight %}  

Notice how we are now not exposing the internal attributes of the order object. We're now just sending a message and waiting for the results. Cleaner and more readable:

{% highlight ruby %}
class OrdersReport
# ...
  private
  
  def orders_within_range
    @orders.select { |order| order.placed_between?(@start_date, @end_date) }
  end
end

class Order < OpenStruct
  def placed_between?(start_date, end_date)
    placed_at >= start_date && placed_at <= end_date
  end
end

{% endhighlight %}  


<br>
3 . Extract an object to hold data clamp values

A data clamp is a pair of arguments that have an implicit reliance on each other i.e you have to pass them together all the time. Data clamps should be extracted into objects.
In this case we'll extract, start_date and end_date into it's own class, DateRange:


{% highlight ruby %} 

class OrdersReport

  def initialize(orders, date_range)
    @orders = orders
    @date_range = date_range
  end

  def total_sales_within_date_range
    orders_within_range.map(&:amount).inject(0) { |sum, amount| amount + sum }
  end  
    
  private
  
  def orders_within_range
    @orders.select { |order| order.placed_between?(@date_range) }
  end
end

class DateRange < Struct.new(:start_date, :end_date)
end

class Order < OpenStruct
  def placed_between?(date_range)
    placed_at >= date_range && date_range <= end_date
  end
end

{% endhighlight %}

Notice that the DateRange class is a simple class which doesn't even have a single behaviour at this stage of refactoring. But it is still useful because it allowed us to make the data clamp explicit and also reduced parameter coupling (coupling is the degree to which two components rely on each other. The lower the coupling the easier it is to make changes without breaking the functionality of other components). So don't be reluctant to extract classes as long as something is improved.

<br>

4 . Combine data and behavior

Things that are tightly coupled should be in the same component. In this case, knowing if a date is between two dates might not be a good responsibility for Order but it is for DateRange:

{% highlight ruby %} 
class Order < OpenStruct
  def placed_between?(date_range)
    placed_at >= date_range && date_range <= end_date
  end
end

{% endhighlight %}


{% highlight ruby %} 

class DateRange < Struct.new(:start_date, :end_date)
  def include?(date)
    # date >= start_date && date <= end_date # refactor this to:
    (start_date..end_date).cover?(date)
  end
end

class Order < OpenStruct
  def placed_between?(date_range)
    date_range.include?(placed_at)
  end
end

{% endhighlight %}

NB: (start_date..end_date).include?(date) could work too but the cover method is much better performance-wise since it only instanciates the two endpoints as opposed to the former which will instanciate all the objects within that range.

<br>
5 . Improve the readability

Public methods should be very clear. Suppose we want to expose the method below as an API, this is not easily readable:

{% highlight ruby %} 
  def total_sales_within_date_range
    orders_within_range.map(&:amount).inject(0) { |sum, amount| amount + sum }
  end  
{% endhighlight %}

Refactor it to this, and its now simple to consume as an API:

{% highlight ruby %} 
  def total_sales_within_date_range
    total_sales(orders_within_range)
  end  
  
  private
  
  def total_sales(orders)
    # orders.map(&:amount).inject(0) { |sum, amount| amount + sum } #refactor this to this:
    orders.map(&:amount).inject(0, :+)   
  end
  # ...
{% endhighlight %}

#### Final Results

{% highlight ruby %} 
class OrdersReport

  def initialize(orders, date_range)
    @orders = orders
    @date_range = date_range
  end

 
  def total_sales_within_date_range
    total_sales(orders_within_range)
  end  
 
  
  private
  
  def total_sales(orders)
    orders.map(&:amount).inject(0, :+)   
  end
   
  def orders_within_range
    @orders.select { |order| order.placed_between?(@date_range) }
  end
end


class DateRange < Struct.new(:start_date, :end_date)
  def include?(date)
    (start_date..end_date).cover?(date)
  end
end


class Order < OpenStruct
  def placed_between?(date_range)
    date_range.include?(placed_at)
  end
end
{% endhighlight %}
*Thanks for reading, see you in the next one!*
