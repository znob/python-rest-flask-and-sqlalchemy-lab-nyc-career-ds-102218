
# Flask and SQL Alchemy Lab

## Introduction
In this lab we are going to practice working with creating a RESTful API that returns data about Tweets and Users. In this domain Users will have many Tweets and Tweets will, therefore, belong to a User. We will be using SQL Alechemy to make our schema and database with which to persist our data. Then we will define RESTful routes and functions that Query our database and return JSON data for each resource. Let's get started!

## Objectives
* Define RESTful Routes that Query the **User** table
* Define RESTful Routes that Query the **Tweet** table
* Define RESTful Rotues that Query a **Relationship**

**Note:** The last three routes listed are going to be *nested routes*. A nested route will contain the names for both resources. For example, if we were talking about movies and cast members and we wanted to query for a single movie's cast members our route would look like the following:
```python
"/api/movies/<int:movie_id>/cast_members" 
# returns a list of cast memebers for the movie whose id is given in the URL 
# Note that we are not returning data about the movie, but the URL is explicit about which movie we are querying
```

## Defining RESTful Routes for User Data

 The routes we want to query our User table should follow REST convention and return:
    * A list of all user objects
    * A single user object by their `id`
    * A list of users with a matching `name`

## Defining RESTful Routes for Tweet Data

The routes we want to query our Tweet table should follow REST convention and return:    
    * A list of all tweet objects
    * A single tweet object by its `id`

## Defining RESTful Routes that Query a Relationship

Since we are dealing with a has many / belongs to relationship we will want to define routes that return data that shows these relationships. We will want routes that, again follow the REST convention and return data for:
    * Tweets that belong to a user by `user_id`
    * Tweets that belong to a user by a user's `name`   
    * A single User that is associated to a tweet by its `id` 

## Summary

In this lab, we practiced desigining a RESTful API that returns JSON data for Users and Tweets. We then went further and defined routes that would return information specific to the relation ship between a Tweet and a User and vice versa. By creating an API like this, we can see that it becomes much easier to leverage this information across other applications as well as our own.
