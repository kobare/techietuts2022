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

### Definition

Base64 encoding is a process that takes binary data and converts it into an ASCII string. This string can then be easily embedded into a URL or email without taking up too much space. The recipient can then decode the string back to its original form.


Base64 decoding is the reverse process. It takes an ASCII string and converts it back to binary data. This can be useful for retrieving files that have been attached to an email or for displaying images on a web page.

<br>
### Base64 Applications

There are many different ways to use Base64 encoding. Some of the most common use cases include:

1 . Embedding images into emails.

2 . Reducing the size of files and data compression.

3 . Encode data in a URL.

4 . Obfuscate data. This means that the data can be encoded in such a way that it is difficult to understand without the proper decoding algorithm. Obfuscating data can be helpful for protecting sensitive information like login passwords.

<br>
### How To Base64 Encode/Decode In Ruby

The simplest ways to do a base64 encode/decode are:

1 . Use the built-in pack method from array class. The pack method returns a base64 encoded string (given the parameter “m”). The String class has a method unpack that likewise decodes(base64) the string.

{% highlight ruby %}
str = "QE3OWEefBhYu16PPDOUBeKUHqqZxNVZJ"

encoded_string = [str].pack("m")

puts encoded_string 
# UUUzT1dFZWZCaFl1MTZQUERPVUJlS1VIcXFaeE5WWko=

decoded_string = encoded_string.unpack("m")

puts decoded_string 
# QE3OWEefBhYu16PPDOUBeKUHqqZxNVZJ
{% endhighlight %} 


<br>
2 . Use the base64 library

{% highlight ruby %}
require "base64"

str = "QE3OWEefBhYu16PPDOUBeKUHqqZxNVZJ"

encoded_string = Base64.encode64(str)

puts encoded_string 
# UUUzT1dFZWZCaFl1MTZQUERPVUJlS1VIcXFaeE5WWko=

decoded_string = Base64.decode64(encoded_string)
# QE3OWEefBhYu16PPDOUBeKUHqqZxNVZJ

puts decoded_string 

{% endhighlight %} 


<br>
*Thanks for reading, see you in the next one!*
