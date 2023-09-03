---
layout: post
title:  "Authentication and Authorization in React Apps"
author: Denis Kobare
date:   2024-06-02 15:30:00 +0300
img: /assets/img/svg/react.svg
categories: frameworks
sub_category: react
type: concepts
technology: JavaScript/React
permalink: "category/:categories/react/concepts/:year:month/:title"
---


### Introduction

Building secure and reliable web applications is a top priority in today's 
digital landscape. Ensuring that only authorized users can access certain parts 
of your application while protecting user data is essential. In this section, 
we'll explore how to handle user authentication and authorization in a React 
application. We'll cover secure authentication methods, protecting routes, and 
managing user sessions.



Authentication is the process of verifying the identity of a user, while 
authorization is the process of determining what a user is allowed to do within 
the application. In a React application, it's crucial to implement robust 
authentication and authorization mechanisms to ensure that only authenticated 
users can access certain parts of the application and perform authorized actions.



<br>
### User Authentication

#### Using JWT (JSON Web Tokens)

JSON Web Tokens (JWT) are a popular method for handling user authentication. A 
JWT is a compact, URL-safe token that contains claims (user information) and is 
digitally signed for integrity. Here's a high-level overview of how JWT 
authentication works:

- The user logs in with valid credentials.

- The server generates a JWT containing user claims (e.g., user ID, roles, 
expiration).

- The JWT is sent to the client and stored (e.g., in a secure HTTP-only cookie).

- The client includes the JWT in the header of each authenticated request.

- The server verifies the JWT's signature and extracts user information.




<br>
#### Storing JWTs

It's essential to store JWTs securely to prevent unauthorized access. A common 
approach is to store JWTs in HTTP-only cookies. This prevents JavaScript code 
from accessing the token, reducing the risk of cross-site scripting (XSS) 
attacks.

#### Example: Implementing JWT Authentication

Let's create a simple React authentication example using JWT. We'll use the 
jsonwebtoken library to generate and verify tokens.

{% highlight jsx %}

// Install the required libraries
// npm install jsonwebtoken

// client/src/components/Login.js
import React, { useState } from 'react';
import jwt from 'jsonwebtoken';

const Login = () => {
  const [loggedIn, setLoggedIn] = useState(false);

  const handleLogin = () => {
    // Assume successful login
    const user = { id: 123, username: 'exampleUser' };
    const token = jwt.sign(user, 'secretKey', { expiresIn: '1h' });
    // Store the token (e.g., in a cookie)
    document.cookie = `token=${token}; path=/; HttpOnly; Secure`;
    setLoggedIn(true);
  };

  return (
    <div>
      {loggedIn ? (
        <p>Welcome, you are logged in!</p>
      ) : (
        <button onClick={handleLogin}>Log In</button>
      )}
    </div>
  );
};

export default Login;

{% endhighlight %}


In this example, we create a simple login component that generates a JWT upon 
successful login and stores it in an HTTP-only cookie.



<br>
### Authorization and Protected Routes

#### Restricting Access to Authenticated Users

Once we have user authentication in place, we can implement authorization by 
restricting access to certain routes or components based on user roles or 
permissions.

{% highlight jsx %}

// client/src/components/ProtectedRoute.js
import React from 'react';
import { Route, Redirect } from 'react-router-dom';

const ProtectedRoute = ({ component: Component, isAuthenticated, ...rest }) => (
  <Route
    {...rest}
    render={(props) =>
      isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
    }
  />
);

export default ProtectedRoute;

{% endhighlight %}


In this example, we create a ProtectedRoute component that redirects 
unauthenticated users to the login page.

#### Example: Protecting Routes

Let's protect a specific route using the ProtectedRoute component.

{% highlight jsx %}

// client/src/App.js
import React, { useState } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import ProtectedRoute from './components/ProtectedRoute';
import Login from './components/Login';
import Dashboard from './components/Dashboard';

const App = () => {
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  return (
    <Router>
      <Switch>
        <Route path="/login" component={Login} />
        <ProtectedRoute
          path="/dashboard"
          component={Dashboard}
          isAuthenticated={isAuthenticated}
        />
      </Switch>
    </Router>
  );
};

export default App;

{% endhighlight %}


In this example, the Dashboard component will only be accessible to 
authenticated users.


<br>
### Managing User Sessions

#### Implementing Session Management

Managing user sessions involves handling the duration of user authentication. 
It's common to set an expiration time for JWTs. When a token expires, the user 
needs to reauthenticate.

To handle token expiration, you can implement a token refresh mechanism. This 
involves issuing a new token (with a longer expiration) when the user's token is 
close to expiration.

#### Example: Managing User Sessions

Let's implement a basic token refresh mechanism in the ProtectedRoute component.

{% highlight jsx %}

// client/src/components/ProtectedRoute.js
import React, { useEffect, useState } from 'react';
import { Route, Redirect } from 'react-router-dom';
import jwt from 'jsonwebtoken';

const ProtectedRoute = ({ component: Component, ...rest }) => {
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  useEffect(() => {
    // Check if the token is valid
    const token = getStoredToken();
    if (token) {
      try {
        const decodedToken = jwt.verify(token, 'secretKey');
        if (decodedToken.exp * 1000 > Date.now()) {
          // Token is valid
          setIsAuthenticated(true);
        } else {
          // Token expired, handle token refresh here
          refreshAccessToken();
        }
      } catch (error) {
        // Invalid token, handle accordingly
        console.error('Invalid token');
      }
    }
  }, []);

  // Rest of the component implementation...

  return (
    <Route
      {...rest}
      render={(props) =>
        isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
      }
    />
  );
};

export default ProtectedRoute;

{% endhighlight %}


In this example, we use useEffect to check if the stored token is valid, and if 
it's close to expiration, we can implement the refreshAccessToken function to 
issue a new token.



<br>
### Conclusion

In this tutorial, we've covered the fundamentals of implementing user 
authentication and authorization in a React application. While this is a 
simplified example, it provides a solid foundation for building a more robust 
and secure authentication system in your projects. As you progress, consider 
additional security measures, like password hashing and user role management, 
to enhance your application's security.



<br>
*Thanks for reading, see you in the next one!*
