---
layout: post
title:  "React Router: Navigating Single-Page Applications"
author: Denis Kobare
date:   2024-09-01 15:30:00 +0300
img: /assets/img/svg/react.svg
categories: frameworks
sub_category: react
type: concepts
technology: JavaScript/React
permalink: "category/:categories/react/concepts/:year:month/:title"
---


### Introduction

In the world of modern web development, single-page applications (SPAs) have 
become the norm, providing a seamless user experience by updating content 
dynamically without full page reloads. React, a popular JavaScript library for 
building user interfaces, is commonly used for developing SPAs. To make 
navigation within these SPAs effortless and user-friendly, React Router comes to 
the rescue. React Router is a widely adopted library that allows you to handle 
routing and navigation in your React applications.

This section will guide you through the process of setting up React Router, 
creating routes, and handling navigation in your single-page application. We'll 
cover practical examples, code explanations, and best practices to ensure you 
have a solid foundation for building SPAs with React Router.



<br>
### Getting Started with React Router

Before diving into the code, make sure you have a React application set up. If 
not, you can create one using Create React App or any other method of your 
choice.

To get started with React Router, you need to install it as a dependency in your 
project. Open your terminal and run the following command:

{% highlight terminal %}

npm install react-router-dom

{% endhighlight %}


This command installs the react-router-dom package, which provides the tools 
needed for web routing with React.



<br>
### Setting Up Routes

In a single-page application, different "views" are typically represented by 
distinct components. React Router allows you to map these components to specific 
routes. Let's start by creating a basic navigation structure.

{% highlight jsx %}

import React from 'react';
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

// Components for different views
import Home from './Home';
import About from './About';
import Contact from './Contact';

function App() {
  return (
    <Router>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/contact">Contact</Link>
          </li>
        </ul>
      </nav>

      <Route path="/" exact component={Home} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
    </Router>
  );
}

export default App;

{% endhighlight %}


In this example, we import the necessary components from react-router-dom and 
define routes for the Home, About, and Contact views. The Route component is 
used to define a route, and the Link component is used to create navigation 
links.



<br>
### Route Parameters

Often, you'll need to pass dynamic values through your routes, such as a user ID 
or a product slug. React Router allows you to capture these dynamic segments 
using route parameters.

{% highlight jsx %}

import React from 'react';
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

// Components for different views
import UserProfile from './UserProfile';
import ProductDetails from './ProductDetails';

function App() {
  return (
    <Router>
      <nav>
        <ul>
          <li>
            <Link to="/user/123">User Profile</Link>
          </li>
          <li>
            <Link to="/product/abc">Product Details</Link>
          </li>
        </ul>
      </nav>

      <Route path="/user/:id" component={UserProfile} />
      <Route path="/product/:slug" component={ProductDetails} />
    </Router>
  );
}

export default App;

{% endhighlight %}


In this example, we use a colon (:) to denote route parameters. These parameters 
will be accessible in the component through the match prop.




<br>
### Programmatic Navigation

Sometimes, you need to navigate programmatically, for example, after a form 
submission or a button click. React Router provides the history object, which 
allows you to manipulate the browser's history and navigate programmatically.

{% highlight jsx %}

import React from 'react';
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

// Components for different views
import Login from './Login';

function App() {
  const handleLogin = () => {
    // Perform login logic here
    // Then navigate to the dashboard
    history.push('/dashboard');
  };

  return (
    <Router>
      <nav>
        <ul>
          <li>
            <Link to="/login">Login</Link>
          </li>
        </ul>
      </nav>

      <Route path="/login" render={(props) => <Login {...props} onLogin={handleLogin} />} />
      {/* Define other routes */}
    </Router>
  );
}

export default App;

{% endhighlight %}


In this example, the onLogin prop in the Login component is used to handle the 
login logic. After a successful login, the history.push() method is called to 
navigate to the dashboard.



<br>
### Nesting Routes

React Router allows you to nest routes within components. This is useful for 
creating complex layouts with nested views.

{% highlight jsx %}

import React from 'react';
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

// Components for different views
import Dashboard from './Dashboard';
import UserProfile from './UserProfile';
import UserSettings from './UserSettings';

function App() {
  return (
    <Router>
      <nav>
        <ul>
          <li>
            <Link to="/dashboard">Dashboard</Link>
          </li>
        </ul>
      </nav>

      <Route path="/dashboard" component={Dashboard} />
      {/* Other top-level routes */}
    </Router>
  );
}

export default App;

// Inside Dashboard component
function Dashboard({ match }) {
  return (
    <div>
      {/* Dashboard content */}
      <Route path={`${match.path}/user/:id`} component={UserProfile} />
      <Route path={`${match.path}/settings`} component={UserSettings} />
    </div>
  );
}

{% endhighlight %}


In this example, the UserProfile and UserSettings components are nested within 
the Dashboard component. The match.path property is used to construct nested 
paths.



<br>
### Conclusion

React Router is a powerful tool for handling navigation in single-page 
applications built with React. By following this tutorial, you've learned how to 
set up routes, handle route parameters, perform programmatic navigation, and 
nest routes within components. This knowledge will help you create seamless and 
user-friendly SPAs that provide a fantastic user experience.

Remember to explore the official React Router documentation for more advanced 
features and customization options. Happy coding!



<br>
*Thanks for reading, see you in the next one!*
