---
layout: post
title:  "Ruby on Rails Best Practices"
date:   2020-06-10 16:53:00 +0300
categories: theories
technology: Ruby/Rails
tutorial_type: How To
---

<div align="center" style="background-color:#B22222"> 
<img srcset="
  https://drive.google.com/uc?id=1BSD09VUFcwg9TCVsF846b8UnJtcJHfWa 3x,
  https://drive.google.com/uc?id=1BSD09VUFcwg9TCVsF846b8UnJtcJHfWa 6x
" alt="missing image">
</div>
<br>


#### 1. Space Indentations

One of the easiest and most widely agreed-upon Ruby on Rails best practices is to use two space indentations instead of four space indentations. Additionally, most Ruby on Rails developers agree on never using tabs, which includes not mixing tabs and spaces. This will keep long strings of code easier to read and maintain.

{% highlight ruby %}

def some_method
  some_var = true
  if some_var
    do_something
  else
    do_something_else
  end
end

{% endhighlight %} 

<br>
#### 2. Iteration: Use each Instead of for

Use each instead of a for when iterating through a collection. It is simply more readable.

{% highlight ruby %}
  #for loop
  for i in 1..100
    ...
  end

  # each
  (1..100).each do |i|
    ...
  end

{% endhighlight %}  

<br>
#### 3. Conditionals: Use unless Instead of !if:

Avoid using an if statement with a negative condition unless you need to involve an else to your conditional.

{% highlight ruby %}
  # don't do this
  if !true
    do_this
  end

  # or
  if name != "pluto"
    do_that
  end

  # use Ruby’s exclusive unless
  unless true
    do_this
  end
  
  unless name == "pluto"
    do_that
  end

{% endhighlight %} 



That's it!

*See you in the next tuts*


