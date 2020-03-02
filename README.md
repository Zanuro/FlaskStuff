# Flask Learning

This is some flask compilation code and examples from multiple sources such as Flask Mega Tutorial by Miguel Grinberg and others.


### Chapter 1-Basic installation

First step,check to see python(especially python3).
Check if python is installed: `$python3`

Then you have to install Flask.Flask is a package available in a public repository therefore we can install it like a normal package using the following command: `$pip install <package-name>`

There is a problem though.Altought you can install a package globally and it could be used by all the python scripts that need it but considering that maybe you've created an app using a specific version X of a package and there is a new version Y of that package,you'd be interested to use this new version on a new app but not on all the apps,that's why you need to mantatin different versions of the same package for different applications and for that will be talking about virtual environments.
This is the only solution to have multiple versions of the same packages used by different apps, without implying any conflict between them.
Virtual environments have the added benefit that they are owned by the user who creates them, so they do not require an administrator account.
Will start creating a directory for our app: `$mkdir blog`.
To create a virtual environment we need to do: `$python3 -m venv virtual-env`.
We run venv package with python3 creating a virtual env called "virtual-env".
After the command completes, you are going to have a directory named virtual-env where the virtual environment files are stored.
Now you have to tell the system that you want to use it, and you do that by activating it. To activate your brand new virtual environment you use the following command: `$ source virtual-env/bin/activate`
When you activate a virtual environment, the configuration of your terminal session is modified so that the Python interpreter stored inside it is the one that is invoked when you type python. Also, the terminal prompt is modified to include the name of the activated virtual environment. The changes made to your terminal session are all temporary and private to that session, so they will not persist when you close the terminal window.
If you have multiple terminals with multiple apps you can have a different virtual environment open for each one of them.
Now we can install Flask on our virtual env: `$pip install flask`
You have to install it while being into the virtual env.

You can check that you installed flask correctly starting the python interpter and import flask: `$python3 $import flask`

If it ocurrs an error saying that pip module is not installed you can try forcing an reinstallation of pip like this: `$curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py` and then `$python3 get-pip.py --force-reinstall`.


The application will exist in a package. In Python, a sub-directory that includes a "__init__.py" file is considered a package, and can be imported. When you import a package, the "__init__.py" executes and defines what symbols the package exposes to the outside world.

Will create a package called application inside our blog directory: `$ mkdir application`.

In the "__init__.py" file we import Flask and create an instance of Flask passing a config variable __name__.Then we import the routes module.In the second import we import from the given instance created above the routes module that is why it needs to be created before importing.

In the routes module we have to set the different URLs the application will take and for that we wil be using view functions, that are normal python function that are mapped to one or multiple URLs so that Flask knwos what function to execute given from the user a certain path.

We will define certain routes in our routes.py file.For that we will be using decorators.
A decorator modifies the function that follows it. A common pattern with decorators is to use them to register functions as callbacks for certain events.

That being said,we have to @app.route() with two different routes and a given function above.When either of this two routes are invoked by the user(accesing this path) the index() function will be called and the return value of the function will be returned to the user.
We all need a script to import the application at the top-level of the application.

We will import app from the application package.
Then we need to tell Flask how to import the app: `$export FLASK_APP=blog.py`

And then we run the app using: `$flask run`.

The app will be available on our localhost listening as a development server on the port 5000.

Accesing the / and /index will have the same result outputting the message from the decorator function.
You can stop the server with Ctrl+C.

Since environment variables aren't remembered across terminal sessions, you may find tedious to always have to set the FLASK_APP environment variable when you open a new terminal window. Starting with version 1.0, Flask allows you to register environment variables that you want to be automatically imported when you run the flask command. To use this option you have to install the python-dotenv package:`$pip install python-dotenv`.

Then you can just write the environment variable name and value in a .flaskenv file in the top-level directory of the project:`FLASK_APP=blog.py`

## Chapter2-Templates

We will be creating a mock user for the welcome page.We will be creating it as a dictonary and then in the view function decorator index that we defined we return a html page.

Althought this solution is easy and simple inserting HTML directly into the browser is not the correct one.If we want to change the layour of the page will need to update the HTML code everytime from here.
If you want a scalable application this is not a solution.

We will be storing the different view templates in /templates.
`$mkdir app/templates`.

We will be creating a template for the home page.The dynamic content of this page will be represented by {{...}}.

Will modify the decorator to pass certain values to the index.html template and return that viewa and for that we will be using the render_template function from Flask.
The render_template() invokes the template engine that Flask is based on(Jinja 2).

We can also add conditional statment in the template to modify the flow of the rendering of a page.
For example we can check if the title variable is passed: `{% if title %}`.

When the user will log in he will want to see the recently posted posts from all the other users of the website.
Will create differente posts using a list of authors and subjects, each author will be represented by a username.

The template will render as many posts as the view sends.To implement that will use a for conditional in the template to represent the different posts.

Sometimes we will need certain parts such as a navigation bar in multiple pages and we don't really want to copy each the nav bar into each one of those pages.

Jinja2 has a template inheritance feature that specifically addresses this problem. In essence, what you can do is move the parts of the page layout that are common to all templates to a base template, from which all other templates are derived.

We will implement a simple navigation bar in a base template.
To insert a derived template into that base template will use a block control statment `{% block content %}`.
And then we make index.html to derive from base.html.
In the new index.html will specify that this extends from base.html with the following instruction: `{% extends "base.html" %}`.

And then will specify the unique content of that derived html page inside the block statment.
Then we can use the base.html page to create other pages from it.


