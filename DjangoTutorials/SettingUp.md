# Basic tutorial about Django

## setting up the django and create a django project

1. create a new virtual environment files `env/` for django

```linux=
python3 -m ven env
```

2. activate the virtual environment

```linux=
source env/bin/activate
```

3. after finish all project want to deploy, generate the `requirements.txt` to let cloud service know the libraries needed to be installed

```linux=
pip freeze > requirements.txt
```

4. using the following commend to create the Django project named `Demo`

- command:

```linux=
django-admin startproject Demo
```

- 1. inside the file there will be:

```linux=
Demo/
    manage.py
    Demo/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

- 2. These files are:

  - The outer `Demo/` root directory is a container for your project. Its name ==doesn’t matter== to Django; you can rename it to anything you like.
  - `manage.py`: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about `manage.py` in [django-admin and manage.py](https://docs.djangoproject.com/en/4.1/ref/django-admin/).
  - The inner `Demo/` directory is the ==actual Python package== for your project. Its name is the Python package name you’ll need to use to import anything inside it (\*\*e.g. `Demo.urls`).
  - `Demo/__init__.py`: An empty file that tells Python that this directory should be considered a Python package. If you’re a Python beginner, read [more about packages](https://docs.python.org/3/tutorial/modules.html#tut-packages) in the official Python docs.
  - `Demo/settings.py`: Settings/configuration for this Django project. [Django settings](https://docs.djangoproject.com/en/4.1/topics/settings/) will tell you all about how settings work.
  - `Demo/urls.py`: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in [URL dispatcher](https://docs.djangoproject.com/en/4.1/topics/http/urls/).
  - `Demo/asgi.py`: An ==entry-point== for ASGI-compatible web servers to serve your project. See [How to deploy with ASGI for more details.](https://docs.djangoproject.com/en/4.1/howto/deployment/asgi/)
  - `Demo/wsgi.py`: An ==entry-point== for WSGI-compatible web servers to serve your project. See [How to deploy with WSGI for more details.](https://docs.djangoproject.com/en/4.1/howto/deployment/wsgi/)

4. change directory to `Demo/` directory and use the following command to run Django server

```linux
python3 manage.py runserver
```

5. we’ll create our `home` app in the same directory as your `manage.py` file so that it can be imported as its own top-level module, rather than a submodule of Demo. To create your app, make sure you’re in the same directory as manage.py and type this command:

```linux
python manage.py startapp home
```

the directory structure will be like following:

```linux
home/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

6. To let Django know we create a `home` app, we need to add `home` under `setting.py` files in `Demo/` directory

in `Demo/setting.py`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'home',
]
```

7. modify `urls.py` under `Demo/` directory to make Django be able to route to `/home` when user type `/home` in URLs

inside `Demo/urls.py`:

```python=
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('home/', include('home.urls')),
]
```

8. create `urls.py` in `home/`： to tell Django to access views.home when user connect to `WEBSITE_URL/home`

inside `home/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home),
]
```

9. modify `home/views.py`：
   first import render to directly return render(request, ‘home/home.html’); so that we can use `home.html`under `templates/` directory or we have to do:
   ```python
   template = loader.get_template(‘home/home.html’)，
   return  HttpResponse(template.render(request))
   ```

inside home/views.py:

```python
from django.shortcuts import render,HttpResponse

def home(request):
    return render(request, 'home/home.html')
```

10. create `templates/home/home.html` under `home` directory

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <div class="container">
      <h1>HOME</h1>
      <h2>Hello World!</h2>
    </div>
  </body>
</html>
```

11. change `DIRS` under `TEMPLATES` in `Demo/Demo/settings.py` to tell Django the route of template

```python
'DIRS': [os.path.join(BASE_DIR, 'templates').replace('\\', '/')],
```
