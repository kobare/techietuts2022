---
layout: post
title:  "Ruby Array Methods [J - O]"
author: Denis Kobare
date:   2022-08-16 07:35:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . join()
Converts each element of the array to a string, separated by the given separator.

{% highlight ruby %}

# Array.join(separator)

arr = [1, 2, 3, 4, 5]

arr.join("-")
# "1-2-3-4-5"

arr.join("9")
# "192939495"

{% endhighlight %}


<br>
### 2 . keep_if()
Retains those elements for which the block returns a truthy value then deletes all 
other elements and returns the array.

{% highlight ruby %}

# Array.keep_if {...}

arr = [1, 2, 3, 4, 5]

arr.keep_if {|i| i.odd? }
# [1, 3, 5]

{% endhighlight %}


<br>
### 3 . last()
Returns the last n elements in a new Array. When no argument is given, it returns the last element.

{% highlight ruby %}

# Array.last(n)

arr = [1, 2, 3, 4, 5]

arr.last(2)
# [4, 5]


arr.last
# 5

{% endhighlight %}


<br>
### 4 . length
Returns the count of elements in the array.

{% highlight ruby %}

# Array.length

arr = [1, 2, 3, 4, 5]

arr.length
# 5

{% endhighlight %}


<br>
### 5 . map!
Calls the block with each element and replaces the element with the block's return value.
This method is an alias for <span class="badge">collect!</span> method.

{% highlight ruby %}

# Array.map!

arr = [1, 2, 3, 4, 5]

arr.map! {|e| e**2 }
# [1, 4, 9, 16, 25]


arr
# [1, 4, 9, 16, 25]

{% endhighlight %}


<br>
### 6 . map
Calls the block with each element and returns a new array whose elements are the return values from the block.
This method is an alias for <span class="badge">collect</span> method.

{% highlight ruby %}

# Array.map

arr = [1, 2, 3, 4, 5]

arr.map {|e| e**2 }
# [1, 4, 9, 16, 25]

arr
# [1, 2, 3, 4, 5]

{% endhighlight %}


<br>
### 7 . minmax
Returns a new 2-element Array containing the minimum and maximum values from the array.
When a block is given, the block must return an Integer. When no block is given, 
each element must respond to Comparable method.

{% highlight ruby %}

# Array.minmax {...}
# Array.minmax

arr = [1, 2, 3, 4, 5]

arr.minmax {|a, b| a <=> b }
# [1, 5]

arr.minmax
# [1, 5]

{% endhighlight %}


<br>
### 8 . max
Returns one of the maximum-valued element from array. When a block is given, the 
block must return an Integer. When no block is given, each element must respond to Comparable method.

With an argument Integer n and no block, it returns a new array with at most n elements.
With an argument n and a block, it returns a new array with at most n elements, in 
descending order per the block.

{% highlight ruby %}

# Array.max(n) {...}
# Array.max(n)
# Array.max {...}

arr = [1, 2, 3, 4, 5]

arr.max
# 5

arr.max(2)
# [5, 4]

arr.max {|a, b| a <=> b }
# => 5

arr.max(2) {|a, b| a <=> b }
# [5, 4]

{% endhighlight %}


<br>
### 9 . min
Returns one of the minimum-valued element from array. When a block is given, the 
block must return an Integer. When no block is given, each element must respond to Comparable method.

With an argument Integer n and no block, it returns a new array with at most n elements.
With an argument n and a block, it returns a new array with at most n elements, in 
ascending order per the block.

{% highlight ruby %}

# Array.min(n) {...}
# Array.min(n)
# Array.min {...}

arr = [1, 2, 3, 4, 5]

arr.min
# 1

arr.min(2)
# [1, 2]

arr.min {|a, b| a <=> b }
# => 1

arr.min(2) {|a, b| a <=> b }
# [1, 2]

{% endhighlight %}


