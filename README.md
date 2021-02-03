
# API Documentation For Capstone Casting Agency
## Motivation
I developed this project so as to make use of the knowledge I acquired in this nanodegree and hence gain confidence in these skills.
And can provide inputs to the company I am working for.
## About
The Casting Agency models a company that is responsible for creating movies and managing and assigning actors to those movies. 
The application aims at streamlining and simplifying the process of CRUD operations for actors and movies.


## Getting Started

### Installing Dependencies 

### Python 3.7
Install the latest version of python for your platform.

#### PIP Dependencies

Run the below to install all the necessary dependencies:

```bash
pip install -r requirements.txt
```

This will install all the required packages.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py. 

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server. 

## Running the server

To run the server, execute:
For Linux:
```
 export FLASK_APP=__init.py__
 export FLASK_ENV=development
 python3 -m flask run
 ```
 For Windows:
```
 set FLASK_APP=__init.py__
``` 
We can now also open the application via Heroku using the URL:
https://bhagyash-capstone-agency.herokuapp.com/


## DATA MODELING:
#### models.py
The schema for the database and helper methods to simplify API behavior are in models.py.

Does not use raw SQL or only where there are not SQLAlchemy equivalent expressions.
Correctly formats SQLAlchemy to define models
Creates methods to serialize model data and helper methods to simplify API behavior such as insert, update and delete.
####Models:
-Movies with attributes title and release date
-Actors with attributes name, age and gender

## API ARCHITECTURE AND TESTING
### Roles and permissions

1.Casting Assistant : Permissions for GET operations. \
2.Casting Director  : Permissions for GET,POST,PATCH,DELETE for actor and GET,PATCH for movie \
3.Executive Producer: Permissions for GET,POST,PATCH,DELETE for actor and movie \
### Endpoint Library

RESTful principles are followed throughout the project, including appropriate naming of endpoints, use of HTTP methods GET, POST, and DELETE.

Routes perform CRUD operations.

The @app.errorhandler decorator has been utilized to format error responses as JSON objects fordifferent status codes
 A custom @requires_auth decorator has been included to enable Role Based Authentication and roles-based access control (RBAC) in the application.
 
The JWT token needs to be passed for RBAC for every endpoint.

JWT tokens have been provided in setup.sh.
In case of token expiration,it can be generated using the below steps:
1. Go to http://capstone-admin.us.auth0.com/authorize?response_type=token&client_id=QvsqXaFdw95ybQhMNelS25IaGP3OAmd5&redirect_uri=http://127.0.0.1:8100

2. Click on Login button and enter the below mail ids and passwords as per the requirement:
Assistant(asst@gmail.com)
Director(director@gmail.com)
Producer(producer@gmail.com)
Passsword for all the users: Capstone11@

3.Run the below to load the environment variables 
```
source setup.sh
```

#### GET '/actors'
Returns a list of all available actors and return status code.
```
 curl -i -H "Content-Type: application/json" -X GET -H "Authorization: Bearer $DIRECTOR_TOKEN" http://localhost:5000/actors
```
```  
{
  "actors": [
    {
      "age": 40,
      "gender": "M",
      "id": 2,
      "name": "Christian Bale"
    },
    {
      "age": 45,
      "gender": "M",
      "id": 4,
      "name": "Bradley Cooper"
    },
    {
      "age": 24,
      "gender": "Male",
      "id": 5,
      "name": "DiCaprio"
    }
  ],
  "success": true
}
```
#### GET '/movies'
Returns a list of all available movies and return status code.
```
curl -i -H "Content-Type: application/json" -X GET -H "Authorization: Bearer $ASSISTANT_TOKEN" http://localhost:5000/movies
```
```
{
  "movies": [
    {
      "id": 2,
      "release_date": "Thu, 15 Dec 2016 00:00:00 GMT",
      "title": "Silver Lining"
    },
    {
      "id": 3,
      "release_date": "Sat, 16 Dec 2017 00:00:00 GMT",
      "title": "Sherlock Holmes"
    },
    {
      "id": 4,
      "release_date": "Mon, 17 Dec 2018 00:00:00 GMT",
      "title": "LA Streets"
    },
    {
      "id": 5,
      "release_date": "Thu, 01 Feb 2018 00:00:00 GMT",
      "title": "Designated Survivor"
    },
    {
      "id": 6,
      "release_date": "Thu, 01 Feb 2018 00:00:00 GMT",
      "title": "Designated Survivor"
    }
  ],
  "success": true
}
```
#### POST '/actors'
Can be used to add new actor details.
```
 curl -i -H "Content-Type: application/json" -X POST -H "Authorization: Bearer $PRODUCER_TOKEN" -d '{"name":"Bridgerton","gender":"M","age":"25"}' http://localhost:5000/actors
```
```
{
  "created": 12,
  "message": "Successfully added actor",
  "success": true
}
```
#### POST '/movies'
Can be used to add new movie details.Returns the id and success message.
```
 curl -i -H "Content-Type: application/json" -X POST -H "Authorization: Bearer $PRODUCER_TOKEN" -d '{"title":"Bridgerton", "release_date":"2021-02-01"}' http://localhost:5000/movies
```
```
 {
  "created": 7,
  "message": "Successfully added movie",
  "success": true
}
```
#### PATCH '/actors/update/{actor_id}'
Used to update actor details by id.
```
 curl -i -H "Content-Type: application/json" -X PATCH -H "Authorization: Bearer $PRODUCER_TOKEN" -d '{"name":"Bridgerton","gender":"M","age":"30"}' http://localhost:5000/actors/update/12
```
```
{
  "actor": 12,
  "message": "Successfully updated actor",
  "success": true
}
```
#### PATCH '/movies/update/{moive_id}'
Used to update movie details by id.
```
curl -i -H "Content-Type: application/json" -X PATCH -H "Authorization: Bearer $PRODUCER_TOKEN" -d '{"title":"Bridgerton", "release_date":"2021-04-01"}' http://localhost:5000/movies/update/7
```
```
{
  "message": "Successfully updated movie",
  "movie": 7,
  "success": true
}
```
#### DELETE '/actors/delete/{actor_id}'
Delete actor details by id from the database
```
curl -i -H "Content-Type: application/json" -X DELETE -H "Authorization: Bearer $PRODUCER_TOKEN" http://localhost:5000/actors/delete/12
```
```
{
  "delete": 12,
  "message": "Successfully deleted actor",
  "success": true
}
```
#### DELETE '/movies/delete/{movie_id}'
Delete movie details by id from the database
```
 curl -i -H "Content-Type: application/json" -X DELETE -H "Authorization: Bearer $PRODUCER_TOKEN" http://localhost:5000/movies/delete/7
```
```
{
  "delete": 7,
  "message": "Successfully deleted movie",
  "success": true
}
```
## Testing
There are 17 unittests in test_app.py. To run this file use:
```
python test_app.py
```
The tests include one test for expected success and error behavior for each endpoint, and tests demonstrating role-based access control, 
where all endpoints are tested with and without the correct authorization.
