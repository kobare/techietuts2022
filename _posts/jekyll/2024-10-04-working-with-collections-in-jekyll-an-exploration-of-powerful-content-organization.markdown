---
layout: post
title:  "Working with Collections in Jekyll: An Exploration of Powerful Content Organization"
author: Denis Kobare
date:   2024-10-04 15:30:00 +0300
img: /assets/img/svg/jekyll.svg
categories: frameworks
sub_category: jekyll
type: concepts
technology: Ruby
permalink: "category/:categories/jekyll/concepts/:year:month/:title"
---


### Introduction

Jekyll, a popular static site generator, has gained significant traction in the 
web development community due to its simplicity and flexibility in creating 
websites and blogs. While Jekyll is primarily known for its ability to generate 
static content from Markdown and HTML files, it offers an advanced feature 
called collections that empowers developers to efficiently manage and present 
diverse types of content, such as blog posts, portfolio items, or products. In 
this section, we'll delve into Jekyll collections, exploring their use cases, 
implementation, and code explanations to help you harness this powerful feature 
effectively.



<br>
### Understanding Jekyll Collections

In Jekyll, a collection is a group of documents that share a common purpose or 
content structure. While the default collections are typically posts and pages, 
Jekyll allows developers to define custom collections, enabling more specialized 
content organization. This is particularly useful when dealing with content 
types that possess unique attributes or metadata.



### Use Cases for Custom Collections

- Portfolio Showcase: If you're building a portfolio website, you might want to 
showcase your projects or creative works. Using a custom collection allows you 
to create a consistent template for each portfolio item and display them in an 
organized manner.

- Product Catalog: For an e-commerce site, organizing products using collections 
can streamline the management of product information, images, and other 
attributes, facilitating the creation of a well-structured catalog.

- Documentation: When creating extensive documentation for a project, custom 
collections can help categorize different types of documentation, such as guides, 
tutorials, and API references, making it easier for users to navigate and find 
relevant information.



<br>
### Implementing Custom Collections

Let's walk through the process of creating and utilizing a custom collection 
named portfolio for a portfolio website.



<br>
#### Step 1: Define the Collection

In your Jekyll project's _config.yml file, define the new collection:

{% highlight yaml %}

collections:
  portfolio:
    output: true

{% endhighlight %}


This creates a collection named portfolio and enables its content to be output 
as HTML.



<br>
#### Step 2: Organize Content

Create a new directory in your project's root directory called _portfolio. 
Inside this directory, add Markdown files for each portfolio item. For instance, 
_portfolio/project1.md, _portfolio/project2.md, and so on.



<br>
#### Step 3: Front Matter

In each Markdown file, include front matter to define metadata specific to the 
portfolio item:

{% highlight markdown %}

---
title: Project 1
description: A brief overview of Project 1.
image: /assets/project1.jpg
---

{% endhighlight %}


<br>
#### Step 4: Create a Layout

Design a layout in your project's _layouts directory specifically for the 
portfolio items. Customize it to display the metadata you defined in the front 
matter.



<br>
#### Step 5: Rendering the Collection

In a page where you want to display the portfolio items, loop through the 
collection and render each item using the custom layout:


{% highlight liquid %}

---
layout: default
title: Portfolio
---

<h1>Portfolio</h1>

% for project in site.portfolio %
  <h2>{ project.title }</h2>
  <p>{ project.description }</p>
  <img src="{ project.image }" alt="{ project.title }">
% endfor %

{% endhighlight %}


<br>
### Conclusion

Jekyll collections provide an elegant solution for organizing and presenting 
various types of content beyond the default posts and pages. Custom collections 
enable developers to maintain a structured approach to content management, 
enhancing the user experience by facilitating content discovery and navigation. 
By following this tutorial, you've gained insights into the creation and 
utilization of custom collections in Jekyll, equipping you with the tools to 
implement this powerful feature in your projects. Whether you're building a 
portfolio, a product catalog, or a documentation hub, Jekyll collections can be 
your ally in creating well-organized and impactful websites.



<br>
*Thanks for reading, see you in the next one!*
