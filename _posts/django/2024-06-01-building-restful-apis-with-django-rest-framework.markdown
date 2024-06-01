---
layout: post
title:  "Building RESTful APIs with Django Rest Framework"
author: Denis Kobare
date:   2024-06-01 15:30:00 +0300
img: /assets/img/svg/django.svg
categories: frameworks
sub_category: django
type: concepts
technology: Python
permalink: "category/:categories/django/concepts/:year:month/:title"
---


### Introduction

RESTful APIs (Application Programming Interfaces) have become the backbone of 
modern web applications, allowing different software components to communicate 
and exchange data seamlessly. Django Rest Framework (DRF) is a powerful toolkit 
that simplifies the process of creating RESTful APIs in Django, a popular Python 
web framework. In this comprehensive guide, we'll walk you through the process 
of building robust and scalable RESTful APIs using Django Rest Framework, 
covering key concepts such as serializers, authentication, permissions, and 
versioning.



<br>
### Prerequisites

Before we dive into creating RESTful APIs with Django Rest Framework, make sure 
you have the following prerequisites in place:

- Python and Django: Ensure you have Python installed on your system, preferably 
version 3.6 or above, along with Django.

- Virtual Environment (optional but recommended): It's a good practice to create 
a virtual environment for your project to isolate dependencies.

- Basic Understanding of Django: Familiarity with Django's models and views will 
be helpful.



<br>
### Step 1: Setting up a Django Project

If you're not already familiar with setting up a Django project, you can follow 
these steps:

<br>
#### Create a Virtual Environment (Optional):

{% highlight terminal %}

python3 -m venv myenv
source myenv/bin/activate

{% endhighlight %}


<br>
#### Install Django:

{% highlight terminal %}

pip install django

{% endhighlight %}


<br>
#### Create a New Django Project:

{% highlight terminal %}

    django-admin startproject myproject
    cd myproject

{% endhighlight %}



<br>
### Step 2: Installing Django Rest Framework

Now that you have a Django project, let's install Django Rest Framework.

<br>
#### Install Django Rest Framework:

{% highlight terminal %}

pip install djangorestframework

{% endhighlight %}


Add <span class="badge">rest_framework</span> to 
<span class="badge">INSTALLED_APPS</span>:

Open your project's <span class="badge">settings.py</span> file and add 
<span class="badge">rest_framework</span> to the INSTALLED_APPS list:

{% highlight python %}

    INSTALLED_APPS = [
        # ...
        'rest_framework',
    ]

{% endhighlight %}



<br>
### Step 3: Creating a Simple API

Let's start by creating a simple API that returns a list of items. We'll use 
Django's models and views along with Django Rest Framework's serializers and 
views.

<br>
#### Create a Django Model:

Open the models.py file in one of your app directories and define a simple model, 
for example, a "Todo" model:

<br>
{% highlight python %}

from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=100)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title

{% endhighlight %}




#### Create a Serializer:

In the same app directory, create a file called serializers.py and define a 
serializer for the "Todo" model:

<br>
{% highlight python %}

from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = ['id', 'title', 'completed']

{% endhighlight %}


<br>
#### Create a View:

Create a file called views.py in the same app directory and define a view using 
DRF's ModelViewSet:

<br>
{% highlight python %}

from rest_framework import viewsets
from .models import Todo
from .serializers import TodoSerializer

class TodoViewSet(viewsets.ModelViewSet):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

{% endhighlight %}


<br>
#### Configure URL Routing:

In your app's urls.py file, configure the URL routing for the API:

<br>
{% highlight python %}


    from django.urls import path, include
    from rest_framework.routers import DefaultRouter
    from .views import TodoViewSet

    router = DefaultRouter()
    router.register(r'todos', TodoViewSet)

    urlpatterns = [
        path('', include(router.urls)),
    ]

{% endhighlight %}



<br>
### Step 4: Running the Development Server

Now that you've created the API, let's run the development server and test it 
out.

<br>
#### Run the Development Server:

<br>
{% highlight terminal %}

 python manage.py runserver
    
{% endhighlight %}



