---
layout: post
title:  "JWT Authentication"
author: Denis Kobare
date:   2022-09-27 06:00:00 +0300
img: /assets/img/svg/ruby.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition
A JSON Web Token (JWT) is a string that the server generates for the client that can be passed along 
inside an http request, effectively transferring authentication-related data between a client 
and a server or between applications. 

The JWT stores user credentials and data in a secure manner and is able to 
verify that the data is correct and was not tampered with. It therefore simplifies the 
authentication process for known users.


<br>
### Components of an JWT

A JSON Web Token consists of three parts:

1 . The header: defines the token type being used, as well as the signing algorithm involved.

2 . The payload: defines the token issuer and the token’s 
expiration details and provides information about the user or other metadata.

3 . The signature: Guarantees the integrity and authenticity of the claims in the payload.


<br>
### JWT Authentication Process

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/token_authentication.png 1.3x,
  {{site.baseurl}}/assets/img/posts/token_authentication.png 2.8x
" alt="missing image">

The idea is that the client application exchanges authentication credentials for 
an authentication token and in subsequent requests, just sends the token. When 
the server receives the token, it can then look up the credentials of the user 
and determine whether or not it is authorized to the information it is requesting.


Tokens are usually given out with an expiration time, after which they become 
invalid and a new token needs to be obtained. The potential damage that can be 
done if a token is leaked is much smaller due to their short life span. A server 
can easily determine if a token is too old and decide to reject it if it doesnt view it as valid.


<br>
*Thanks for reading, see you in the next one!*
