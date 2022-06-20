---
layout: post
title:  "Ruby Array Methods [A - C]"
author: Denis Kobare
date:   2022-06-12 06:30:00 +0300
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
truthy elements, false otherwise.

{% highlight ruby %}

# Array.all?

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


arr.all?(Integer)

# true


%w[Ruby Jekyll Rails].all? { |word| word.length >= 4 } 

# true
{% endhighlight %}

<br>
### 5 . any?
With no block given and no argument, returns true if self has any truthy 
element, false otherwise.

{% highlight ruby %}

# Array.any?

arr = [1, 2, 3, 4, 5]

arr.any?

# true


%w[Ruby Jekyll Rails].any? { |word| word.length >= 7 }

# false
{% endhighlight %}


<br>
### 6 . abbrev
Calculates the set of unambiguous abbreviations for the strings.

{% highlight ruby %}

# Array.abbrev

%w[Ruby Jekyll Rails].abbrev

# => 
{"Ruby"=>"Ruby",                                                                     
 "Rub"=>"Ruby",                                                                      
 "Ru"=>"Ruby",                                                                       
 "Jekyll"=>"Jekyll",                                                                 
 "Jekyl"=>"Jekyll",                                                                  
 "Jeky"=>"Jekyll",                                                                   
 "Jek"=>"Jekyll",                                                                    
 "Je"=>"Jekyll",                                                                     
 "J"=>"Jekyll",                                                                      
 "Rails"=>"Rails",                                                                   
 "Rail"=>"Rails",                                                                    
 "Rai"=>"Rails",                                                                     
 "Ra"=>"Rails"}   
{% endhighlight %}


<br>
### 7 . bsearch_index
Finds the index of the array value that meets the given condition.
Returns a new Enumerator if no block given.

{% highlight ruby %}

# Array.bsearch_index

arr = [1, 2, 3, 4]

arr.bsearch_index {|a| a>=3}

#  2

{% endhighlight %}


<br>
### 8 . bsearch
Returns an element selected by a binary search.
Returns a new Enumerator if no block given.

{% highlight ruby %}

# Array.bsearch

arr = [1, 2, 3, 4]

arr.bsearch {|a| a>=3}

#  3

{% endhighlight %}


<br>
### 9 . collect
Returns a new array, built from the output of the block. This allows performing 
operations on values in the original array, and storing the output of that.

{% highlight ruby %}

# Array.collect

arr = [1, 2, 3, 4]

arr.collect {|a| (a>=3 ? a+1 : a)}

# [1, 2, 4, 5]


# You can save yourself from re-assigning it to the same variable with collect!

arr.collect! {|a| (a>=3 ? a+1 : a)}

{% endhighlight %}


<br>
### 10 . compact
Removes all nil elements from an array.

{% highlight ruby %}

# Array.compact

arr = [1, 2, 3, 4, 0, false, nil]

arr.compact

# [1, 2, 3, 4, 0, false]


# You can save yourself from re-assigning it to the same variable with compact!

arr.compact!

{% endhighlight %}


<br>
### 11 . count
Returns a count of specified elements. With no argument and no block, returns 
the count of all elements.

{% highlight ruby %}

# Array.count

arr = [1, 2, 3, 4, 0, 5, 6]

arr.count

# 7


arr.count {|a| a<=5}

# 6

{% endhighlight %}



<br>
### 12 . combination
When called with a block yields a two-dimensional array consisting of all 
sequences of a collection of numbers. Unlike permutation, order is disregarded 
in combinations. For instance, [1,2,3] is the same as [3,2,1].

{% highlight ruby %}

# Array.combination

arr = [1, 2, 3, 4]

arr.combination(3).to_a

# [[1, 2, 3], [1, 2, 4], [1, 3, 4], [2, 3, 4]]


arr.combination(4).to_a

# [[1, 2, 3, 4]]

{% endhighlight %}



<br>
### 13 . cycle()
Calls the given block n times for each item in the array.
Returns nil if the loop has finished without getting interrupted.

{% highlight ruby %}

# Array.cycle()

arr = [1, 2, 3, 4]

arr.cycle(3) {|a| puts a}

# 1
# 2
# 3
# 4
# 1
# 2
# 3
# 4
# 1
# 2
# 3
# 4

{% endhighlight %}


<br>
### 14 . clear
Removes all elements from array.

{% highlight ruby %}

# Array.clear

arr = [1, 2, 3, 4]

arr.clear

# []

{% endhighlight %}


<br>
### 15 . concat()
Adds to array all elements from each Array in other_arrays. 

{% highlight ruby %}

# Array.concat(another_array)

arr = [1, 2, 3, 4]

arr2 = [5, 6, 7, 8]

arr.concat(arr2)

# [1, 2, 3, 4, 5, 6, 7, 8]

{% endhighlight %}


<br>
### 16 . chunk
Enumerates over the items, chunking them together based on the return value of the block.
Consecutive elements which return the same block value are chunked together.

If the block value is the same but the elements are not consecutive, then a new group is created.

{% highlight ruby %}

# Array.chunk

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.chunk {|a| a.even? }.each { |even, ary| p [even, ary]}

# [true, [4]]
# [false, [7, 1]]
# [true, [2]]
# [false, [3]]
# [true, [4]]
# [false, [5]]
# [true, [6]]
# [false, [7]]
# [true, [8]]
# [false, [9]]
# => nil

{% endhighlight %}


<br>
### 17 . chunk_while
Returns elements organized into chunks as specified by the given block.

{% highlight ruby %}

# Array.chunk_while

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# For example, one-by-one increasing subsequence can be chunked as follows:
arr.chunk_while {|m, n| m+1 == n }.to_a

# [=> [[4], [7], [1, 2, 3, 4, 5, 6, 7, 8, 9]]

{% endhighlight %}



<br>
### 18 . chain
Adds to array all elements from each Array in other_arrays.

{% highlight ruby %}

# Array.chain(another_array)

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.chain([10, 11]).to_a

# [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

{% endhighlight %}



<br>
### 19 . collect_concat
Returns a new array with the concatenated results of running block once for 
every element in enum. If no block is given, an enumerator is returned instead.

{% highlight ruby %}

# Array.collect_concat

arr = [4, 10]
  
arr.collect_concat { |el| [2*el, 3*el] }

# [8, 12, 20, 30]

{% endhighlight %}



<br>
### 20 . class
Returns the class of obj.
                          
{% highlight ruby %}

# Array.class

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9]

arr.class

# Array

{% endhighlight %}



<br>
### 21 . clone
Produces a shallow copy of object. The instance variables of object are copied, 
but not the objects they reference. #clone copies the frozen value state of obj, 
unless the :freeze keyword argument is given with a false or true value.
                          
{% highlight ruby %}

# Array.clone

arr = [4, 7, 1, 2, 3, 4, 5, 6, 7, 8, 9].freeze

arr.frozen?

# true

arr.object_id

# 5597800


clone_copy = arr.clone

clone_copy.frozen?

# true


clone_copy.object_id

# 5609580


# In comparisson to dup

dup_copy = arr.dup

dup_copy.frozen?

# false


dup_copy.object_id

# 5621360

{% endhighlight %}

<br>
*Thanks for reading, see you in the next one!*