<br>
#### Access the API:
Open your web browser or a tool like curl and access the API at 
http://127.0.0.1:8000/todos/. You should see a list of todos (initially empty).



<br>
### Step 5: Adding Authentication

Authentication is crucial for securing your API. We'll use DRF's built-in 
authentication classes to add token-based authentication.


<br>
#### Install Required Packages:

<br>
{% highlight terminal %}

pip install djangorestframework-simplejwt django-cors-headers

{% endhighlight %}


<br>
#### Configure Authentication and CORS:
In your project's settings.py file, add the following configurations:

<br>
{% highlight python %}


INSTALLED_APPS = [
    # ...
    'rest_framework.authtoken',
    'corsheaders',
]

MIDDLEWARE = [
    # ...
    'corsheaders.middleware.CorsMiddleware',
]

REST_FRAMEWORK = {
    # ...
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
    'SLIDING_TOKEN_LIFETIME': timedelta(days=30),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=60),
}

CORS_ALLOW_ALL_ORIGINS = True

{% endhighlight %}


<br>
#### Create API for User Authentication:

Create a file called views.py in your app directory and define API views for 
user registration and token generation:


<br>
{% highlight python %}

from rest_framework import generics, permissions
from rest_framework.response import Response
from rest_framework_simplejwt.tokens import RefreshToken
from django.contrib.auth.models import User
from .serializers import UserSerializer

class RegisterView(generics.CreateAPIView):
    queryset = User.objects.all()
    permission_classes = (permissions.AllowAny,)
    serializer_class = UserSerializer

class TokenObtainPairView(TokenObtainPairView):
    serializer_class = TokenObtainPairSerializer

class TokenRefreshView(TokenRefreshView):
    serializer_class = TokenRefreshSerializer

{% endhighlight %}


<br>
#### Configure URL Routing:

Update your app's urls.py file to include the new views:

<br>
{% highlight python %}

    from django.urls import path, include
    from .views import TodoViewSet, RegisterView, TokenObtainPairView, TokenRefreshView

    router = DefaultRouter()
    router.register(r'todos', TodoViewSet)

    urlpatterns = [
        path('register/', RegisterView.as_view(), name='register'),
        path('token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
        path('token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
        path('', include(router.urls)),
    ]

{% endhighlight %}



<br>
### Step 6: Adding Permissions and Versioning

Permissions control who can access your API, while versioning ensures smooth 
transitions as your API evolves. We'll implement both using DRF.


<br>
#### Adding Permissions:

In your app's views.py file, update the TodoViewSet to include permissions:

{% highlight python %}

from rest_framework import viewsets, permissions
from .models import Todo
from .serializers import TodoSerializer

class TodoViewSet(viewsets.ModelViewSet):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
    permission_classes = [permissions.IsAuthenticated]

{% endhighlight %}


<br>
#### Adding Versioning:

DRF makes it easy to handle API versioning. Update your app's views.py file to 
include versioning:

{% highlight python %}

    from rest_framework import viewsets, permissions
    from rest_framework.decorators import action
    from rest_framework.response import Response
    from .models import Todo
    from .serializers import TodoSerializer

    class TodoViewSet(viewsets.ModelViewSet):
        queryset = Todo.objects.all()
        serializer_class = TodoSerializer
        permission_classes = [permissions.IsAuthenticated]

        # ... (existing code)

        @action(detail=True, methods=['post'])
        def complete(self, request, pk=None):
            todo = self.get_object()
            todo.completed = True
            todo.save()
            serializer = self.get_serializer(todo)
            return Response(serializer.data)

{% endhighlight %}


### Conclusion

Congratulations! You've successfully built a robust and scalable RESTful API 
using Django Rest Framework. This comprehensive guide covered essential concepts 
such as serializers, authentication, permissions, and versioning. Remember that 
this is just the beginning, and there's much more you can do with Django Rest 
Framework to create feature-rich and secure APIs for your web applications. 
Explore the official Django Rest Framework documentation for more advanced 
topics and best practices.



<br>
*Thanks for reading, see you in the next one!*
