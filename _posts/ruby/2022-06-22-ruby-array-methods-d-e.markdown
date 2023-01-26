---
layout: post
title:  "Ruby Array Methods [D - E]"
author: Denis Kobare
date:   2022-06-22 06:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . difference()
Returns a new Array containing only those elements that are not found in the 
arrays passed in the argument.

{% highlight ruby %}

# Array.difference(other_array)

arr = [1, 2, 3, 4, 5]

arr.difference([1, 5])

# [2, 3, 4]

{% endhighlight %}


<br>
### 2 . delete_at()
Deletes an element from array, per the given index.

{% highlight ruby %}

# Array.delete_at(index)

arr = [1, 2, 3, 4, 5]

arr.delete_at(2)

arr
# [1, 2, 4, 5]

{% endhighlight %}


<br>
### 3 . delete_if
Removes each element in array for which the block returns a truthy value.

{% highlight ruby %}

# Array.delete_if {...}

arr = [1, 2, 3, 4, 5]

arr.delete_if {|a| a==4}

# [1, 2, 3, 5 ]

{% endhighlight %}


<br>
### 4 . drop()
Returns a new Array containing all but the first n element of array, where n is a
non-negative Integer. Does not modify array.

{% highlight ruby %}

# Array.drop(n)

arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0]

arr.drop(5)

# [[6, 8, 9], [8, 9, 10], 0]

{% endhighlight %}


<br>
### 5 . drop_while
Calls the block with each successive element of array; stops if the block returns
false or nil; returns a new Array omitting those elements for which the block 
returned a truthy value.

{% highlight ruby %}

# Array.drop_while {...}

arr = [1, 2, 3, 4, 5]

arr.drop_while {|a| a > 3}

# [1, 2, 3]

{% endhighlight %}


<br>
### 6 . delete()
Removes the specified element from the array.

{% highlight ruby %}

# Array.delete(element)

arr = [1, 2, 3, 4, 5]

arr.delete(3)

arr

# [1, 2, 4, 5]

{% endhighlight %}


<br>
### 7 . dig()
Returns the element/array in nested arrays that is specified by index and identifiers.

{% highlight ruby %}

# Array.dig(index)

arr = [1, 2, [[6, 8, 9], [8, 9, 10]], 0]

arr.dig(2)

# [[6, 8, 9], [8, 9, 10]]


arr.dig(2, 1)

# [8, 9, 10]

{% endhighlight %}


<br>
### 8 . detect
Returns the first element for which the block returns a truthy value.
                           
{% highlight ruby %}

# Array.detect {...}

arr = [1, 2, 3, 4, 5]

arr.detect {|a| a > 2}

#  3

{% endhighlight %}


<br>
### 9 . dup
Produces a shallow copy of object. The instance variables of object are copied, 
but not the objects they reference. It also doesn't copy the frozen value state 
of object unlike the clone method.

{% highlight ruby %}

# Array.dup

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9].freeze

arr.frozen?

# true

arr.object_id

# 5597800


dup_copy = arr.dup

dup_copy.frozen?

# false


dup_copy.object_id

# 5621360


# In comparisson to dup

clone_copy = arr.clone

clone_copy.frozen?

# true


clone_copy.object_id

# 5609580


{% endhighlight %}


<br>
### 10 . each_index
Passes each successive array index to the block.

{% highlight ruby %}

# Array.each_index {...}

arr = [3, 4, "Ruby", "Rails"]

arr.each_index {|index|  puts "#{index} #{a[index]}" }

# 0 3
# 1 4
# 2 Ruby
# 3 Rails

{% endhighlight %}


<!-- ....................................................................... -->

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-6160810274402662"
     crossorigin="anonymous"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-6160810274402662"
     data-ad-slot="1384666378"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<!-- ....................................................................... -->


<br>
### 11 . empty?
Returns true if the count of elements in array is zero, false otherwise.
                           
{% highlight ruby %}

# Array.empty?

arr = [1, 2, 3, 4, 5]

arr.empty?

# false


arr2 = [ ]

arr2.empty?

# true

{% endhighlight %}



<br>
### 12 . eql?
Returns true if array and another array are the same size and contain the same elements in the same index.
                         
{% highlight ruby %}

# Array.eql?(other_array)

arr = [1, 2, 3, 4]

arr2 = [1, 2, 3]

arr3 = [1, 2, 3, 4]


arr.eql?(arr2)

# false


arr.eql?(arr3)

# true

{% endhighlight %}



<br>
### 13 . each
Iterates over array elements. When a block is given, it passes each successive array 
element to the block and returns the array.
                                     
{% highlight ruby %}

# Array.each

arr = [1, false, 3, "Ruby", true]

arr.each {|a| puts "#{a} - #{a.class}" }

# 1 - Integer
# false - FalseClass
# 3 - Integer
# Ruby - String
# true - TrueClass
# => [1, 2, 3, "Ruby", true]

{% endhighlight %}


<br>
### 14 . entries
This method is an alias for to_a method. Returns an array containing the items 
in self.

{% highlight ruby %}

# Array.entries

arr = [1, 2, 3, 4]

arr.entries

# [1, 2, 3, 4]


(1..5).entries

# [1, 2, 3, 4, 5]

{% endhighlight %}


<br>
### 15 . each_with_index
With a block given, it calls the block with each element and its index and returns the array.

{% highlight ruby %}

# Array.each_with_index {...}

arr = [3, 4, "Ruby", "Rails"]

arr.each_with_index {|a, i| p "#{i}. #{a}"}

# "0. 3"
# "1. 4"                                                                                                    
# "2. Ruby"                                                                                                 
# "3. Rails"                                                                            
# => [3, 4, "Ruby", "Rails"]                                                            

{% endhighlight %}


<br>
### 16 . each_entry
Calls the given block with each element, converting multiple values from yield 
to an array; returns self.

{% highlight ruby %}

# Array.chunk

arr = []
(1..4).each_entry {|element| arr.push(element) }

arr

# [1, 2, 3, 4]

{% endhighlight %}


<br>
### 17 . each_slice
Iterates for each range of N elements and prints them. It takes an integer 
argument (slice size) into which the elements are grouped.

{% highlight ruby %}

# Array.each_slice(slice_size) {...}

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.each_slice(4){|a| p a}

# [4, 7, 1, 2]
# [3, 4, 5, 6]
# [7, 8, 9]

{% endhighlight %}



<br>
### 18 . each_cons
Iterates for consecutive N elements starting from each element every time.

{% highlight ruby %}

# Array.each_cons(size)

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.each_cons(5){|a| p a}

#  [4, 7, 1, 2, 3]
# [7, 1, 2, 3, 4]
# [1, 2, 3, 4, 5]
# [2, 3, 4, 5, 6]
# [3, 4, 5, 6, 7]
# [4, 5, 6, 7, 8]
# [5, 6, 7, 8, 9]

{% endhighlight %}



<br>
### 19 . each_with_object
Calls the block once for each element, passing both the element and the given object.

{% highlight ruby %}

# Array.each_with_object(object) {...}

arr = [1, 2, 3, 4]

# e.g squaring the elements  
arr.each_with_object([]) {|i, a| a.push(i**2) }

# [1, 4, 9, 16]

{% endhighlight %}



<br>
### 20 . equal?
Checks for equality at the Object level and returns true only if array and 
another array are the same object.
                          
{% highlight ruby %}

# Array.equal?(another_array)

arr = [1, 2, 3, 4]

arr2 = arr.dup

arr3 = arr

arr.equal?(arr2)
# false

arr.equal?(arr3)
# true

{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
