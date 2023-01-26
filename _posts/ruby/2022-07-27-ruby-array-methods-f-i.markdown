---
layout: post
title:  "Ruby Array Methods [F - I]"
author: Denis Kobare
date:   2022-07-26 14:05:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . fetch()
Returns the element at offset index.

{% highlight ruby %}

arr = [1, 2, 3, 4, 5]

# Array.fetch(index, default_value)
arr.fetch(5, 0)
# 0

# Array.fetch(index)
arr.fetch(1)
# 2


# When used with a block, the block is evaluated when the array has no value for the given index.
arr.fetch(5) {|i| p "Index #{i} is nil"}

# "Index 5 is nil"
# => "Index 5 is nil"                                                                 

{% endhighlight %}


<br>
### 2 . filter!
Calls the block, if given  with each element of array then removes from array those 
elements for which the block returns false or nil.

{% highlight ruby %}

# Array.filter!

arr = [1, 2, 3, 4, 5]

arr.filter! {|e| e if e > 2}
# => [3, 4, 5]


arr
# => [3, 4, 5]

{% endhighlight %}


<br>
### 3 . filter
Calls the block, if given, with each element of array then returns a new Array 
containing those elements of the array for which the block returns a truthy value.

{% highlight ruby %}

# Array.filter

arr = [1, 2, 3, 4, 5]

arr.filter {|e| e if e > 2}
# => [3, 4, 5]

arr
# => [1, 2, 3, 4, 5]

{% endhighlight %}


<br>
### 4 . fill()
Replaces specified elements in the array with specified objects then returns the array.

{% highlight ruby %}

# Array.fill(object, index, length)
# Array.fill(object, start_index)
# Array.fill(object)

arr = [1, 2, 3, 4, 5, [6, 8, 9], [8, 9, 10], 0]

arr.fill(11,5,1)
# => [1, 2, 3, 4, 5, 11, [8, 9, 10], 0]

arr.fill(11,5,2)
# => [1, 2, 3, 4, 5, 11, 11, 0]

arr.fill(11,6)
# => [1, 2, 3, 4, 5, [6, 8, 9], 11, 11]

arr.fill(11,6)
# => [11, 11, 11, 11, 11, 11, 11, 11]

{% endhighlight %}


<br>
### 5 . flatten()
Returns a new Array that is a recursive flattening of the array. Each non-Array 
element is unchanged. Each Array is replaced by its individual elements. With 
non-negative Integer argument level, flattens recursively through level levels

{% highlight ruby %}

# Array.flatten(level)

arr = [1, 2, 3, 4, 5, [6, 7, [8, 9, 10], 11, 12], 13]

arr.flatten(1)
# => [1, 2, 3, 4, 5, 6, 7, [8, 9, 10], 11, 12, 13]

arr.flatten(2)
# => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]

arr.flatten(-1)
# => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]

arr
# => [1, 2, 3, 4, 5, [6, 7, [8, 9, 10], 11, 12], 13]

{% endhighlight %}


<br>
### 6 . flatten!()
Replaces each nested array in the array with the elements from that array; returns the array if any changes, nil otherwise. With non-negative Integer argument level, flattens recursively through level levels. 

{% highlight ruby %}

# Array.flatten!(level)

arr = [1, 2, 3, 4, 5, [6, 7, [8, 9, 10], 11, 12], 13]

arr.flatten!(1)
# => [1, 2, 3, 4, 5, 6, 7, [8, 9, 10], 11, 12, 13]


arr
# => [1, 2, 3, 4, 5, 6, 7, [8, 9, 10], 11, 12, 13]

{% endhighlight %}



<br>
### 7 . find_index()
Returns the index of a specified element. When argument object is given but no 
block, returns the index of the first element element for which object == element.

{% highlight ruby %}

# Array.find_index(element)

arr = ["Ruby", "JS", "Python", "RoR"]

arr.find_index("RoR")
# => 3

{% endhighlight %}


<br>
### 8 . first()
Returns elements from the array; does not modify the array. When no argument is given, 
returns the first element. When non-negative Integer argument n is given, returns the first n
elements in a new Array.
                           
{% highlight ruby %}

# Array.first()

arr = ["Ruby", "JS", "Python", "RoR"]

arr.first

# => ["Ruby"]

arr.first(2)
# => ["Ruby", "JS", "Python"]

