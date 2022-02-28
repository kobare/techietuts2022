---
layout: post
title:  "CSS Grid Layout"
author: Denis Kobare
date:   2022-02-28 3:33:00 +0300
img: /assets/img/svg/css3.svg
categories: frameworks
sub_category: css
type: concepts
technology: CSS
permalink: "category/:categories/css/concepts/:year:month/:title"
---

### Definition

The CSS Grid layout uses rows and columns to position elements on a web page without having to use floats.

It is an alternative to flexbox but most developers agree that the grid is more suitable for the general layout while flexbox is more effective when you narrow down to smaller layouts within the main layout.

<br>
1 . Set display to grid.

{% highlight css %}

  display: grid;
  
{% endhighlight %} 


<br>
2 . Declare your columns or rows using grid-template-column/row.

{% highlight css %}

  grid-template-column: 50% 25% 25%; /* declares 3 columns of those particular widths */

{% endhighlight %}  


<br>
3 . Use the fraction unit fr to divide columns or rows equally.

{% highlight css %}

  grid-template-column: 1fr 1fr 1fr 1fr;

  grid-template-column: repeat(4, 1fr); /* use the repeat function to achieve the same results */   

{% endhighlight %} 


<br>
4 . Use grid-auto-flow instead of grid-template-column/grid-template-rows if you want the width to auto-adjust depending on the number of columns or rows.

{% highlight css %}

  grid-auto-flow: column;

  /* Doing this: */
  grid-template-column: 50% 25% 25%; /* would not evenly re-distribute the with in case you reduced the number of columns to 2. */

{% endhighlight %} 


<br>
5 . Use grid-auto-flow with media queries to change the direction of the elements e.g from columns to rows whenever the screen size gets smaller.

{% highlight css %}

  .main-grid {
    grid-auto-flow: row;
  }

  @media (min-width: 55em){
    .main-grid {
      grid-auto-flow: column;
    }
  }

{% endhighlight %} 


<br>
6 . Use grid gap to space the elements instead of margins.

{% highlight css %}

  gap: 1.5rem;
  
{% endhighlight %} 


<br>
7 . No need to declare grid-template-row. Unless you absolutely have too, leave it out, let it be implicit. This will help you reduce the overheads incase you add more columns than you declared.


<br>
8 . Use span to extend columns or rows.

{% highlight css %}

  .grid-col-span-2 {
  grid-column: span 2;
  }
  
{% endhighlight %} 


<br>
9 . Re-position elements within the grid using grid-column-start, grid-column-end and the nth-child properties.

{% highlight css %}

  .main-grid:first-child {
    grid-column-start: 4;
    grid-column-end: 5;
  
    grid-column: 4 / 5    /* short hand for the two lines above */
    grid-row: 1 / span 2  
  }
  
{% endhighlight %} 


*Thanks for reading, see you in the next one!*
