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
- The first way is the one shown above
- Can also using the include() function from django.conf.urls

1. in "first_project\first_project\urls.py" import include()and add url pattern for the app page

```
from django.conf.urls import url
from django.contrib import admin
from django.conf.urls import include#import the include()
from first_app import views #import views.py
urlpatterns = [
    url(r'^$',views.index,name="index"),# homepage link, content views.link
    url(r'^first_app/',include('first_app.urls')),
    # allow us to look for url with the pattern www.domainname.com/first_app/…
    url(r'^admin/', admin.site.urls),
]
```
`url(r'^first_app/',include('first_app.urls'))` the include() function basically tells Django to go look at the urls.py file inside of first_app folder

2. Create the urls.py in "first_project/first_app"

```
from django.conf.urls import url
from first_app import views
# urls.py file set up for the individual app
urlpatterns = [
    url(r'^$',views.index,name="index"),# generic regular expression
]
```

##### Templates
we can actually render hthe html template rather tahn just simple strings

1. In "first_project\first_project\settings.py", setup the template folder directory:

```
import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
# BASE_DIR is the path of first_project

TEMPLATES_DIR = os.path.join(BASE_DIR,"templates")
#build template directory by join to dir together
```
  - by using os module, the directory will be Generated based on user's operating system
  - "BASE_DIR" is the dir of project folder
  - build template folder directory by join dir together

2. In "first_project\first_project\settings.py", add the template folder directory to "TEMPLATES" list:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [TEMPLATES_DIR,],#label the template folder path
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

3. create index.html as template in templates folder
4. In "first_project\first_app\views.py", render the index.html by "render()"

```
from django.shortcuts import render
from django.http import HttpResponse #import HttpResponse object

# Create your views here.

def index(request):
    # an view, each view must return in HttpResponse object
    my_dict = {"insert_me": "Hello I am James"}
    # content to be inserted into template
    # {"insertNameSameAsIndexHTML":"InsertContent"}
    return render(request,"first_app/index.html",context=my_dict)
    # render(request,templateDir,context)
```
##### Overall order: URL mapping + template to create an fresh page

##### static files
- Need also to render static medias(anything that is not going to change) such as img, css, JS
- `{% load staticfiles %}` at the begining of index.html file then `<img src={%static "imgs/pic.jpg"%}>`
- {%%} instead of {{}}

1. Create "static" folder(parallel to "templates" folder). In "first_project\first_project\settings.py", set up the path for static folder

```
import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
# BASE_DIR is the path of first_project
TEMPLATES_DIR = os.path.join(BASE_DIR,"templates")
#build template directory by join to dir together
STATIC_DIR = os.path.join(BASE_DIR,"static")#build static directory
```
2. In "first_project\first_project\settings.py", at the bottom:

```
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.10/howto/static-files/

STATIC_URL = '/static/'
# "domainname/static/images" to access the static files
STATICFILES_DIRS = [
    STATIC_DIR,
]
# set up the staticfiles directory(list)
```

3. In the template(index.html):
  - Firstly introduce the staticfiles at the top:

  ```
  <!doctype html>
  {% load staticfiles %}
  ```
  - Then insert img: the img path should be the path in static folder

  ```
  <img src="{% static "images\crown fish.jpg" %}" alt="crown fish">
  ```
  - Insert css in the same way

  ```
  <link rel="stylesheet" href="{% static 'css/style.css' %}" >
  ```
