---
layout: post
title:  "HTTP Requests in Ruby"
author: Denis Kobare
date:   2022-03-22 06:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: concepts
technology: Ruby
permalink: "category/:categories/ruby/concepts/:year:month/:title"
---

### Definition

HTTP(Hypertext Transfer Protocol) is the fundamental protocol used to transfer data over the internet. An HTTP request typically contains three parts: the URL, headers and body. And this data is transferred using a specific method.

The GET and POST methods are most common types of request methods. Other methods include HEAD, PUT, DELETE, CONNECT, and OPTIONS. 

The get method is used to request information from a web server, while the post method is used to send information to a web server. The data transmitted through the HTTP client is usually in text, JSON, or XML format.


### Structure of an HTTP Request
An HTTP request consists of three parts:

1 . The request line: containing the method, uri and protocol version 

2 . The header: containing the metadata (optional)

3 . The body: containing the data (optional)


<br>
There are multiple ways to implement an http request in Ruby, this section will look at the standard built-in HTTP library.

<br>
### The Net::HTTP Library

Ruby has a built-in HTTP client that you can use to make requests to web servers. The Ruby HTTP client is a module called Net::HTTP. This module is used alongside the URI module. URI(Uniform Resource Identifier) contains two subsets, URN, which gives the name, and URL, which gives the location of the resource.
<br>
*NB: When using net/http module, you don't need to require 'uri' separately since net/http takes care of that.*

<br>
#### The GET Request
{% highlight ruby %}
# http.rb
require 'net/http'

uri = URI("https://jsonplaceholder.typicode.com/users")
response = Net::HTTP.get_response(uri)
puts response.body if response.is_a?(Net::HTTPSuccess)

{% endhighlight %}

This sends a GET request to the API endpoint located at typicode.com, that contains a list of users and their information. 

If you run the file on the console like so:

{% highlight console %}
  ruby http.rb
{% endhighlight %}

You get the following results:

{% highlight console %}

[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": {
      "street": "Kulas Light",
      "suite": "Apt. 556",
      "city": "Gwenborough",
      "zipcode": "92998-3874",
      "geo": {
        "lat": "-37.3159",
        "lng": "81.1496"
      }
    },
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org",
    "company": {
      "name": "Romaguera-Crona",
      "catchPhrase": "Multi-layered client-server neural-net",
      "bs": "harness real-time e-markets"
    }
  },
  {
    "id": 2,
    "name": "Ervin Howell",
    "username": "Antonette",
    "email": "Shanna@melissa.tv",
    "address": {
      "street": "Victor Plains",
      "suite": "Suite 879",
      "city": "Wisokyburgh",
      "zipcode": "90566-7771",
      "geo": {
        "lat": "-43.9509",
        "lng": "-34.4618"
      }
    },
    "phone": "010-692-6593 x09125",
    "website": "anastasia.net",
    "company": {
      "name": "Deckow-Crist",
      "catchPhrase": "Proactive didactic contingency",
      "bs": "synergize scalable supply-chains"
    }
  }
]

{% endhighlight %}


As you can see, the API here contains data that is serialized into JSON, therefore, you need to deserealize it into a hash object in order to work with it in Ruby. You should require the json library and then parse the result body like so:

{% highlight ruby %}

# http.rb
require 'net/http'
require 'json'

uri = URI("https://jsonplaceholder.typicode.com/users")
response = Net::HTTP.get_response(uri)
puts JSON.parse(response.body) if response.is_a?(Net::HTTPSuccess)

{% endhighlight %}

After parsing it, you get:

{% highlight console %}

{"id"=>1, "name"=>"Leanne Graham", "username"=>"Bret", "email"=>"Sincere@april.biz", "address"=>{"street"=>"Kulas Light", "suite"=>"Apt. 556", "city"=>"Gwenborough", "zipcode"=>"92998-3874", "geo"=>{"lat"=>"-37.3159", "lng"=>"81.1496"}}, "phone"=>"1-770-736-8031 x56442", "website"=>"hildegard.org", "company"=>{"name"=>"Romaguera-Crona", "catchPhrase"=>"Multi-layered client-server neural-net", "bs"=>"harness real-time e-markets"}}
{"id"=>2, "name"=>"Ervin Howell", "username"=>"Antonette", "email"=>"Shanna@melissa.tv", "address"=>{"street"=>"Victor Plains", "suite"=>"Suite 879", "city"=>"Wisokyburgh", "zipcode"=>"90566-7771", "geo"=>{"lat"=>"-43.9509", "lng"=>"-34.4618"}}, "phone"=>"010-692-6593 x09125", "website"=>"anastasia.net", "company"=>{"name"=>"Deckow-Crist", "catchPhrase"=>"Proactive didactic contingency", "bs"=>"synergize scalable supply-chains"}}

{% endhighlight %}

<br>
A GET request may contain a header. The header provides additional information about the request in the form of a key-value pair. For instance, an API may require that you provide authentication details on the headers as shown below:

{% highlight ruby %}

require 'net/http'
require 'json'

url = URI("https://compare-flight-prices.p.rapidapi.com/GetPricesAPI/GetPrices.aspx?SearchID=%3CREQUIRED%3E")

request = Net::HTTP.get_response(url, {'x-rapidapi-host' => 'compare-flight-prices.p.rapidapi.com', 'x-rapidapi-key' => 'ac89149b5bmsh5a1b0c18ca230fcp19f1b3jsn2ef80b09b974'})

puts JSON.parse(request.body)

{% endhighlight %}


<br>
#### The POST Request
A POST request always has a body and sometimes both a header and a body. The body contains the payload.
Sending a post request is also simple:

{% highlight ruby %}

require 'net/http'
require 'json'

uri = URI('https://jsonplaceholder.typicode.com/posts')
res = Net::HTTP.post_form(uri, 'title' => 'et ea vero quia laudantium autem', 'body' => "delectus reiciendis molestiae", 'userID' => 11)
puts res.body  if res.is_a?(Net::HTTPSuccess)

{% endhighlight %}


<br>
However, the API that you are posting to may require that you configure your request to send appropriate headers. Unfortunately Net::HTTP’s post_form method doesn’t support headers. 
The key to achieving that is to create a new Net::HTTP object and then send the headers hash as a third argument to its post method:

{% highlight ruby %}

require 'net/http'
require 'openssl'
require 'json'



uri = URI('https://sandbox.safaricom.co.ke/mpesa/stkpush/v1/processrequest')
http = Net::HTTP.new(uri.host, uri.port)
http.use_ssl = true



headers = {"Authentication" => "Bearer MdgtzT0ZFZWZBaFl1MTZQUERPVVBlS1V
            IcySAeE5WWko6QVlDWkdTQWJscm1IcmROdR==",
           "Content-Type" => "application/json"
           }
           
body = {

      "BusinessShortCode": 174379,

      "Password": "passwrd",
    
      "Timestamp": "2022-03-24 18:05:18.563720136 +0300",

      "TransactionType": "CustomerPayBillOnline",

      "Amount": 300000,

      "PartyA": 254700111000,

      "PartyB": 234563,

      "PhoneNumber": 254700111000,

      "CallBackURL": "https://2eb8-41-90-179-155/callback",

      "AccountReference": "S-Ential Technologies LLC",

      "TransactionDesc": "Payment of SaaS Product" 

      } 


response = http.post(uri.path, body.to_json, headers)

{% endhighlight %}

<br>
*Thanks for reading, see you in the next one!*
