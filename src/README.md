
I am currently going through https://www.youtube.com/watch?v=F5mRW0jo-U4 

I am at 1:49:08 and this is my code and my notes.

# Key steps


## Set up
* virtualenv
* install Django
* create src directory
```shell
django-admin startproject [project_name] .
```

```shell
python manage.py startserver
```




# App
```
django-admin starapp [app_name]
```
This command creates a folder with all the required files (models, views...)

It has to be registered in the ```INSTALLED_APPS``` variable of the file settings.py 

# Models
Models are classes inherited from models.Model where all the attribute of the class will be a field stored in 
the database

Whenever a model is modified:
* python manage.py makemigrations
* python manage.py migrate

# Admin
The admin interface allows simple CRUD of models

```shell
python manage.py createsuperuser
```

To register the Model "Product" go in the ```admin.py``` interface add
```python 
from django.contrib import admin
from .models import Product

admin.site.register(Product)
```

# Views
Views are endpoints of the browser that will generate content for the user.

it is required to:
* create a function in the views.py of the app ("pages" in the given example)
```python
def about_view(request, *args, **kwargs):
    my_context = {'my_text':'useless text present in the context',
                  'my_list': [123,312,5435]}
    return render(request, "about.html", my_context)
```

* register the endpoit in the file ```urls.py```
```python 
from pages.views import about_view

urlpatterns = [
    path('about/', about_view, name='contact'),
    ]
```
it is to be noted that the import is not ```from ..pages.views``` but ```from pages.views```

This is surprising at first sight as the ```urls.py``` in in the ```[project_name]``` directory which is a 
brother directory to the app directory ```[pages]```.

In fact as we run the server with  ```python manage.py startserver``` which is located above ```[project_name]``` and 
```[page]``` explaining why we do not have to use the ```..``` 

# Templates
* Create a ```tempaltes``` directory 
* Register it in the ```settings.py```

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
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

Templates are simple html files with interesting capacities:
## Block content
the block of the file base.html ```{% block block_name%} {% endblock %}``` will 
be replaced if a file uses ```{% extends 'base.html' %}``` and defines the content such a block

```
{% extends 'base.html' %}
{% block block_name%} 
I am the file about.html and this sentence will be displayed whenever this template is asked to be rendered.
{% endblock %}
```  
 
There is also the possibility to ```{% include filename %}``` to render a part that will be used 
often (for example a navbar will be included in the base.html if we want it in a different file).
 
## Rendering the context
The templates are used with the render function which takes an argument giving a dictionnary call "context"
any key of the contet can be rendered in the templace with ```{{key}}```

* there exist filters to modify the content of the key 
* there exist a if / for in the rendering of the template depending on the keyed-value.