{% endhighlight %}


<br>
### 9 . find
Returns the first element for which the block returns a truthy value. With a 
block given, calls the block with successive elements of the collection then returns
the first element for which the block returns a truthy value.
 
{% highlight ruby %}

# Array.find {...}

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.find {|i| i == 4}
# => 4

{% endhighlight %}


<br>
### 10 . find_all
Returns an array containing elements selected by the block. With a block given, 
calls the block with successive elements then returns an array of those elements for
which the block returns a truthy value.

{% highlight ruby %}

# Array.find_all {...}

arr = [3, 4, "Ruby", "Rails"]

arr.find_all {|i| i.class == String}
# => ["Ruby", "Rails"]

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
### 11 . filter_map
Returns an array containing truthy elements returned by the block. With a block 
given, calls the block with successive elements then returns an array containing 
each truthy value returned by the block.
                           
{% highlight ruby %}

# Array.filter_map {...}

arr = [3, 4, "Ruby", "Rails"]

arr.filter_map {|i|  i.class == Integer}
# => [true, true]


{% endhighlight %}



<br>
### 12 . flat_map
Returns an array of flattened objects returned by the block. With a block given,
calls the block with successive elements; returns a flattened array of objects 
returned by the block.
                         
{% highlight ruby %}

# Array.flat_map

arr = [1, 2, 3, 4, 5, [6, 7, [8, 9, 10], 11, 12], 13]

arr.flat_map {|e| e }

# => [1, 2, 3, 4, 5, 6, 7, [8, 9, 10], 11, 12, 13]


arr2 = [[8, 9, 10], [11, 12, 13]]
# => [8, 9, 10, 22, 11, 12, 13, 22]

{% endhighlight %}



<br>
### 13 . freeze
Prevents further modifications to obj. A FrozenError will be raised if 
modification is attempted. There is no way to unfreeze a frozen object.
                                    
{% highlight ruby %}

# Array.freeze

arr = arr = ["Ruby", "JS", "Python", "RoR"].freeze

arr << "CSS"
# can't modify frozen Array: [1, 2, 3, 4, 5, [6, 7, [8, 9, 10], 11, 12], 13] (FrozenError)

{% endhighlight %}


<br>
### 14 . frozen?
Returns the freeze status of object.
                                    
{% highlight ruby %}

# Array.frozen?

arr = [1, false, 3, "Ruby", true]

arr.frozen?
# => false


arr.freeze

arr.frozen?
# => true

{% endhighlight %}


<br>
### 15 . grep
Returns an array of objects based elements of the array that match the given
pattern.

With no block given, returns an array containing each element for which
pattern === element is true.

With a block given, calls the block with each matching element and
returns an array containing each object returned by the block.

{% highlight ruby %}

# Array.grep(pattern)
# Array.grep(pattern) {...}

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.grep(Integer)
# => [3, 4]

arr.grep(/S/)
# => ["CSS", "JS"]

arr.grep(1..3)
# => [3]

arr.grep(1..3) {|e| e+2 }
# => [5]

{% endhighlight %}


<br>
### 16 . grep_v
Returns an array of objects based on elements of self that
don't match the given pattern.

With no block given, returns an array containing each element for which
pattern === element is false.

With a block given, calls the block with each non-matching element and
returns an array containing each object returned by the block.

{% highlight ruby %}

# Array.grep_v(pattern)
# Array.grep_v(pattern) {...}

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.grep_v(Integer)
# => ["Ruby", "CSS", "JS", "Rails"]

arr.grep_v(/S/)
# => [3, 4, "Ruby", "Rails"]

arr.grep_v(1..3)
# => [4, "Ruby", "CSS", "JS", "Rails"]

arr.grep_v(1..3) {|e| e+2 if e.class == Integer }
# => [6, nil, nil, nil, nil]

{% endhighlight %}


<br>
### 17 . group_by
With a block given returns a hash:

* Each key is a return value from the block.
* Each value is an array of those elements for which the block returned
  that key.
  
{% highlight ruby %}

# Array.group_by {...}

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.group_by {|e| e.class}
# => {Integer=>[3, 4], String=>["Ruby", "CSS", "JS", "Rails"]}

{% endhighlight %}



<br>
### 18 . hash
Returns the integer hash value for the array.

{% highlight ruby %}

# Array.hash

