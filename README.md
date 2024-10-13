# learnDjango-
Learning django
**Documentation**👉🏻 https://docs.djangoproject.com/en/5.1/

First App Tutorial    https://docs.djangoproject.com/en/5.1/intro/tutorial01/

# Web framework - Collection of modules, library and packages to speed up development.

- **Module:** A single Python file containing code (like a specific book).
- **Package:** A collection of related modules organized together (like a bookshelf).
- **Library:** A collection of packages and modules that provide tools for a wide variety of tasks (like an entire library).

Django makes api development easy!! - because of its Django rest api framework.  

```bash
pip install django
django-admin startproject mysite
```

 Now in directory, a new folder has been created “Project name”

The following file structure has been created in that following directory.

```bash
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

These files are:

- The outer **`mysite/`** root directory is a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.
- **`manage.py`**: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about **`manage.py`** in [django-admin and manage.py](https://docs.djangoproject.com/en/5.1/ref/django-admin/).
- The inner **`mysite/`** directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. **`mysite.urls`**).
- **`mysite/__init__.py`**: An empty file that tells Python that this directory should be considered a Python package. If you’re a Python beginner, read [more about packages](https://docs.python.org/3/tutorial/modules.html#tut-packages) in the official Python docs.
- **`mysite/settings.py`**: Settings/configuration for this Django project. [Django settings](https://docs.djangoproject.com/en/5.1/topics/settings/) will tell you all about how settings work.
- **`mysite/urls.py`**: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in [URL dispatcher](https://docs.djangoproject.com/en/5.1/topics/http/urls/).
- **`mysite/asgi.py`**: An entry-point for ASGI-compatible web servers to serve your project. See [How to deploy with ASGI](https://docs.djangoproject.com/en/5.1/howto/deployment/asgi/) for more details.
- **`mysite/wsgi.py`**: An entry-point for WSGI-compatible web servers to serve your project. See [How to deploy with WSGI](https://docs.djangoproject.com/en/5.1/howto/deployment/wsgi/) for more details.

Now move to the root directory and initiate/verify the django server.

```bash
python manage.py runserver
```

This will start your server at port http://127.0.0.1:8000/

**Note- Ignore the warning about unapplied database migrations for now; we’ll deal with the database shortly.**

## CREATING THE POLLS APP

### **Projects vs. apps**

What’s the difference between a project and an app? An app is a web application that does something – e.g., a blog system, a database of public records or a small poll app. A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

To create your app, make sure you’re in the same directory as **`manage.py`** and type this command:

```bash
python manage.py startapp polls
```

This will create a directory **polls,** which is laid out like this.

```bash
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

## Views in django

In Django, views are Python functions (or classes) that take a web request and return a web response. They are the core component of the Django framework that handles the logic for processing user requests and generating the appropriate output. 

## Writing our first view

Let’s open the polls/views.py  and put the following code in it.

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

This is the most basic view possible in Django. To access it in a browser, we need to map it to a URL - and for this we need to define a URL configuration, or “URLconf” for short. These URL configurations are defined inside each Django app, and they are Python files named **`urls.py`**.

To define a URLconf for the **`polls`** app, create a file **`polls/urls.py`** with the following content:

```python
from django.urls import path

from . import views#from current directory import the module views

urlpatterns = [
    path("", views.index, name="index"),
]
```

The next step is to configure the global URLconf in the **`mysite`** project to include the URLconf defined in **`polls.urls`**. To do this, add an import for **`django.urls.include`** in **`mysite/urls.py`** and insert an [**`include()`**](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.include) in the **`urlpatterns`** list, so you have:

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('polls/', include(polls.site.urls)),
    path("admin/" , admin.site.urls)
]

```

The [**`path()`**](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.path) function expects at least two arguments: **`route`** and **`view`**. The [**`include()`**](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.include) function allows referencing other URLconfs. Whenever Django encounters [**`include()`**](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.include), it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing.

**The idea behind [`include()`](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.include) is to make it easy to plug-and-play URLs. Since polls are in their own URLconf (`polls/urls.py`), they can be placed under “/polls/”, or under “/fun_polls/”, or under “/content/polls/”, or any other path root, and the app will still work.**

### **When to use [`include()`](https://docs.djangoproject.com/en/5.1/ref/urls/#django.urls.include)**

**You should always use `include()` when you include other URL patterns. `admin.site.urls` is the only exception to this.**

You have now wired an **`index`** view into the URLconf. Verify it’s working with the following command: 

```bash
python manage.py runserver
```

**No need to push if web-server is already active. Follow this link** http://localhost:8000/polls/ and you’ll see- “Hello, world. You're at the polls index. “

 

# **Tutorial 2**

We’ll set up the database, create your first model, and get a quick introduction to Django’s automatically-generated admin site.

## Database setup

Now open **mysite/settings.py.** It’s normal Python module with multi-level-variables representing Django settings.

By default, The database uses SQLite. It is beginner friendly.

[**`INSTALLED_APPS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-INSTALLED_APPS) setting at the top of the file. That holds the names of all Django applications that are activated in this Django instance. Apps can be used in multiple projects, and you can package and distribute them for use by others in their projects.

