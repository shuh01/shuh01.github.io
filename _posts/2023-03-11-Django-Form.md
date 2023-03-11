---
layout: single
title: Django - Form
categories: Django
use_math: true
toc: true
---
# Forms

## Django Model Forms

### Create forms.py

```python
from django import forms

from .models import Product

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = [
            'title',
            'description',
            'price',
        ]
```

### Update views.py

```python
from .forms import ProductForm

def product_create_view(request):
    form = ProductForm(request.POST or None)
    if form.is_valid():
        form.save()
        form = ProductForm()
    context = {
        'form': form

    }
    return render(request, "products/product_create.html", context)
```

### Create product_create.html

```python
{% extends 'base.html' %}

{% block content %}
<form method="Post"> {% csrf_token %}
    {{ form.as_p}}
    <input type = 'submit' value='Save' />
</form>

{% endblock %}
```

### Update urls.py

```python
from products.views import product_detail_view, product_create_view

urlpatterns = [
	path('create/', product_create_view),
]
```

## RAW HTML Form

### views.py

```python
def product_create_view(request):
    title = request.POST.get('title')
		if request.method == "POST":
        my_new_title = request.POST.get('title')
        print(my_new_title)
    context = {}
    return render(request, "products/product_create.html", context)
```

### product_create.html

```html
{% extends 'base.html' %}

{% block content %}

<form action="." method="POST">{% csrf_token %}
    <input type="text" name="title" placeholder="Your Title"/>
    <input type = 'submit' value='Save' />
</form>

{% endblock %}
```

## Pure Django Form

### views.py

```python
def product_create_view(request):
    my_form = RawProductForm()
    if request.method == "POST":
        my_form = RawProductForm(request.POST)
        if my_form.is_valid():
            #now the data is good
            print(my_form.cleaned_data)
            Product.objects.create(**my_form.cleaned_data)
        else:
            print(my_form.errors)
    context = {
        "form": my_form

    }
    return render(request, "products/product_create.html", context)
```

### product_create.html

```python
{% extends 'base.html' %}

{% block content %}

<form action="." method="POST">{% csrf_token %}
    {{ form.as_p }}
    <input type = 'submit' value='Save' />
</form>

{% endblock %}
```

### forms.py

```python
from django import forms

from .models import Product

class RawProductForm(forms.Form):
    title = forms.CharField()
    description = forms.CharField()       
    price = forms.DecimalField()
```

## Form Widgets

### forms.py

```python
class RawProductForm(forms.Form):
    title = forms.CharField(label='', widget = forms.TextInput(
        attrs={
            "placeholder": "Your title"
        }
    ))
    description = forms.CharField(required=False, widget=forms.Textarea(
        attrs={
            "placeholder": "Your description",
            "class": "new-class-name two",
            "id" : "my-id-for-textarea",
            "rows": 20,
            "cols": 120,

        }
    ))       
    price = forms.DecimalField(initial=199.99)
```

## Form Validation Methods

### views.py

```python
def product_create_view(request):
    form = ProductForm(request.POST or None)
    if form.is_valid():
        form.save()
        form = ProductForm()
    context = {
        'form': form

    }
    return render(request, "products/product_create.html", context)
```

### forms.py

- if statement and raise forms.ValidationError

```python
class ProductForm(forms.ModelForm):
    title = forms.CharField(label='', widget = forms.TextInput(
        attrs={
            "placeholder": "Your title"
        }
    ))
    email = forms.EmailField()
    description = forms.CharField(required=False, widget=forms.Textarea(
        attrs={
            "placeholder": "Your description",
            "class": "new-class-name two",
            "id" : "my-id-for-textarea",
            "rows": 20,
            "cols": 120,

        }
    ))       
    price = forms.DecimalField(initial=199.99)

    class Meta:
        model = Product
        fields = [
            'title',
            'description',
            'price',
        ]
    def clean_title(self, *args, **kwargs):
        title = self.cleaned_data.get("title")
        if not "Sung" in title:
            raise forms.ValidationError("This is not a valid title")
        if not "Yerim" in title:
            raise forms.ValidationError("This is not a valid title")
        return title
    
    def clean_email(self, *args, **kwargs):
        email = self.cleaned_data.get("email")
        if not email.endswith("edu"):
            raise forms.ValidationError("This is not a valid email")
        return email
```
