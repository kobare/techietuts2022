---
layout: post
title:  "Ruby Array Methods [T - Z]"
author: Denis Kobare
date:   2022-10-24 04:00:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---


### 1 . to_h
Returns a new hash formed from the array. 

When a block is given, it calls the 
block with each array element and the block must return a 2-element Array whose 
two elements form a key-value pair in the returned Hash.

When no block is given, the array must be an array of 2-element sub-arrays, each 
sub-array is formed into a key-value pair in the new Hash.

{% highlight ruby %}

# Array.to_h { |item| [item, item] }

arr = [1, 2, 3, 4, 5]

arr2 = [["first", 1], ["second", 2], ["third", 3]]

arr.to_h { |item| [item, item] }
# {1=>1, 2=>2, 3=>3, 4=>4, 5=>5}


arr2.to_h
# {"first"=>1, "second"=>2, "third"=>3}

{% endhighlight %}


<br>
### 2 . transpose
Transposes the rows and columns in an array of arrays. The nested arrays must all 
be the same size.

{% highlight ruby %}

# Array.transpose

arr = [["first", 1], ["second", 2], ["third", 3]]

arr.transpose
# [["first", "second", "third"], [1, 2, 3]]

{% endhighlight %}


<br>
### 3 . take()
Returns a new array containing the first n element of the given array, where n is a
non-negative Integer. It does not modify the original array.

{% highlight ruby %}

# Array.take(n)

arr = [1, 2, 3, 4, 5]

arr.take(3)
# [1, 2, 3]

{% endhighlight %}


<br>
### 4 . take_while
Returns a new array containing zero or more leading elements of the array. It does 
not modify the original array.

With a block given, calls the block with each successive element of the array and
stops if the block returns false or nil. It then returns a new array containing 
those elements for which the block returned a truthy value.

{% highlight ruby %}

# Array.take_while {...}

arr = [1, 2, 3, 4, 5]

arr.take_while {|e| e > 4}
# []


arr.take_while {|e| e < 4}
# [1, 2, 3]

{% endhighlight %}


<br>
### 5 . to_s
This method is an alias for <span class="badge">inspect</span> method.

Returns the new String formed from each array element.

{% highlight ruby %}

# Array.to_s

arr = [1, 2, 3, 4, 5]

arr.to_s

# "[1, 2, 3, 4, 5]"

{% endhighlight %}


<br>
### 6 . to_set
Converts the array to a set.

{% highlight ruby %}

# Array.to_set

arr = ["a", "b", "c", "b"]


arr.to_set

#<Set: {"a", "b", "c"}>

{% endhighlight %}


<br>
### 7 . tally
Counts element occurences in the array.

With a hash argument, that hash is used for the tally (instead of a new hash), 
and is returned. This may be useful for accumulating tallies across multiple enumerables.

{% highlight ruby %}

# Array.tally

arr = ["a", "b", "c", "b", "a", "b"]

arr.tally
# => {"a"=>2, "b"=>3, "c"=>1}

arr2 = ["Ruby", "RoR", "React", "Ruby", "CSS"]
arr3 = ["JS", "CSS", "Python", "Ruby"]


hash = {}

arr.tally(hash)
# {"a"=>2, "b"=>3, "c"=>1}

arr2.tally(hash)
# {"a"=>2, "b"=>3, "c"=>1, "Ruby"=>2, "RoR"=>1, "React"=>1, "CSS"=>1}


arr3.tally(hash)
# {"a"=>2, "b"=>3, "c"=>1, "Ruby"=>3, "RoR"=>1, "React"=>1, "CSS"=>2, "JS"=>1, "Python"=>1}

{% endhighlight %}


<br>
### 8 . tap
Taps into a method chain, in order to perform operations on intermediate results
within the chain.

{% highlight ruby %}

# Array.tap

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

arr                      .tap {|x| puts "original: #{x}" }
  .to_a                  .tap {|x| puts "array:    #{x}" }
  .select {|x| x.even? } .tap {|x| puts "evens:    #{x}" }
  .map {|x| x*x }        .tap {|x| puts "squares:  #{x}" }

# original: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# array:    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# evens:    [2, 4, 6, 8, 10]
# squares:  [4, 16, 36, 64, 100]

{% endhighlight %}


<br>
### 9 . then
Yields the array to the block and returns the result of the block.

{% highlight ruby %}

# Array.then {...}

arr = ["Ruby", "RoR", "React"]

arr.then {|lang| "I like #{lang.first}"}

# "I like Ruby"

{% endhighlight %}


<br>
### 10 . to_enum
Protects an array from being modified by some_method.

{% highlight ruby %}

# some_method(Array.to_enum)

arr = [1, 2, 3, 4, 5]

some_method(arr.to_enum)

{% endhighlight %}


<br>
### 11 . union
Returns a new array that is the union of the given and all other arrays. 
Duplicates are removed, the order is preserved and items are compared using 
<span class="badge">eql?</span>

{% highlight ruby %}

# Array.union(other_array)

arr = [1, 2, 3]
arr2 = [4, 5, 6]

arr.union(arr2)
# [1, 2, 3, 4, 5, 6]

{% endhighlight %}


<br>
### 12 . unshift
Prepends the given objects to the array. Alias for <span class="badge">prepend</span>
method.

{% highlight ruby %}

# Array.unshift(obj)

arr = [{name: "Ruby"}, {name: "Python"}] 
arr2 = [1, 2, 3, 4, 5]

arr.prepend(arr2)
# [[1, 2, 3, 4, 5], {:name=>"Ruby"}, {:name=>"Python"}]

{% endhighlight %}


<br>
### 13 . uniq
Returns a new array containing those elements from the array that are not
duplicates, the first occurrence always being retained.

With a block given, it calls the block for each element and identifies (using
method eql?) and omits duplicate values, that is, those elements for which the 
block returns the same value.

{% highlight ruby %}

# Array.uniq
# Array.uniq {...}

arr = [1, 2, 3, 1, 3, 4]

arr.uniq
# [1, 2, 3, 4]


arr2 = ['a', 'aa', 'aaa', 'b', 'bb', 'bbb']
arr2.uniq {|element| element.size } 
# ["a", "aa", "aaa"]

{% endhighlight %}


<br>
### 14 . values_at
Returns a new array whose elements are the elements of the array at the given
Integer or Range indexes.

{% highlight ruby %}

# Array.values_at(*indexes)

arr = ["Ruby", "RoR", "React", "JS", "CSS", "Python"]

arr.values_at(5) 
# ["Python"]


arr.values_at(2, 3)
# ["React", "JS"]

{% endhighlight %}


<br>
### 15 . yield_self
Yields the array to the block and returns the result of the block.

{% highlight ruby %}

# Array.yield_self {...} 


arr = [1, 2, 3, 4, 5]

arr.yield_self {|s| s.last.to_s? } 
# 5

{% endhighlight %}


<br>
### 16 . zip
Merges elements of the array with corresponding elements from each argument.

{% highlight ruby %}

# Array.zip(other_array)


arr = ["Ruby", "Ruby on Rails" "CSS", "Python", "React"]
arr2 = [1, 2, 3, 4, 5]

arr.zip(arr2)
# [["Ruby", 1], ["Ruby on RailsCSS", 2], ["Python", 3], ["React", 4]]

{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
