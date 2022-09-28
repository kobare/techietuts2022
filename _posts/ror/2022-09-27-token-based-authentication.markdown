---
layout: post
title:  "Token-Based Authentication"
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
A token is a string that the server generates for the client that can be passed along 
inside an http request, effectively transferring authentication-related data between a client 
and a server or between applications. 

The token stores user credentials and data in a secure manner and is able to 
verify that the data is correct and was not tampered with. It therefore simplifies the 
authentication process for known users.


<br>
### Components of an Authentication Token

An authentication token consists of three parts:

1 . The header: defines the token type being used, as well as the signing algorithm involved.

2 . The payload: defines the token issuer and the token’s 
expiration details and provides information about the user or other metadata.

3 . The signature: Guarantees the integrity and authenticity of the claims in the payload.


<br>
### Token-Based Authentication Process

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
### Types of Token-Based Authentication

1 . Connected Tokens

Connected tokens are physical devices that users must plug in to their computer or system for authentication. Examples include smart cards and Universal Serial Bus (USB) devices, as well as discs, drives, and keys.


<br>
2 . Contactless Tokens

Contactless tokens uses a wireless connection to access the system. An example of this is Microsoft’s ring device Token, which is a wearable ring that enables users to quickly and seamlessly log in to their Windows 10 device without entering a password.


<br>
3 . Disconnected Tokens

Disconnected tokens work by setting up a device to generate one-time access codes, which serve as part of 2FA or MFA. A good example of this is entering a code on a mobile phone for two-factor authentication (2FA).


<br> 
4 . JSON Web Token (JWT)

JSON Web Tokens (JWTs) enable secure communication between two parties through 
an open industry standard, Request For Comments 7519 (RFC 7519). This defines a 
simple, self-contained method for transmitting information between parties securely.

JWT standard uses JavaScript Object Notation (JSON) objects to transmit tokens 
between parties and the data shared is verified by a digital signature using an 
algorithm and public and private key pairing, which ensures optimal security. 
Data sent via Hypertext Transfer Protocol (HTTP) is kept secure by encryption.




<br>
*Thanks for reading, see you in the next one!*
