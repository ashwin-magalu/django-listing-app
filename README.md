# Procedure:

## Creating a Virtual Environment

python -m venv ll_env --> here ll_env is a name of a virtual environment

## Activating the Virtual Environment

1. bash: source ll_env/Scripts/activate
2. cmd: ll_env\Scripts\activate

## Deactivating the Virtual Environment

deactivate

## Installing Django

1. Activate virtual environment
2. pip install django

## Creating a Project in Django

django-admin startproject project_name .

#### The dot at the end of the command creates the new project with a directory structure that will make it easy to deploy the app to a server when we’re finished developing it. Don’t forget this dot, or you might run into some configuration issues when you deploy the app. If you forget the dot, delete the files and folders that were created (except environment), and run the command again

#### The project directory contains four files; the most important are settings.py, urls.py, and wsgi.py. The settings.py file controls how Django interacts with your system and manages your project. We’ll modify a few of these settings and add some settings of our own as the project evolves. The urls.py file tells Django which pages to build in response to browser requests. The wsgi.py file helps Django serve the files it creates. The filename is an acronym for web server gateway interface.

## Creating the Database

#### Django stores most of the information for a project in a database, so next we need to create a database that Django can work with

python manage.py migrate

## Viewing the Project

python manage.py runserver

### Open a web browser and enter the URL http://localhost:8000/, or http://127.0.0.1:8000/

#### If you receive the error message That port is already in use, tell Django to use a different port by entering python manage.py runserver 8001, and then cycle through higher numbers until you find an open port.

## Starting an App

#### A Django project is organized as a group of individual apps that work together to make the project work as a whole.

## You should leave the development server running in the terminal window you opened earlier. Open a new terminal window (or tab), and navigate to the directory that contains manage.py. Activate the virtual environment, and then run the startapp command

python manage.py startapp app_name

## Defining Models

#### Open the file models.py, and look at its existing content:

#### To see the different kinds of fields you can use in a model, see the Django Model Field Reference at https://docs.djangoproject.com/en/2.2/ref/models/fields/. You won’t need all the information right now, but it will be extremely useful when you’re developing your own apps

## Activating Models

#### To use our models, we have to tell Django to include our app in the overall project. Open settings.py, you’ll see a section that tells Django which apps are installed and work together in the project. Add our app to this list by modifying INSTALLED_APPS.

#### It’s important to place your own apps before the default apps in case you need to override any behavior of the default apps with your own custom behavior.

#### Next, we need to tell Django to modify the database so it can store information related to the model Topic

python manage.py makemigrations app_name

#### The command makemigrations tells Django to figure out how to modify the database so it can store the data associated with any new models we’ve defined

#### Now we’ll apply this migration and have Django modify the database for us

python manage.py migrate

#### Whenever we want to modify the data that our project manages, we’ll follow these three steps: modify models.py, call makemigrations on app_name, and tell Django to migrate the project.

## The Django Admin Site

### Setting Up a Superuser

#### Django allows you to create a superuser, a user who has all privileges available on the site. A user’s privileges control the actions that user can take

python manage.py createsuperuser
Username (leave blank to use 'eric'): user_name
Email address: email@email.com
Password: password
Password (again): password

#### Some sensitive information can be hidden from a site’s administrators. For example, Django doesn’t store the password you enter; instead, it stores a string derived from the password, called a hash. Each time you enter your password, Django hashes your entry and compares it to the stored hash. If the two hashes match, you’re authenticated. By requiring hashes to match, if an attacker gains access to a site’s database, they’ll be able to read its stored hashes but not the passwords. When a site is set up properly, it’s almost impossible to get the original passwords from the hashes.

## Registering a Model with the Admin Site

#### Open the admin.py file. To register Topic with the admin site, enter the following:

from .models import Topic
admin.site.register(Topic)

#### Now use the superuser account to access the admin site. Go to http://localhost:8000/admin/, and enter the username and password for the superuser you just created

## Adding Topics

#### We can add topics in django admin page

## Defining the Entry Model

#### For a user to record what they’ve been learning about chess and rock climbing, we need to define a model for the kinds of entries users can make in their learning logs. Each entry needs to be associated with a particular topic. This relationship is called a many-to-one relationship, meaning many entries can be associated with one topic. Open models.py and create a new class called as Entry

## Migrating the Entry Model

python manage.py makemigrations app_name
python manage.py migrate

## Registering Entry with the Admin Site

#### As discussed earlier, same procedure

from .models import Entry
admin.site.register(Entry)

# Making Pages: The Learning Log Home page

#### Making web pages with Django consists of three stages: defining URLs, writing views, and writing templates. You can do these in any order, but in this project we’ll always start by defining the URL pattern. A URL pattern describes the way the URL is laid out. It also tells Django what to look for when matching a browser request with a site URL so it knows which page to return. Each URL then maps to a particular view—the view function retrieves and processes the data needed for that page. The view function often renders the page using a template, which contains the overall structure of the page.

