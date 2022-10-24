---
layout: post
title:  "Ruby Array Methods [P - S]"
author: Denis Kobare
date:   2022-09-01 04:00:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . push()
Appends each argument in objects to the array and returns the array.
This method is an alias for <span class="badge">append</span> method.

{% highlight ruby %}

# Array.push(*objects)

arr = [1, 2, 3, 4, 5]

arr.push(6, 7, [8, 9])
# [1, 2, 3, 4, 5, 6, 7, [8, 9]]

{% endhighlight %}


<br>
### 2 . pop()
Removes and returns trailing elements. When no argument is given and the array 
is not empty, it removes and returns the last element. And if the array is empty, 
it returns nil. When a non-negative Integer argument n is given and is in range, 
it removes and returns the last n elements in a new Array. If n is positive and 
out of range, it removes and returns all elements.

{% highlight ruby %}

# Array.pop(n)

arr = [1, 2, 3, 4, 5]

arr.pop
# 5

arr
# [1, 2, 3, 4]


arr.pop(2)
# [4, 5]

arr
# [1, 2, 3]


arr.pop(10)
# [1, 2, 3, 4, 5]

arr
# []

{% endhighlight %}


<br>
### 3 . permutation()
Returns the possible ordered arrangements of elements. The order of permutations 
is indeterminate.
When a block and an in-range positive Integer argument n (0 < n <= Array.size) 
are given, calls the block with all n-tuple permutations of the array.

{% highlight ruby %}

# Array.permutation(n)

arr = [1, 2, 3]

arr.permutation {|permutation| p permutation }
# [1, 2, 3]
# [1, 3, 2]                                                                         
# [2, 1, 3]                                                                         
# [2, 3, 1]                                                                         
# [3, 1, 2]                                                                         
# [3, 2, 1]                                                                         


arr.permutation(2) {|permutation| p permutation }
# [1, 2]
# [1, 3]
# [2, 1]
# [2, 3]
# [3, 1]
# [3, 2]


{% endhighlight %}


<br>
### 4 . product()
Returns an array of all combinations of elements from all arrays.

{% highlight ruby %}

# Array.product(*other_arrays) {...}

arr1 = ["Very", "Rather"]

arr2 = ["Good", "Pleasant"]

arr1.product(arr2) {|combination| p combination }
# ["Very", "Good"]
# ["Very", "Pleasant"]
# ["Rather", "Good"]
# ["Rather", "Pleasant"]

{% endhighlight %}


<br>
### 5 . pack()
Packs the contents of arr into a binary sequence according to the directives in 
<span class="badge">aTemplateString</span>.

{% highlight ruby %}

# Array.pack(aTemplateString)

arr = ["a", "b", "c"]


arr.pack("A3A3A3")

# "a  b  c  "


arr.pack("a3a3a3")

# "a\x00\x00b\x00\x00c\x00\x00"


[1, 2, 3].pack("ccc")   

# "\x01\x02\x03"

{% endhighlight %}


<br>
### 6 . prepend
Prepends the given objects to the array. It is an alias for <span class="badge">unshift</span> method.

{% highlight ruby %}

# Array.prepend(obj)

arr = ["a", "b", "c"]


arr.prepend(1, 4)

#  [1, 4, "a", "b", "c"]


{% endhighlight %}


<br>
### 7 . partition
Returns an array of two arrays: The first having those elements for which the 
block returns a truthy value. The other having all other elements.

{% highlight ruby %}

# Array.minmax {...}
# Array.minmax

arr = ["Ruby", "Ruby on Rails", "CSS", "Python", "React"]

arr.partition {|key, value| key.start_with?('R') }
# [["Ruby", "Ruby on Rails", "React"], ["CSS", "Python"]]

{% endhighlight %}


<br>
### 8 . private_methods
Returns the list of private methods accessible to obj. If the all parameter is 
set to false, only those methods in the receiver will be listed.

{% highlight ruby %}

