---
layout: single
title: Django - Views
categories: Django
use_math: true
toc: true
---

# Views

# Default Homepage to Custom Homepage

```python
$ python manage.py startapp pages #Create pages application first

Add 'pages' to installed_apps in settings.py
```

### /pages/views.py

```python
from django.http import HttpResponse
from django.shortcuts import render

def home_view(*args, **kwargs):
	return HttpResponse("<h1>Hello World</h1>"_
```

### /trydjango/urls.py

```python
#add
from pages.views import home_view

urlpatterns = [
	path('', home_view, name='home'),
	path('admin/', admin.site.urls),
]
```


## URL Routing and Requests

### /trydnango/urls.py

```python
urlpatterns = [
	...,
	path('contact/', contact_view),
	...,
]
```

http://127.0.0.8000/contact/ → This will show us the contact_view page

 

# Django Templates

```powershell
$ mkdir templates
$ cd templates

In templates folder, create html files you need
#home.html
<h1>Hello World</h1>
<p>This is a template</p>
```

### /pages/views.py

```python
def home_view(request, *args, **kwargs):
	return render(request, "home.html", {})

def contact_view(request, *args, **kwargs):
	return render(request, "contact,html", {})
```

### /trydjango/settings.py

```python
At 'DIRS' in TEMPLATES:

'DIRS': [os.path.join(BASE_DIR, "templates")],
```

### trydjango/urls.py

```python
urlpatterns = [
	path('', home_view),
	path('contact/', contact_view),
	...,
]
```

By doing this, django will automatically render a HTML to a website.

## Templates Engine Basics

### Create a base.html and set it as a basic structure of other htmls.

```html
#base.html

<!DOCTYPE html>
<html>
  <head>
    <title>Sung is doing try django</title>
  </head>
  <body>
    {% block content %} 
				replace me    -> Where the actual content will be in other htmls.
		{% endblock %}
  </body>
</html>

#home.html

{% extends 'base.html' %} 
{% block content %}
	<h1>Hello world</h1>
	{{ result.user }}
	<p>This is a template</p>
{% endblock %}
```

## Include template tag

### /pages/views.py

- Create a dictionary “my_context” to pass the data to “about.html”

```python
def about_view(request, *args, **kwargs):
	my_context = {
		"my_text": "This is about us",
    "my_number": 123,
    "my_list": [12, 23, 34]
  }
return render(request, "about.html", my_context)
```

### /templates/about.html

```html
{% raw %}
{% extends 'base.html' %} 
{% block content %}
	<h1>About page</h1>
	<p>This is a template</p>
	<p>
		{{ my_text }}, {{ my_number }}, {{ my_list }}
	</p>
{% endblock %}
{% endraw %}
```

## For loop in a template

### /templates/about.html

```html
<ul>
  {% for my_sub_item in my_list %}
  <li>{{forloop.counter}} - {{my_sub_item}}</li>
  {% endfor %}
</ul>
```

## Using conditions in a template

### /pages/views.py

```html
{% raw %}
<ul>
  {% for abc in my_list %} 
    {% if abc == 312 %}
        <li>{{forloop.counter}} - {{abc|add:22}}</li>
    {% elif abc == "ABC" %}
        <li>This is not the network</li>
    {% else %}
        <li>{{forloop.counter}} - {{abc}}</li>
    {% endif %}
  {% endfor %}
</ul>
{% endraw %}
```

## Template tags and filters

- [https://docs.djangoproject.com/en/4.1/ref/templates/builtins/](https://docs.djangoproject.com/en/4.1/ref/templates/builtins/)

## Render data from database with a model

### /products/views.py

```python
from .models import Product

def product_detail_view(request):
	obj = Product.objects.get(id=1)
	context = {
		'object':obj
	}
	return redner(request, "products/detail.html", context)
```

### /templates/product/detail.html

```html
{% raw %}
{% extends 'base.html' %}

{% block content %}
	<h1>{{object.title}}</h1>
	<p>{%if object.description %}{{object.description}}{% else %}Description Coming Soon{% endif %}</p>

{{object.price}}

{% endblock %}
{% endraw %}
```

### /trydjango/urls.py

```python
from products.views import product_detail_view

urlpatterns = [
	...,
	...,
	path('product/', product_detail_view)
	...,
]
```
