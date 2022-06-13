---
layout: post
title:  "Ruby Array Methods"
author: Denis Kobare
date:   2023-06-12 06:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . at()
Finds item at the given index and returns nil if the index does not exist in the array.

{% highlight ruby %}

# Array.at(index)

arr = [1, 2, 3, 4, 5]

third_item = arr.at(2)

# 3

{% endhighlight %}


<br>
### 2 . append()
Appends items to the end of the array.

{% highlight ruby %}

# Array.append(index)

arr = [1, 2, 3, 4, 5]

third_item = arr.append(6)

# [1, 2, 3, 4, 5, 6]

{% endhighlight %}


<br>
### 3 . assoc()
Searches for an array within another array. The argument is compared with first 
item of the inner array, and that array is returned if that first item matches the argument. Further matches are ignored.

*NB: Works only for two dimensional arrays*

{% highlight ruby %}

# Array.assoc(key_word)

arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10] ]

find_first_array_containing_8 = arr.assoc(8)

# [8, 9, 10]

{% endhighlight %}


<br>
### 4 . all?
With no block given and no argument, returns true if self contains only 
truthy elements, false otherwise

{% highlight ruby %}

# Array.assoc(key_word)

arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0]

arr.all?

# true



arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0, true]

arr.all?

# true


arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0, nil]

arr.all?

# false


arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0, false]

arr.all?

# false

{% endhighlight %}


<br>
### 5 . any?
With no block given and no argument, returns true if self has any truthy 
element, false otherwise

{% highlight ruby %}

# Array.append(index)

arr = [1, 2, 3, 4, 5]

arr.any?

# true

{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