# Array.private_methods

arr = [1, 2, 3, 4, 5]

arr.private_methods
# [:initialize,                                          
# :inherited,                                           
# :remove_const]


{% endhighlight %}


<br>
### 9 . public_methods
Returns the list of public methods accessible to obj. If the all parameter is 
set to false, only those methods in the receiver will be listed.

{% highlight ruby %}

# Array.public_methods

arr = [1, 2, 3, 4, 5]

arr.public_methods

# [:hash,
# :to_h,
# :include?,
# :&]


{% endhighlight %}


<br>
### 10 . rotate
Returns a new Array formed from the array with elements rotated from one end to 
the other.

- When no argument given, it returns a new Array that is like the orginal array, except
that the first element has been rotated to the last position.

- When given a non-negative Integer count, it returns a new Array with count
elements rotated from the beginning to the end.

- If count is large, it uses count % array.size as the count.

- If count is zero, it returns a copy of the array, unmodified.

- When given a negative Integer count, it rotates in the opposite direction, from 
end to beginning.

- If count is small (far from zero), uses count % array.size as the count.

- <span class="badge">rotate!</span> method modifies the original array

{% highlight ruby %}

# Array.rotate {...}
# Array.rotate(count)
# Array.rotate

arr = [1, 2, 3, 4, 5]


arr.rotate
# [2, 3, 4, 5, 1]


arr.rotate(2)
# [3, 4, 5, 1, 2]


arr.rotate(8)
# [4, 5, 1, 2, 3]


arr.rotate(0)
# [1, 2, 3, 4, 5]


arr.rotate(-2)
# [4, 5, 1, 2, 3]


arr.rotate(-6)
# [5, 1, 2, 3, 4]


{% endhighlight %}


<br>
### 11 . reject
Returns a new Array whose elements are all those from the original array for which
the block returns false or nil.

- <span class="badge">reject!</span> method modifies the original array

{% highlight ruby %}

# Array.reject { ... }

arr = [1, 2, 3, 4, 5]

arr.reject {|e| e > 3 }
# [1, 2, 3]

{% endhighlight %}


<br>
### 12 . rassoc
Returns the first element in the array that is an Array whose second element == obj.

{% highlight ruby %}

# Array.rassoc(obj)

arr = [{name: "Ruby"}, {name: "Python"},  [1, 2, 3, 4, 5], [22, 14], [8, 9]]

arr.rassoc(14)
# => [22, 14]

{% endhighlight %}


<br>
### 13 . repeated_permutation
Calls the block with each repeated permutation of length n of the elements of 
the array; each permutation is an Array; it then returns array. The order of the 
permutations is indeterminate.

When a block and a positive Integer argument n are given, it calls the block 
with each n-tuple repeated permutation of the elements of the array. The number 
of permutations is array.size**n.

{% highlight ruby %}

# Arrayrepeated_permutation(n) {|permutation| ... }

arr = [1, 2, 3]

arr.repeated_permutation(1) {|permutation| p permutation }
# [1]
# [2]
# [3]
# [4]
# [5]


arr.repeated_permutation(2) {|permutation| p permutation }
# [1, 1]
# [1, 2]
# [1, 3]
# [2, 1]
# [2, 2]
# [2, 3]
# [3, 1]
# [3, 2]
# [3, 3]

{% endhighlight %}


<br>
### 14 . repeated_combination
Calls the block with each repeated combination of length n of the
elements of the array; each combination is an Array; it then returns the array. 
The order of the combinations is indeterminate.

When a block and a positive Integer argument n are given, it calls the
block with each n-tuple repeated combination of the elements of the array.
The number of combinations is (n+1)(n+2)/2.

{% highlight ruby %}

# Array.repeated_combination(n) {|combination| ... }

arr = [1, 2, 3, 4, 5]

arr.repeated_combination(1) {|combination| p combination }
# [1]
# [2]
# [3]
# [4]
# [5]