By default, [**`INSTALLED_APPS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-INSTALLED_APPS) contains the following apps, all of which come with Django:

- [**`django.contrib.admin`**](https://docs.djangoproject.com/en/5.1/ref/contrib/admin/#module-django.contrib.admin) – The admin site. You’ll use it shortly.
- [**`django.contrib.auth`**](https://docs.djangoproject.com/en/5.1/topics/auth/#module-django.contrib.auth) – An authentication system.
- [**`django.contrib.contenttypes`**](https://docs.djangoproject.com/en/5.1/ref/contrib/contenttypes/#module-django.contrib.contenttypes) – A framework for content types.
- [**`django.contrib.sessions`**](https://docs.djangoproject.com/en/5.1/topics/http/sessions/#module-django.contrib.sessions) – A session framework.
- [**`django.contrib.messages`**](https://docs.djangoproject.com/en/5.1/ref/contrib/messages/#module-django.contrib.messages) – A messaging framework.
- [**`django.contrib.staticfiles`**](https://docs.djangoproject.com/en/5.1/ref/contrib/staticfiles/#module-django.contrib.staticfiles) – A framework for managing static files.

Above are the default applications provided by django for common case.

Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:

```bash
python manage.py migrate
```

The [**`migrate`**](https://docs.djangoproject.com/en/5.1/ref/django-admin/#django-admin-migrate) command looks at the [**`INSTALLED_APPS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-INSTALLED_APPS) setting and creates any necessary database tables according to the database settings in your **`mysite/settings.py`** file and the database migrations shipped with the app (we’ll cover those later). You’ll see a message for each migration it applies. If you’re interested, run the command-line client for your database and type **`\dt`** (PostgreSQL), **`SHOW TABLES;`** (MariaDB, MySQL), **`.tables`** (SQLite), or **`SELECT TABLE_NAME FROM USER_TABLES;`** (Oracle) to display the tables Django created.

## Creating Models

Now we’ll define your models – essentially, your database layout, with additional metadata.

In Django, a model is like a blueprint for a database table. Think of it as a way to describe the structure of your data.

**Here's a simple analogy:**

Imagine you're designing a form to collect information about your friends. You'd need fields for their name, email, phone number, and maybe their birthday.

In our poll app, we’ll create two models: **`Question`** and **`Choice`**. A **`Question`** has a question and a publication date. A **`Choice`** has two fields: the text of the choice and a vote tally. Each **`Choice`** is associated with a **`Question`**.

polls/models.py

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

