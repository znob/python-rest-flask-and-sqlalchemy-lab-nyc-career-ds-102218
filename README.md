
# Flask and Flask-SQLAlchemy Lab

## Introduction
In this lab we are going to practice working with creating a RESTful API that returns data about Tweets and Users. In this domain Users will have many Tweets and Tweets will, therefore, belong to a User. We will be using the `flask-sqlalchemy` module to make our models, schema, and connect to our SQLite databse. Then we will define RESTful routes and functions that Query our database and return JSON data for each resource. Let's get started!

## Objectives
* Get started with `flask_sqlalchemy`
* Define RESTful Routes that Query the **User** table
* Define RESTful Routes that Query the **Tweet** table
* **Bonus:** Define RESTful Rotues that Query a **Relationship**

**Note:** The routes that query a Relationship are going to be *nested routes*. A nested route will contain the names for both resources. For example, if we were talking about movies and cast members and we wanted to query for a single movie's cast members our route would look like the following:
```python
"/api/movies/<int:movie_id>/cast_members" 
# returns a list of cast memebers for the movie that matches the id that is given in the URL
"/api/movies/<int:movie_id>/director" 
# returns information about the director of the movie that matches the id that is given in the URL 
# Note that we are not returning data about the movie, but the URL is explicit about which movie we are querying
```

## Get Started with flask_sqlalchemy

Alright, so, in order to both persist and return data for our request resources, we will need to connect our flask app to a database. In order to do this, we will need to set up SQL alchemy. Thankfully, there is a module that makes this helps with this set-up process. First we will need to `pip install flask_sqlalchemy`. Then we will need to update our imports so that we can use this module. In the app.py file, your imports should look like the following:
```python
    from flask import Flask, jsonify
    from flask_sqlalchemy import SQLAlchemy
```

Next, we can instantiate an new instance of Flask for our `app` like we do for all of our flask apps. Then we need to add some configuration to our app in addition to telling the app to run with `'DEBUG' = True`. We need to also tell our app where its SQLite database is. So your app's code should look like the following:
```python
    # initialize new flask app
    app = Flask(__name__)
    # add configurations and database URI
    app.config['DEBUG'] = True
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
```

Finally, we need to connect SQLalchemy to our application and create our database object, (`db`). So, we are going to take our new Flask app and use it as an argument for our SQLAlchemy module. We will set the return value to the variable `db` so we can continue to interact with our database like we have in our lessons on SQL alchemy. Your code should now include the following:

```python
    # connect flask_sqlalchemy to the configured flask app
    db = SQLAlchemy(app)
```

We'll see that our models are already provided for us. In order to successfully create our tables in our newly connected database, we will need to use our `db` object and call the `create_all` function somewhere after we define our models.

If you look at the seed.py file, we see that we have some data already provided for us and the `db.create_all()` function is being called there before we try to create any information in our database. Run this file by writing 
`python seed.py` in your terminal.

And that's it! Now all we need to do is go back to our `app.py` file and create our routes. We'll be using the seed data we just created to return in our functions.

## Defining RESTful Routes for User Data

 The routes we want to query our User table should follow REST convention and return:
    * A list of all user objects
    * A single user object by their `id`
    * A list of users with a matching `name`

## Defining RESTful Routes for Tweet Data

The routes we want to query our Tweet table should follow REST convention and return:    
    * A list of all tweet objects
    * A single tweet object by its `id`

## BONUS:

## Defining RESTful Routes that Query a Relationship

Since we are dealing with a has many / belongs to relationship we will want to define routes that return data that shows these relationships. We will want routes that, again follow the REST convention and return data for:
    * Tweets that belong to a user by `user_id`
    * Tweets that belong to a user by a user's `name`   
    * A single User that is associated to a tweet by its `id` 

## Summary

In this lab, first we connected the flask_sqlalchemy module to our app and seeded our database with some users and tweets. Then, we practiced desigining a RESTful API that returns JSON data for our new Users and Tweets. In the bonus section, we then went further and defined routes that would return information specific to the relationship between a Tweet and a User and vice versa. By creating an API like this, we can see that it becomes much easier to leverage this information across other applications as well as our own.
