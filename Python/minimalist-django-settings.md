---
title: Minimalist Django configuration
author: db0
abstract: Using Django for very simple web pages
---

[Django](https://www.djangoproject.com/) is my favorite web framework so far. But when I wanted
to publish a very simple site as an example to illustrate a concept, I was a bit disappointed to
see that it was kind of hard to find the interesting pieces of code if you're not familiar with
the Django environment. I started searching for a way to make a simple website with Django
without all the fancy features and the complex files tree. Here's  what I came up with:

##### Files tree

```
- manage.py
- settings.py
- templates/
  - index.html
  - about.html
- urls.py
- views.py
```

##### manage.py

```python
#!/usr/bin/env python
import os, sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "settings")
    from django.core.management import execute_from_command_line
    execute_from_command_line(sys.argv)
```

##### settings.py

```python
import os

DEBUG = True
ROOT_URLCONF = 'urls'
SECRET_KEY = '%no*6sl#g&34vg61&4zs*pjk+gb9_ma-oua!@h1o0wn3fxb!k#'
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            os.path.dirname(os.path.abspath(__file__)) + '/templates/',
        ],
    },
]
```

##### urls.py

```python
from django.conf.urls import url
from django.views.generic import TemplateView
import views

urlpatterns = [
    url(r'^$', views.index),
    url(r'^about/$', TemplateView.as_view(template_name='about.html')),
]
```

##### views.py

```python
from django.shortcuts import render

def index(request):
    return render(request, 'index.html', {
      'title': 'Welcome!',
    })
```

##### templates/index.html

```html
<!doctype html>
<html lang=en>
  <head>
    <meta charset=utf-8>
    <title>Minimalist Django site</title>
  </head>
  <body>
    <h1>{{ title }}</h1>
  </body>
</html>
```
