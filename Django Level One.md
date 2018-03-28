##### create the first django project
- `django-admin startproject first_project`
- then an folder named first_project is created in the directory
##### use manage.py:
- 'cd first_project'
- `python manage.py runserver`
- http://127.0.0.1:8000/ an webpage will be local hosted on your computer
- "You have 13 unapplied migration(s)": currently ignore this warning
##### Django application
- A Django Application is created to perform a particular functionality for your entire web application
- These Django Apps can then be plugged into other Django Projects, so you can reuse them!
- `python manage.py startapp first_app` to create the first app
- a new folder "first_app" will be created. prompt will directly switch to it
##### process of creating a view and mapping it to a URL
- in setting.py, add app name to the end of the list

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'first_app'
]
```
- in "first_project\first_app\views.py"

```
from django.shortcuts import render
from django.http import HttpResponse #import HttpResponse object

# Create your views here.

def index(request):
    # an view, each view must return in HttpResponse object
    return HttpResponse("hello world")
```
- The contents in `HttpResponse()`can also be HTML code
- To see this view, need to map this view to the urls.py:

```
from django.conf.urls import url
from django.contrib import admin
from first_app import views #import views.py
urlpatterns = [
    url(r'^$',views.index,name="index"),#what we add to the list
    url(r'^admin/', admin.site.urls),
]
```

##### URL mapping