## Chapter3-WebForms

To handle the web forms in this application we will use the Flask-WTF extension, a wrapper for the WTForms package: `$ pip3 install flask-wtf`.

We will need to specify some configuration variables to the application in a config file, the most basic one being:
` app = Flask(__name__)
  app.config("SECRET_KEY") = "abracadabra"
`

But the correct way to do that is to store the configuration file and store the configuration variables there.

We will define a class to store the different variables.
The SECRET_KEY variable is used in web application for things like signatures and tokes and to prevent attacks such as XSS or CSRF.
In our file we defined the envirnoment variable(if it exists) or a harcoded variable.On the development server there is not an issue if the string is hardcoded but on the production server we will need a secure key.
We  will then tell Flask to read the configuration file and apply it.


We will continue defining a web form for the user login form which will ask the user for the username and password.Also there will be a remember me checkbox and the submit button.
Firstly we import the base class FlaskForm and all the other classes representing our different fields from wtformss package.For each field we have a name and some optional validators to check for something.For example the DataRequired() checks that the field cannot be submitted empty.

We then create the html template to be rendered on the web page.
This template is using the base template base.html that we previously defined.It also contains an form element that wraps the whole web form.We use the novalidate attribute in order to tell the web browser to not apply validation to the fields, and our Flask app will have to do this validation.
The form.hidden_tag() template argument generates a hidden field that includes a token that is used to protect the form against CSRF attacks. All you need to do to have the form protected is include this hidden field and have the SECRET_KEY variable defined in the Flask configuration. If you take care of these two things, Flask-WTF does the rest for you.

To view this newly implemented form we need to implement the view function in the application.
We will map /login url to this form.

We have to import the LoginForm object created previously and then we pass the form object to the template with the same name-form.

Once we have the rendered page we have to implement the logic for the page to process the data submitted by the user.
We'll modify the routes.py to accept get and post requests instead of only get requests.The form.validate_on_submit() does all the work.If the browser sends a get request it won't enter into this control statment and will just render the template.If it sends a post request it gathers all the data from the form and can be processed by the application.If the request is succesfull then it calls the function flash to show a message to the user and then it calls the second function-redirect() which redirects the page to another page.
To rendered the flashed messages we need to modify the render template.
Will use a with construct to assign the result of calling the function get_flashed_messages() to a messages variable and then we check if messages has any content and make a list with each message item.

We want to display some messages in case the user doesn't put anything in the field and still tries to submit it.We want to let the user know that he must complete the field before submiting.
We modify the template and add all the validators messages necessary when the user submits the form.

A problem that we have is putting the links directly in the templates and redirect() as we have them in our navigation bar.
If you want to re-structure the page you would need to modify all of the links.For that we will use url_for() that maps url to the view function.(the name of the view function)

This is easier to mantain just because the view function names don't change so often like urls do, and the url_for() can generate more complex urls.
We change the directly hardcoded url in our templates and redirects to url_for().

## Chapter 4-Database

Flask does not support any database natively which in a sense is good news because you can choose whatever database suits you the best.

We are going to use SQLAlchemy,an ORM used for many relational databases supporting MySQL,PostgreSQL,SQLite,etc.
We install it: `$ pip install flask-sqlalchemy`.

As the application grows or changes we need to address the problem of the migration and for that we'll use flask-migrate,a migration framework for SQLAlchemy.
To develop small application the most convenient choice is SQLite as there is no need to run a database server for it.

We need to modify the config file as the SQLAlchemy will take the location of the database from the variable SQLALCHEMY_DATABASE_URI.
The database is going to be represented in the application by the database instance. The database migration engine will also have an instance.
In the __init__ file we add an object db to represent the database and another object that represents the migration engine and import the models module to later define the structure of the db.
We will want to represent a database model creating a representation for the users.Each user in the database will be identified by an unique id(primary key).It also has a username,email and a password hash(we store the hash and not the actual password) as strings(VARCHAR).
We create the model User which inherits from db.Model,a base class for all models.The class defines several fields that we previously mentioned with some options such as uniqueness and indexing for the efficency of the database.
We also define a method to print data of a particular object of a class.

We create the migration repository for the blog app,a directory in which it stores its migration scripts.
To create it we need to do: `$ flask db init`.

We then proceed to create a database migration,we can do it automatically or manually.To generate the migration automatically,Alembic compares the database schema against the actual database schema used and then creates the migration script.
At first as we don't have any previous database it will add the entire User model to the script.
With this we create the migration script that defines to functions: upgrade() to apply the migration and downgrade() to remove it.
We apply this changes: `$ flask db upgrade`.
The command does not detect any existing database so will create a default SQLite one.If you have a db server you need to create the database before executing upgrade().

In our app the most efficent way to store the post each user makes is by creating another table posts to store and reference the posts that a user makes.
The posts table will have a unique id(primary key),a body(the content of the post),a timestamp and an additional field which references the post to its author-the user which posted it(referenced by using a foreign key).The relation between users and posts its "1-N" relation meaning that a user can have many posts.
For the timestamp we use a UTC format giving it a default time of the post.Using UTC format is easier as in the server it will be converted to the users current hour.
We reference the user_id of the posts table as a foreign key to the id of the table user.
We modified the User table to include the posts using the db.relationship() function to link the user and the posts made.
The first argument is then N part of the relation-"Posts",the backref is the 1 in the relation which is represented by "backref" which is the author and the "lazy" argument represents how the db query for the relationship will be issued.
We once again will migrate it and apply the changes to the database.