arr = [1, 2, 3]
arr2 = [0, 2, 3]


arr.hash
# => -1564449371230135169

arr2.hash
# => -4455935079606845780

NB: Two arrays with different content will have different hash code (and 
will return false when compared using eql?)

{% endhighlight %}



<br>
### 19 . include?
Returns true if for some index i in the array, object == array[i]; otherwise false.

{% highlight ruby %}

# Array.include?(object)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.include?("CSS")
# => true

{% endhighlight %}



<br>
### 20 . intersection
Returns a new Array containing each element found both in the array and in
all of the given Arrays other_arrays; duplicates are omitted; items are
compared using eql?
                          
{% highlight ruby %}

# Array.intersection(another_array)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr2 = [4, "JS", "Python"]

arr3 = [4, "JS", "CSS"]

arr.intersection(arr2)
# => [4, "JS"]

arr.intersection(arr2, arr3)
# => [4, "JS"]

{% endhighlight %}



<br>
### 21 . intersect?
Returns true if the array and other_ary have at least one element in
common, otherwise returns false.
                          
{% highlight ruby %}

# Array.intersect?(another_array)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr2 = [4, "JS", "Python"]

arr.intersect(arr2)
# => true

{% endhighlight %}



<br>
### 22 . insert
Inserts given objects before or after the element at Integer index offset; 
returns the array.

When index is non-negative, inserts all given objects before the element
at offset index. When index is negative, inserts all given objects after the
element at offset index+self.size.


Extends the array if index is beyond the array (index >= array.size).
                      
{% highlight ruby %}

# Array.insert(index, object)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]


arr.insert(3, "Python")
# => [3, 4, "Ruby", "Python", "CSS", "JS", "Rails"]

arr.insert(-5, "Jekyll")
# => [3, 4, "Jekyll", "Ruby", "CSS", "JS", "Rails"]

arr.insert(7, "Python")
# => [3, 4, "Ruby", "CSS", "JS", "Rails", nil, "Python"]

{% endhighlight %}



<br>
### 23 . index
This method is an alias for Array#find_index. When argument object is given but 
no block, returns the index of the first element element for which object == element.

When both argument object and a block are given, calls the block with
each successive element then returns the index of the first element for
which the block returns a truthy value.
                          
{% highlight ruby %}

# Array.index(object)

# Array.index(object) {...}

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.index("Ruby")
# => 2

arr.index {|e| e == "CSS" }
# => 3

{% endhighlight %}



<br>
### 24 . inspect
This is an alias for <span class="badge">to_s</span> method. Returns the new String formed by calling method <span class="badge">inspect</span> 
on each array element.
                          
{% highlight ruby %}

# Array.inspect

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.intersection(arr2)
# => "[3, 4, \"Ruby\", \"CSS\", \"JS\", \"Rails\"]"

{% endhighlight %}



<br>
### 25 . inject
This is an alias for the <span class="badge">reduce</span> method.
It returns an object formed from operands via either:

* A method named by symbol.
* A block to which each operand is passed.

Inject takes the first element of the array and uses it as the initial value for 
sum (if an argument is passed, it becomes the initial value). Then it takes the 
next element and adds them together. The the result becomes the initial sum value 
which is then added to the next element in the array until all elements have passed through the block.

{% highlight ruby %}

# Array.inject(initial_operand) {|memo, operand| ... }
# Array.inject(:+)

arr = [5, 10, 20]

arr.inject {|sum, n| sum + n}
# => 35

arr.inject(1) {|sum, n| sum + n}
# => 36

arr.inject(:+)
# => 35

arr.inject(1, :+)
# => 36

{% endhighlight %}



<br>
### 26 . itself
Returns the receiver.
                          
{% highlight ruby %}

# Array.itself

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.itself.object_id == arr.object_id
# => true


{% endhighlight %}


<br>
### 27 . instance_of?
Returns true if object is an instance of the given class.
                          
{% highlight ruby %}

# Array.instance_of?(class)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.instance_of?(Array)
# => true

{% endhighlight %}



<br>
### 28 . is_a?
This method is an alias for <span class="badge">kind_of?</span>.
                          
{% highlight ruby %}

# Array.is_a?(class)

arr = [3, 4, "Ruby", "CSS", "JS", "Rails"]

arr.is_a?(Array)
# => true

{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