Here, each model is represented by a class that subclasses [**`django.db.models.Model`**](https://docs.djangoproject.com/en/5.1/ref/models/instances/#django.db.models.Model). Each model has a number of class variables, each of which represents a database field in the model.

Each field is represented by an instance of a [**`Field`**](https://docs.djangoproject.com/en/5.1/ref/models/fields/#django.db.models.Field) class – e.g., [**`CharField`**](https://docs.djangoproject.com/en/5.1/ref/models/fields/#django.db.models.CharField) for character fields and [**`DateTimeField`**](https://docs.djangoproject.com/en/5.1/ref/models/fields/#django.db.models.DateTimeField) for datetimes. This tells Django what type of data each field hold

The name of each [**`Field`**](https://docs.djangoproject.com/en/5.1/ref/models/fields/#django.db.models.Field) instance (e.g. **`question_text`** or **`pub_date`**) is the field’s name, in machine-friendly format. You’ll use this value in your Python code, and your database will use it as the column name.

## Activating models

That small bit of model code gives Django a lot of information. With it, Django is able to:

- Create a database schema (**`CREATE TABLE`** statements) for this app.
- Create a Python database-access API for accessing **`Question`** and **`Choice`** objects.

 Now our polls app is installed.

To include the app in our project, we need to add a reference to its configuration class in the [**`INSTALLED_APPS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-INSTALLED_APPS) setting. The **`PollsConfig`** class is in the **`polls/apps.py`** file, so its dotted path is **`'polls.apps.PollsConfig'`**. Edit the **`mysite/settings.py`** file and add that dotted path to the [**`INSTALLED_APPS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-INSTALLED_APPS) setting. It’ll look like this:

```python
INSTALLED_APPS = [
    "polls.apps.PollsConfig",
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

Now Django knows to include the **` polls** app. Let’s run another command:

```bash
 python manage.py makemigrations polls
```

**By running `makemigrations`, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a *migration*.**

There is a command that will only run the migrations for you and manage your database schema automatically - that is called migrate, and we will come to it in a moment. The sqlmigrate command takes migration  names and returns their SQL.
```bash
python manage.py sqlmigrate polls 0001
```
You should see similar to something like this.

```bash
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" (
    "id" bigint NOT NULL PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    "question_text" varchar(200) NOT NULL,
    "pub_date" timestamp with time zone NOT NULL
);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
    "id" bigint NOT NULL PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL,
    "question_id" bigint NOT NULL
);
ALTER TABLE "polls_choice"
  ADD CONSTRAINT "polls_choice_question_id_c5b4b260_fk_polls_question_id"
    FOREIGN KEY ("question_id")
    REFERENCES "polls_question" ("id")
    DEFERRABLE INITIALLY DEFERRED;
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");

COMMIT;
```

Now, run Migrate again to create those model tables in your database.
```bash
python manage.py migrate
```
Migrations are very powerful and let you change your models over time, as you develop your project, without the need to delete your database or tables and make new ones - it specializes in upgrading your database live, without losing data. We’ll cover them in more depth in a later part of the tutorial, but for now, remember the three-step guide to making model changes:

Change your models (in models.py).
Run python manage.py makemigrations to create migrations for those changes
Run python manage.py migrate to apply those changes to the database.

**Playing With API**
Now, let’s hop into the interactive Python shell and play around with the free API Django gives you. To invoke the Python shell, use this command:
```bash
python manage.py shell
 ```
```python
>>> from polls.models import Choice, Question  # Import the model classes we just wrote.

# No questions are in the system yet.
>>> Question.objects.all()
<QuerySet []>

# Create a new Question.
# Support for time zones is enabled in the default settings file, so
# Django expects a datetime with tzinfo for pub_date. Use timezone.now()
# instead of datetime.datetime.now() and it will do the right thing.
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# Save the object into the database. You have to call save() explicitly.
>>> q.save()

# Now it has an ID.
>>> q.id
1

# Access model field values via Python attributes.
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2024, 10, 12, 9, 28, 49, 545427, tzinfo=datetime.timezone.utc)

# Change values by changing the attributes, then calling save().
>>> q.question_text = "What's up?"
>>> q.save()

# objects.all() displays all the questions in the database.
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>

```
 Wait a minute. <Question: Question object (1)> isn’t a helpful representation of this object. Let’s fix that by editing the Question model (in the polls/models.py file) and adding a __str__() method to both Question and Choice:
 ```python
from django.db import models


class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text


class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```
It’s important to add __str__() methods to your models, not only for your own convenience when dealing with the interactive prompt, but also because objects’ representations are used throughout Django’s automatically-generated admin.

Let's also add a custom method to this model
```python
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```
This code is a part of a Django model class for handling a `Question` in a database. Here's a breakdown of what's happening:

### Key Components:

1. **Imports**:
   - `datetime`: This module is used to handle dates and times in Python.
   - `models` from `django.db`: This is the base class for defining models in Django.
   - `timezone` from `django.utils`: This utility is used for timezone-aware date and time handling in Django.

2. **`Question` Model**:
   - This is a Django model representing a "Question" in your database. It will likely have fields such as `pub_date` to store the date it was published, though this field isn't shown in the code snippet.
   - Django models typically map to database tables, and each field in the model corresponds to a column in the table.

3. **Method `was_published_recently`**:
   - This method checks if the `Question` was published in the last 24 hours.
   - **`timezone.now()`** gets the current date and time (aware of timezones).
   - **`datetime.timedelta(days=1)`** represents a time difference of 1 day (24 hours).
   - The method compares the question's publication date (`self.pub_date`) to see if it is greater than or equal to the time 24 hours ago. If the publication date is within the last 24 hours, this method will return `True`.

### Example Scenario:
- If the `Question` object was published at 3 PM yesterday and you check `was_published_recently()` at 2 PM today, the method will return `True`, because it's within 24 hours.
- If it was published two days ago, it will return `False`.

This method is useful for displaying questions published recently on your website, or for filtering queries based on publication time.

Would you like to add more functionality to this method, or perhaps write tests for it?


