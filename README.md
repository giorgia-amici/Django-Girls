#**Django**

##**What is Django?**

[Django](https://www.djangoproject.com/) is a __free and open source web application framework__, written in **Python**. It's a web framework - a set of components that helps you to develop websites faster and easier.

====================================================================

##**Installation (OS X)**

Python is native in your machine, but you will need to check which version are you actually using. For this tutorial you will need a from version 3.4 onward.

To check your version:

```python

python3 --version

```

The reason why you will need from version 3.4 on, is because you will be using the ```pip``` command (the python version of npm).


If you want to download the latest version, visit the [official Python website](https://www.python.org/downloads/release/python-342/)

##**Virtual Environment & Django installation**

A **Virtual Environment** is a tool to keep the dependencies required by different projects in separate places, by creating virtual Python environments for them (check your folder structure after typing the following command). It's possible to skip this step, but it's highly recommended not to - starting with the best possible setup will save you a lot of trouble in the future!(TRUE, TRUE, TRUE, TRUE!!)

Move into your project directory and run this command:

```python

python3 -m venv myvenv

```

where ```myenv``` is a customizable parameter that refers to the name of your virtual env. (HINT: keep it short because you will be referencing it a lot.)


Then, to **activate** your __virtualenv__, just type:

```python

source myvenv/bin/activate

```

As a result, you will see your prompt looking something like:

```python

(myvenv) ~/djangogirls$

```

If you want to commit those stages to GitHub, remember to disactivate your virtualenv by typing:

```python

deactivate

```

Now, your are finally ready to install **Django**. Just type (in your virtualenv):

```python

pip install django==1.7.1

```

====================================================

##**Starting the actual project**

In your virtualenv, type:

```python

django-admin startproject mysite .

```

This command will create a huge file structure and a ```mysite``` folder with the following structure:

```shell

mysite/
├── __init__.py
├── settings.py
├── urls.py
└── wsgi.py

```

```manage.py``` is a script that helps with management of the site. With it we will be able to start a web server on our computer without installing anything else, amongst other things.

```settings.py``` contains the config of the website.

====================================

##**Creating an app**

From the location in which your manage.py seats, type:

```python

python manage.py startapp blog

```

This command will create a blog app folder with this structure:

```shell

└── blog
    ├── migrations
    |       __init__.py
    ├── __init__.py
    ├── admin.py
    ├── models.py
    ├── tests.py
    └── views.py

```

Now, we need to tell Django that we want to use our ```blog app```. In ```mysite/settings.py``` under ```INSTALLED_APPS``` add ```'blog'```:

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
)

```

==========================================================

###**Creating a BlogPostModel**

Open ```blog/models.py``` and type:


```python
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title

```

This will allow us to create new Posts. Now we need to tell Django about those changes and that we want to use them for our db.
Just type:

```python
python manage.py makemigrations blog

```
This will create some migrations files (as in Rails with the schema) that now we need to apply to the db:

```python
python manage.py migrate blog

```

=================================================

##**Django Shell and QuerySet**

To access the Django Shell, just type:

```python
python manage.py shell

```

Now you can use all the properties of QuerySet. A QuerySet is, in essence, a list of objects of a given Model and it allows you to read the data from database, filter it and order it.

You first need to **import** Post and User. For the first, type:

```python
from blog.models import Post
```

while for the second one, insert:

```python
from django.contrib.auth.models import User
```

Now, if you type ```Post.objects.all()``` or ```User.objects.all()``` you will see: ```[]``` because they both exist but they are empty. So, let's populate them. First, we will create a new User by typing:

```python
User.objects.create(username='giorgia')

```

then, we can create an instance of user:

```python
me = User.objects.get(username='giorgia')
```

Now, is time to create our first Post:

```python

Post.objects.create(author = me, title = 'Sample title', text = 'Test')
```

and now, if you type ```Post.objects.all()```, you will see your first Post: ```[<Post: Sample title>]```

===================================================