<br>
### 10 . min_by
Returns the elements for which the block returns the minimum values. With a block 
given and no argument, returns the element for which the block returns the minimum value.

With a block given and positive integer argument n given, returns an array 
containing the n elements for which the block returns minimum values.

{% highlight ruby %}

# Array.min_by {...}
# Array.min_by(n) {...}

arr = [1, 2, 3, 4, 5]

arr.min_by {|e| e }
# 1

arr.min_by(2) {|e| e }
# [1, 2]

{% endhighlight %}


<br>
### 11 . max_by
Returns the elements for which the block returns the maximum values. With a block 
given and no argument, returns the element for which the block returns the maximum value.

With a block given and positive integer argument n given, returns an array 
containing the n elements for which the block returns maximum values.

{% highlight ruby %}

# Array.max_by {...}
# Array.max_by(n) {...}

arr = [1, 2, 3, 4, 5]

arr.max_by {|e| e }
# 5

arr.max_by(2) {|e| e }
# [5, 4]

{% endhighlight %}


<br>
### 12 . minmax_by
Returns a 2-element array containing the elements for which the block returns 
minimum and maximum values.

{% highlight ruby %}

# Array.minmax_by {...}

arr = [1, 2, 3, 4, 5]

arr.minmax_by {|e| e }
# [1, 5]

{% endhighlight %}


<br>
### 13 . member?
Returns true if for some index i in array, obj == array[i]; otherwise false.
This is an alias for <span class="badge">include?<span> method.

{% highlight ruby %}

# Array.member?(obj)

arr = [1, 2, 3, 4, 5]

arr.member?(3)
# true

{% endhighlight %}


<br>
### 14 . methods
Returns a list of the names of public and protected methods of obj. This will include
all the methods accessible in obj's ancestors. If the optional parameter is false, 
it returns an array of obj's public and protected singleton methods, the array 
will not include methods in modules included in obj.

{% highlight ruby %}

# Array.methods(regular=true) 
# Array.methods

arr = [1, 2, 3, 4, 5]

arr.methods
# [:hash, :abbrev, :to_h, :include?, :&]

{% endhighlight %}


<br>
### 15 . none?
Returns true if no element of the array meet a given criterion.

With no block given and no argument, returns true if the array has no truthy elements, false otherwise.

With a block given and no argument, calls the block with each element in the 
array and returns true if the block returns no truthy value, false otherwise.

If argument obj is given, returns true if obj.=== no element, false otherwise.

{% highlight ruby %}

# Array.none? {...}
# Array.none?
# Array.none?(obj)

arr = [1, 2, 3, 4, 5]

arr.none? {|e| e < 2 }
# false

arr.none?(6)
# true

arr.none?
# false

[].none?

# true

{% endhighlight %}


<br>
### 16 . nil?
Returns true if the array is empty. Only the object nil responds true to nil?.

{% highlight ruby %}

# Array.nil?

arr = [1, 2, 3, 4, 5]

arr.nil? 
# false

{% endhighlight %}


<br>
### 17 . one?
Returns true if exactly one element of the array meets a given criterion.

With no block given and no argument, returns true if the array has exactly one 
truthy element, false otherwise.

With a block given and no argument, calls the block with each element in
the array and returns true if the block yields a truthy value for exactly one element, false otherwise.

If argument obj is given, returns true if obj.=== exactly one element, false otherwise.

{% highlight ruby %}

# Array.one?

arr = [1, 2, 3, 4, 5]

arr.one? 
# false

arr2 = [1, nil]

arr2.one? 
# true


arr.one? {|e| e > 3}
# false

arr.one?(3) 
# true

arr.one? {|e| e > 4}
# true

{% endhighlight %}


<br>
### 18 . object_id
Returns an integer identifier for obj.

{% highlight ruby %}

# Array.object_id

arr = [1, 2, 3, 4, 5]

arr.object_id 
# 739080


arr2 = [6, 7, 8, 9]

arr2.object_id 
# 783540

{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