## Mapping a URL

#### In the main learning_log project folder, open the file urls.py. Add path('', include('learning_logs.urls')), inside urlpatterns Array, import include from django.urls

#### We’ve added a line to include the module learning_logs.urls at The default urls.py is in the learning_log folder; now we need to make a second urls.py file in the learning_logs folder

## Writing a View

#### A view function takes in information from a request, prepares the data needed to generate a page, and then sends the data back to the browser, often by using a template that defines what the page will look like.

#### Edit views.py file

#### When a URL request matches the pattern we just defined, Django looks for a function called index() in the views.py file. Django then passes the request object to this view function. In this case, we don’t need to process any data for the page, so the only code in the function is a call to render(). The render() function here passes two arguments—the original request object and a template it can use to build the page

## Writing a Template

#### The template defines what the page should look like, and Django fills in the relevant data each time the page is requested. A template allows you to access any data provided by the view

#### Inside the learning_logs folder, make a new folder called templates. Inside the templates folder, make another folder called learning_logs. This might seem a little redundant (we have a folder named learning_logs inside a folder named templates inside a folder named learning_logs), but it sets up a structure that Django can interpret unambiguously, even in the context of a large project containing many individual apps. Inside the inner learning_logs folder, make a new file called index.html.

# Building Additional Pages

## Template Inheritance

### The Parent Template

#### We’ll create a template called base.html in the same directory as index.html. This file will contain elements common to all pages; every other template will inherit from base.html.

#### The first part of this file creates a paragraph containing the name of the project, which also acts as a home page link. To generate a link, we use a template tag, which is indicated by braces and percent signs {% %}. A template tag generates information to be displayed on a page. Our template tag {% url 'learning_logs:index' %} generates a URL matching the URL pattern defined in learning_logs/urls.py with the name 'index'. In this example, learning_logs is the namespace and index is a uniquely named URL pattern in that namespace. The namespace comes from the value we assigned to app_name in the learning_logs/urls.py file.

#### We inserted a pair of block tags. This block, named content, is a placeholder; the child template will define the kind of information that goes in the content block. A child template doesn’t have to define every block from its parent, so you can reserve space in parent templates for as many blocks as you like; the child template uses only as many as it requires.

### The Child Template

#### Now we need to rewrite index.html to inherit from base.html

### The Topics Page

##### Add topics url to urls.py, add topics function in views.py. Make a file called topics.html in the same directory as index.html

# Styling

## The django-bootstrap4 App

pip install django-bootstrap4

#### Go to settings.py and add: 'bootstrap4', in INSTALLED_APPS

#### Add bootstrap in respective html files

# Deploying Learning Log

## Making a Heroku Account

#### o make an account, go to https://heroku.com/ and click one of the signup links. It’s free to make an account, and Heroku has a free tier that allows you to test your projects in live deployment before properly deploying them.

## Installing the Heroku CLI

pip install psycopg2==2.7.\*
pip install django-heroku
pip install gunicorn

## Creating a requirements.txt File

pip freeze > requirements.txt

## Specifying the Python Runtime

python --version

## Modifying settings.py for Heroku

#### go to settings.py and add below lines at the end

# Heroku settings.

import django_heroku
django_heroku.settings(locals())

## Making a Procfile to Start Processes

#### A Procfile tells Heroku which processes to start to properly serve the project. Save this one-line file as Procfile, with an uppercase P and no file extension, in the same directory as manage.py.

web: gunicorn learning_log.wsgi --log-file -

## Using Git to Track the Project’s Files

### Installing Git

git --version

### Configuring Git

git config --global user.name "ehmatthes"
git config --global user.email "eric@example.com"

### Ignoring Files

#### add below lines in .gitignore

ll_env/
**pycache**/
\*.sqlite3

### Making Hidden Files Visible

#### Most operating systems hide files and folders that begin with a dot, such as .gitignore. When you open a file browser or try to open a file from an application such as Sublime Text, you won’t see these kinds of files by default. But as a programmer, you’ll need to see them

### Committing the Project

git init
git add .
git commit -am "Ready for deployment to heroku."
git status

### Pushing to Heroku

heroku login
heroku create
git push heroku master
heroku ps
heroku open

### Setting Up the Database on Heroku

heroku run python manage.py migrate

### Refining the Heroku Deployment

#### Creating a Superuser on Heroku

heroku run bash
python manage.py createsuperuser
-->fill details
exit

### Creating a User-Friendly URL on Heroku

heroku apps:rename learning-log

## Securing the Live Project

#### Let’s modify settings.py so it looks for an environment variable when the project is running on Heroku

if os.environ.get('DEBUG') == 'TRUE':
DEBUG = True
elif os.environ.get('DEBUG') == 'FALSE':
DEBUG = False

## Committing and Pushing Changes

git commit -am "Set DEBUG based on environment variables."
git status

## Setting Environment Variables on Heroku

heroku config:set DEBUG='FALSE'
