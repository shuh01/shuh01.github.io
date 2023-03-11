---
layout: single
title: Django - Models
categories: Django
use_math: true
toc: true
---

# Models

## Built-in Apps

- django.contrib.admin - Django administration
- django.contrib.auth
    
    ```powershell
     $python manage.py createsuperuser
    ```
    
- django.contrib.contenttypes
- django.contrib.sessions
- django.contrib.messages
- django.contrib.staticfiles

## Build own apps

```powershell
# Build my own app named 'products'
$python manage.py startapp products
```

### src/products/models.py

```python
from django.db import models

Class Products(models.Model):
	title = models.Textfield()
	description = models.Textfield()
	price = models.Textfield()	
```

### If models.py set, add ‘products’ in settings.py

```python
INSTALLED_APPS = [
	...
	...
	...
	'products',
]
```

### Makemigrations / Migrate

- Anytime when we make changes to models.py

```powershell
$ python manage.py makemigrations
$ python manage.py migrate
```

### Create new products using python shell

```powershell
$ python mangage.py shell
>>> from products.models import Product
>>> Product.objects.all() #This command shows the list of products in db

>>> Product.objects.create(title='product 2', description='another one', price='20', summary='sweet')
<Product: Product object (1)>

>>> Product.objects.all()
<QuerySet [<Product: Product object (1)>]>
```

## django.db.utils.OperationalError: no such table: main.auth_user__old

This error occurred when I tried to create a database on ‘Product’ table

### How I solved:

1. pip install django == 2.1.5
2. Delete original db.sqlite3
3. Migrate again by python manage.py makemigrations and then python manage.py migrate
4. Start the server again python manage.py  runserver
5. And you’re good to go!