arr.repeated_combination(2) {|combination| p combination }
# [1, 1]
# [1, 2]
# [1, 3]
# [1, 4]
# [1, 5]
# [2, 2]
# [2, 3]
# [2, 4]
# [2, 5]
# [3, 3]
# [3, 4]
# [3, 5]
# [4, 4]
# [4, 5]
# [5, 5]

{% endhighlight %}


<br>
### 15 . reverse_each
Iterates backwards over array elements.

When a block is given, it passes, in reverse order, each element to the block then 
returns the array.

{% highlight ruby %}

# Array.reverse_each {...} 


arr = [1, 2, 3, 4, 5]

arr.reverse_each {|e| print e}
# 5
# 4
# 3
# 2
# 1

{% endhighlight %}


<br>
### 16 . rindex
Returns the index of the last element for which object == element.

When argument object is given but no block, returns the index of the last such 
element found.

Returns nil if no such object found. When a block is given but no argument, it 
calls the block with each successive element; returns the index of the last 
element for which the block returns a truthy value.

{% highlight ruby %}

# Array.rindex {...}
# Array.rindex(obj)


arr = ["Ruby", "Ruby on Rails", "CSS", "Python", "React"]

arr.rindex("React")
# 4

arr.rindex {|e| e == "CSS"}
# 2

{% endhighlight %}


<br>
### 17 . replace
Replaces the content of the original array with the content of other_array and 
returns the array.

{% highlight ruby %}

# Array.replace(other_array)

arr = [1, 2, 3, 4, 5]

arr.replace([6, 7, 8, 9]) 
# [6, 7, 8, 9]


{% endhighlight %}


<br>
### 18 . reverse
Returns a new Array with the elements of the original array in reverse order.

- <span class="badge">reverse!</span> method modifies the original array

{% highlight ruby %}

# Array.reverse

arr = [1, 2, 3, 4, 5]

arr.reverse 
# [5, 4, 3, 2, 1]

{% endhighlight %}


<br>
### 19 . reduce
Reduces an array to a single value by summing the elements.

{% highlight ruby %}

# Array.reduce(initial_value) {...}
# Array.reduce(:+)

arr = [1, 2, 3, 4, 5]

arr.reduce(:+) 
# 15


arr.reduce(0) {|sum, num| sum + num} 
# 15

{% endhighlight %}


<br>
### 20 . shift
Removes and returns leading elements. When no argument is given, it removes all 
other elements and returns the first element.

When positive Integer argument n is given, removes the first n elements then it 
returns those elements in a new Array.

If n is as large as or larger than array.length, removes all elements;
returns those elements in a new Array.

{% highlight ruby %}

# Array.shift(n)
# Array.shift

arr = [1, 2, 3, 4, 5]

arr.shift
# 1

arr.shift(2)
# [1, 2]


arr.shift(5)
# [1, 2, 3, 4, 5]

{% endhighlight %}


<br>
### 21 . sort
Returns a new array with sorted elements from the original array.

With no block, compares elements using operator <=> (see Comparable):

- <span class="badge">sort!</span> method modifies the original array

{% highlight ruby %}

# Array.sort {...}
# Array.sort

arr = [1, 2, 3, 4, 5]

arr.sort
# [1, 2, 3, 4, 5]


arr.sort {|a, b| b <=> a }
# [5, 4, 3, 2, 1]

{% endhighlight %}


<br>
### 22 . sort_by
With a block given, it returns an array of elements of the original array, sorted
according to the value returned by the block for each element. The ordering of 
equal elements is indeterminate and may be unstable.

- <span class="badge">sort_by!</span> method modifies the original array

{% highlight ruby %}

# Array.sort_by {...}

arr = [1, 2, 3, 4, 5]


arr.sort_by {|e| -e }
# [5, 4, 3, 2, 1]

{% endhighlight %}


<br>
### 23 . select
Returns a new Array containing those elements of the original array for which the 
block returns a truthy value.

