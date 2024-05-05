---
layout: post
title:  "Unlocking the Power of Custom CSS Properties (Variables) for Dynamic and Reusable Styles"
author: Denis Kobare
date:   2024-05-05 11:00:00 +0300
img: /assets/img/svg/css3.svg
categories: programming
sub_category: css
type: concepts
technology: CSS
permalink: "category/:categories/css/concepts/:year:month/:title"
---

### Definition

CSS (Cascading Style Sheets) is an essential tool for web developers to control 
the presentation and layout of web pages. It allows developers to style HTML 
elements, making websites visually appealing and user-friendly. While CSS 
provides a powerful set of predefined properties, custom CSS properties, also 
known as CSS variables, take this power to the next level by offering dynamic 
and reusable styles. In this section, we'll delve into the benefits and use 
cases of custom CSS properties, and we'll explore examples of theming and 
dynamic UI updates.


### Benefits of Custom CSS Properties (Variables)

- Reusability: One of the key advantages of using custom CSS properties is the 
ability to reuse styles across your website. Imagine defining a color palette or 
a set of font sizes once and then using them consistently throughout your 
project. This not only ensures consistency but also simplifies maintenance. If 
you decide to change a color or a font size, you can update the variable's value, 
and it will automatically propagate to all the elements that use that variable.

- Dynamic Theming: Custom CSS properties are particularly useful for theming. By 
defining variables for various aspects of your design, such as background colors, 
text colors, and borders, you can easily switch between different themes without 
changing individual CSS rules. This flexibility is incredibly powerful when 
catering to user preferences or creating a dark mode.

- Ease of Maintenance: As your project grows, maintaining a large CSS codebase 
can become challenging. Custom CSS properties promote clean and organized code. 
By centralizing style-related values in variables, you can quickly make global 
updates. This helps reduce the risk of errors and ensures that your styles 
remain consistent.

- Dynamic UI Updates: Custom CSS properties can be manipulated using JavaScript, 
enabling dynamic UI updates. This opens the door to interactive user experiences. 
For example, you can create a slider that changes the background color of a 
section in real-time by modifying the value of a custom CSS property.



<br>
### Using Custom CSS Properties

Let's dive into some practical examples to demonstrate how to use custom CSS 
properties for theming and dynamic UI updates.


#### Theming with Custom CSS Properties

{% highlight css %}

/* Define your theme variables */
:root {
  --primary-color: #3498db;
  --secondary-color: #e74c3c;
  --font-size: 16px;
}

/* Apply the theme to elements */
.button {
  background-color: var(--primary-color);
  color: white;
  font-size: var(--font-size);
  padding: 10px 20px;
}

/* Switching to a different theme */
.dark-mode {
  --primary-color: #2c3e50;
  --secondary-color: #f39c12;
  /* ... additional theme changes ... */
}

{% endhighlight %}


In this example, we define theme variables using the :root pseudo-class. These 
variables are then used to style the .button element. To switch to a different 
theme, simply add the dark-mode class to the root element and update the 
variable values accordingly.


<br>
#### Dynamic UI Updates with Custom CSS Properties

{% highlight html %}

<!DOCTYPE html>
<html>
<head>
<style>
/* Define a dynamic property */
:root {
  --bg-color: #f1f1f1;
}

/* Apply the property to an element */
.section {
  background-color: var(--bg-color);
  padding: 20px;
}
</style>
</head>
<body>

<div class="section">
  <p>This is a section with a dynamic background color.</p>
</div>

<input type="color" id="color-picker">
<script>
// Update the dynamic property based on user input
document.getElementById("color-picker").addEventListener("input", function(event) {
  document.documentElement.style.setProperty("--bg-color", event.target.value);
});
</script>

</body>
</html>

{% endhighlight %}


In this example, we define a custom CSS property --bg-color that controls the 
background color of the .section element. We also include an <span class="badge">input type="color"</span> 
element, which allows users to pick a color. The JavaScript code listens for 
changes to the color picker and dynamically updates the --bg-color property, 
resulting in a live preview of the color change in the UI.



<br>
### Conclusion

Custom CSS properties (variables) are a game-changer in modern web development. 
They promote reusability, enable dynamic theming, ease maintenance, and allow 
for dynamic UI updates. By incorporating custom CSS properties into your 
projects, you'll enhance your styling workflow and create more flexible and 
interactive web experiences. Embrace the power of custom CSS properties and take 
your web design skills to the next level.



<br>
*Thanks for reading, see you in the next one!*
