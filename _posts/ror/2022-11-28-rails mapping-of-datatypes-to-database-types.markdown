---
layout: post
title:  "Rails Mapping of Datatypes to Database Types"
author: Denis Kobare
date:   2022-11-28 15:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

<style>

@media(min-width: 81em) {

  .mobile {
    display: none !important;
  }
  
  .grid-container {
    display: grid;
    grid-template-columns: auto auto auto auto auto;
    gap: 10px;
    background-color: #fff;
    padding: 10px;
  }

  .grid-container > div {
    background-color: #C5C6D0;
    text-align: center;
    padding: 20px 0;
    font-size: 18px;
  }

  li {
    list-style: none;
    text-align: left;
    padding-right: 10px;
  }

}


@media(max-width: 81em) {

  .desktop {
    display: none !important;
  }
  
  .grid-container {
    display: grid;
    grid-template-columns: auto auto;
    gap: 10px;
    background-color: #fff;
    padding: 10px;
  }

  .grid-container > div {
    background-color: #C5C6D0;
    text-align: center;
    padding: 20px 0;
    font-size: 18px;
  }

  li {
    list-style: none;
    text-align: left;
    padding-right: 10px;
  }

}

</style>


   
<div class="grid-container desktop">
  <div class="col-one">
   <h4 class="text-center grey-h">&nbsp;</h4>
     <ul>
    <li>:binary</li>   
    <li>:boolean</li>
    <li>:date</li>
    <li>:datetime</li>
    <li>:decimal</li>
    <li>:float</li>
    <li>:integer</li>
    <li>:string</li>
    <li>:text</li>
    <li>:time</li>
    <li>:timestamp</li>    
   </ul>  
  </div>
  
  <div>
   <h4 class="text-center grey-h">MySQL</h4>
     <ul>
    <li>blob</li>   
    <li>tinyint(1)</li>
    <li>date</li>
    <li>datetime</li>
    <li>decimal</li>
    <li>float</li>
    <li>int(11)</li>
    <li>varchar(255)</li>
    <li>text</li>
    <li>time</li>
    <li>datetime</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">Postgresql</h4>
     <ul>
    <li>bytea</li>   
    <li>boolean</li>
    <li>date</li>
    <li>timestamp</li>
    <li>decimal</li>
    <li>float</li>
    <li>integer</li>
    <li>note 1</li>
    <li>text</li>
    <li>time</li>
    <li>timestamp</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">SQLite</h4>
     <ul>
    <li>blob</li>   
    <li>boolean</li>
    <li>date</li>
    <li>datetime</li>
    <li>decimal</li>
    <li>float</li>
    <li>integer</li>
    <li>varchar(255)</li>
    <li>text</li>
    <li>datetime</li>
    <li>datetime</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">Oracle</h4>
     <ul>
    <li>blob</li>   
    <li>number(1)</li>
    <li>date</li>
    <li>date</li>
    <li>decimal</li>
    <li>number</li>
    <li>number(38)</li>
    <li>varchar2(255)</li>
    <li>clob</li>
    <li>date</li>
    <li>date</li>    
   </ul>  
  </div>

</div>


<!-- Mobile view -->


<div class="grid-container mobile">
  <div class="col-one">
   <h4 class="text-center grey-h">&nbsp;</h4>
     <ul>
    <li>:binary</li>   
    <li>:boolean</li>
    <li>:date</li>
    <li>:datetime</li>
    <li>:decimal</li>
    <li>:float</li>
    <li>:integer</li>
    <li>:string</li>
    <li>:text</li>
    <li>:time</li>
    <li>:timestamp</li>    
   </ul>  
  </div>
  
  <div>
   <h4 class="text-center grey-h">MySQL</h4>
     <ul>
    <li>blob</li>   
    <li>tinyint(1)</li>
    <li>date</li>
    <li>datetime</li>
    <li>decimal</li>
    <li>float</li>
    <li>int(11)</li>
    <li>varchar(255)</li>
    <li>text</li>
    <li>time</li>
    <li>datetime</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">Postgresql</h4>
     <ul>
    <li>bytea</li>   
    <li>boolean</li>
    <li>date</li>
    <li>timestamp</li>
    <li>decimal</li>
    <li>float</li>
    <li>integer</li>
    <li>note 1</li>
    <li>text</li>
    <li>time</li>
    <li>timestamp</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">SQLite</h4>
     <ul>
    <li>blob</li>   
    <li>boolean</li>
    <li>date</li>
    <li>datetime</li>
    <li>decimal</li>
    <li>float</li>
    <li>integer</li>
    <li>varchar(255)</li>
    <li>text</li>
    <li>datetime</li>
    <li>datetime</li>    
   </ul>  
  </div>

  <div>
   <h4 class="text-center grey-h">Oracle</h4>
     <ul>
    <li>blob</li>   
    <li>number(1)</li>
    <li>date</li>
    <li>date</li>
    <li>decimal</li>
    <li>number</li>
    <li>number(38)</li>
    <li>varchar2(255)</li>
    <li>clob</li>
    <li>date</li>
    <li>date</li>    
   </ul>  
  </div>

</div>


<br>
*Thanks for reading, see you in the next one!*