- <span class="badge">select!</span> method modifies the original array

{% highlight ruby %}

# Array.select {...}

arr = ["Ruby", "Ruby on Rails", "CSS", "Python", "React"]


arr.select {|element| element.to_s.start_with?('C') }
# => ["CSS"]

{% endhighlight %}


<br>
### 24 . shuffle
Returns a new array with elements of the original array shuffled.

- <span class="badge">shuffle!</span> method modifies the original array

{% highlight ruby %}

# Array.shuffle

arr = [1, 2, 3, 4, 5]


arr.shuffle
# [5, 2, 3, 1, 4]

{% endhighlight %}


<br>
### 25 . sample
Returns random elements from the array. When no arguments are given, it returns 
a random element from the array.

When argument n is given, returns a new Array containing n random
elements from the array.

{% highlight ruby %}

# Array.sample()
# Array.sample

arr = [1, 2, 3, 4, 5]


arr.sample
# 4


arr.sample(2)
# [5, 2]

{% endhighlight %}


<br>
### 26 . sum
Returns the sum of all the elements in the array. If a block is given, the 
block is applied to the array, then the sum is computed. If the array is empty, 
it returns init.

The elements need not be numeric, but must be +-compatible with each other and 
with init.


{% highlight ruby %}

# Array.sum(init = 0) {...}
# Array.sum(init = 0)

arr = [10, 20, 30]


arr.sum
# 60


arr.sum(5)
# 65

arr.sum {|e| e*2 }
# 120


arr2 = ["uvwxyz"]

arr2.sum("qrst")

# "qrstuvwxyz"

{% endhighlight %}



<br>
### 27 . size
Returns the count of elements in the array. This method is an alias for the 
<span class="badge">length</span> method.

{% highlight ruby %}

# Array.size

arr = [1, 2, 3, 4, 5]

arr.size
# 5

{% endhighlight %}


<br>
### 28 . slice
returns a subarray specified by range of indices.

- <span class="badge">slice!</span> method modifies the original array

{% highlight ruby %}

# Array.slice(range)

arr = [1, 2, 3, 4, 5]

arr.slice(1, 3)
# [2, 3, 4]

arr.slice(1..3)
# [2, 3, 4]

{% endhighlight %}


<br>
### 29 . slice_when
Partitions elements into arrays ("slices"); it calls the block with each element 
and its successor and begins a new slice if and only if the block returns a truthy value.

{% highlight ruby %}

# Array.slice_when {|element, next_element| ... }

arr = [0, 1, 2, 4, 5, 6, 8, 9]

enm = arr.slice_when {|i, j| j != i + 1 }

enm.each {|array| p array }

# [0, 1, 2]
# [4, 5, 6]
# [8, 9]

{% endhighlight %}


<br>
### 30 . slice_before
With argument pattern, returns an enumerator that uses the pattern to
partition elements into arrays ("slices"). An element begins a new slice
if element === pattern (or if it is the first element).

{% highlight ruby %}

# Array.slice_before {|element, next_element| ... }

arr = (1..20)

enm = arr.slice_before {|i| i % 4 == 2 }

enm.each {|array| p array }

# [1]
# [2, 3, 4, 5]
# [6, 7, 8, 9]
# [10, 11, 12, 13]
# [14, 15, 16, 17]
# [18, 19, 20]

{% endhighlight %}


<br>
### 31 . slice_after
With argument pattern, returns an enumerator that uses the pattern to
partition elements into arrays ("slices"). An element ends the current
slice if element === pattern.

{% highlight ruby %}

# Array.slice_after {|element, next_element| ... }

arr = (1..20)

enm = arr.slice_after {|i| i % 4 == 2 }

enm.each {|array| p array }

# [1, 2]
# [3, 4, 5, 6]
# [7, 8, 9, 10]
# [11, 12, 13, 14]
# [15, 16, 17, 18]
# [19, 20]

{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
