
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

We need to connect SQLAlchemy to our application and create our database object, (`db`). So, we are going to take our new Flask app and use it as an argument for our SQLAlchemy module. We will set the return value to the variable `db` so we can continue to interact with our database like we have in our lessons on SQLAlchemy. Your code should now include the following:

```python
    # connect flask_sqlalchemy to the configured flask app
    db = SQLAlchemy(app)
```

We'll see that our models are already provided for us. In order to successfully create our tables in our newly connected database, we will need to use our `db` object and call the `create_all` function somewhere after we define our models.

If you look at the seed.py file, we see that we have some data already provided for us and the `db.create_all()` function is being called there before we try to create any information in our database. Run this file by writing 
`python seed.py` in your terminal.

> **Note:** Since flask_sqlalchemy contains the session object for our application, we reference it by calling `session` on our `db` object like so: `db.session`. Similarly, when we want to add our newly created objects to our database, we call `db.session.add()` and commit the new objects by calling `db.session.commit()`.

> This pattern continues to our definition of models and table columns as we can see. Recall from SQLAlchemy that our models inherited from the `Base` class.  Now they will inherit from our conjoined Flask and SQLAlchemy object.  In our class definitions for our Tweet & User model, we give the argument of `db.Model` and adding columns is done by calling `db.Column()`. Finally defining the datatype for a column is also done by calling `db.Integer`, `db.String`, or more generally `db.[data_type]` and providing the desired datatype.

```pyhon
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(20), nullable=False)
    tweets = db.relationship('Tweet', backref='users', lazy=True)
```

Finally, to start the server, we need to tell our Flask app to run with the following command at the bottom of our app.py file below where we'll put our routes.

```python
if __name__ == "__main__":
    app.run()
```

We can start our server by running `python app.py` in the terminal.  If you see a FSADeprecationWarning and your server doesn't start, simply add the following line to your Flask app configuration:

```python
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
```

And that's it! Now we need go back to our `app.py` file and create our routes. We'll be using the seed data we just created to return as JSON in our routes.  Remember that we are building an API containing JSON, so `'/api/'` should be prepended to each route URL.

## Querying Our Database in a Flask-SQLAlchemy App

We'll want our API to show the correct information for each route.  For example, if our route is `'/api/users/2'`, we will need to query our database to pull the correct information, the User with an id of 2, from our database.

As we just learned in the note above, flask_sqlalchemy wraps up a lot of the more manual functionality we are used to in a plain SQLAlchemy set-up. So, while we would typically query our database through our session object like so:

```python
# get all users from the database
session.query(User).all()
```

We now have **two** options. We can stick to this format and just prepend our `db` object:
```python
# get all users from the database
db.session.query(User).all()
```

**OR**

We can now call the query object directly on the class itself:
```python
# get all users from the database
User.query.all()
```

The last query seems to be the easiest to read and adds a great deal more clarity. Let's try using that to make our queries to the database!

## Defining RESTful Routes for User Data

 Our User resource routes should follow REST convention and query our User table to return:
 * A list of all user objects
 * A single user object whose `id` matches the id in the URL
 * A list of users with a whose `username` contains the string in the URL

## Defining RESTful Routes for Tweet Data

Our Tweet resource routes should follow REST convention and query our Tweet table to return:
* A list of all tweet objects
* A single tweet object that has the same `id` as the id in the URL

## BONUS:

## Defining Nested RESTful Routes that Query a Relationship

Since we are dealing with a has many / belongs to relationship we will want to define routes that return data that shows these relationships. We will want routes that, again follow the REST convention and return data for:
* Tweets that belong to a user by `user_id` 
    - URL: `'/api/users/<int:user_id>/tweets'`
* Tweets that belong to a user by a user's `name`
    - URL:`'/api/users/<user_name>/tweets'`
* A single User that is associated to a tweet by its `id`
    - URL: `'/api/tweets/<int:tweet_id>/user'` 

## Summary

In this lab, first we connected the flask_sqlalchemy module to our app and seeded our database with some users and tweets. Then, we practiced designing a RESTful API that returns JSON data for our new Users and Tweets. In the bonus section, we then went further and defined routes that would return information specific to the relationship between a Tweet and a User and vice versa. By creating an API like this, we can see that it becomes much easier to leverage this information across other applications as well as our own.
