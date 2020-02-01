# Casting Agency Backend
Capstone is a Casting Agency which models a company that is responsible for creating movies and managing and assigning actors to those movies. You are an Executive Producer within the company and are creating a system to simplify and streamline your process.

This project has been created to demonstrate my newly learned skills to create a a system that simplifies and streamlines the processes of different stakeholders within the company.

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the root directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) and [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) are libraries to handle the lightweight sqlite database. Since we want you to focus on auth, we handle the heavy lift for you in `./src/database/models.py`. We recommend skimming this code first so you know how to interface with the Drink model.

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

### Setting up Auth0

1. Create a new Auth0 Account
2. Select a unique tenant domain
3. Create a new, single page web application
4. Create a new API
    - in API Settings:
        - Enable RBAC
        - Enable Add Permissions in the Access Token
5. Create new API permissions:
    - `get:actors`
    - `post:actors`
    - `patch:actors`
    - `delete:actors`
    - `get:movies`
    - `post:movies`
    - `patch:movies`
    - `delete:movies`
6. Create a new machine to machine application called Casting         Assistant.
    - For each application created: Authorize the application by selecting at least one authorized API and select permissions required.
    - Repeat steps above for Casting Director and Executive Producer

    Permissions allowed for the different users:
    - Casting Assistant
        - can `get:movies`
        - can `get:actors`
    - Casting Director
        - All permissions a Casting Assistant has and…
        - can `post:actors`
        - can `delete:actors`
        - can `patch:actors`
        - can `patch:movies`
    - Executive Producer
        - All permissions a Casting Director has and…
        - can `post:movies`
        - can `delete:movies`

    - Take note of the client_id and client_secret of the different applications created. These are to be used
      in the environment variables. Check the .env-example for more environment variables.

## Database Setup
With Postgres running, create two databases.
- From the project folder, in terminal run:
```bash
createdb capstone
```
Create another database that will be used for running tests.
```bash
createdb capstone_test
```
Ensure that the user has all privileges access to both databases. If you need additional information on setting up the PostgreSQL databases, checkout the [PostgreSQL tutorial](http://www.postgresqltutorial.com/).

## Running the server

From within the project directory first ensure you are working using your created virtual environment.

To run the server:

- Create a `.env` file in the root of the project directory. 
- Copy the contents of `.env.example` into the `.env` file and update them accordingly.
- Edit with the database information from the previous section.
- Add the Auth0 machine to machine applications' `Client ID` and `Client Secret`.
- Run the command `source .env` to load the environment variables.
- Run the tests `pytest`. If the tests all pass, then the setup was successfull.
- To run the application on `http://localhost:5000`, execute:

```bash
flask run
```
## API Documentation.
7. Endpoints

    | Functionality            | Endpoint                      | Casting assistant  |  Casting Director  | Executive Producer |
    | ------------------------ | ----------------------------- | :----------------: | :----------------: | :----------------: |
    | Fetches a list of actors | GET /actors                   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
    | Fetches a list of movies | GET /movies                   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
    | Fetches a specific actor | GET /actors/&lt;id&gt;        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
    | Fetches a specific movie | GET /movies/&lt;id&gt;        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
    | Creates an actor         | POST /actor                   |        :x:         | :heavy_check_mark: | :heavy_check_mark: |
    | Patches an actor         | PATCH /actors/&lt;id&gt;      |        :x:         | :heavy_check_mark: | :heavy_check_mark: |
    | Delete an Actor          | DELETE /actors/&lt;id&gt;     |        :x:         | :heavy_check_mark: | :heavy_check_mark: |
    | Creates a movie          | POST /movies                  |        :x:         |        :x:         | :heavy_check_mark: |
    | Deletes a movie          | DELETE /movies/&lt;id&gt;     |        :x:         |        :x:         | :heavy_check_mark: |

    >_tip_: The endpoints are prefixed with  **api/v1** i.e GET actors **/api/v1/actors**

#### Sample Data to use for actors
```
Adding an actor. POST `/api/v1/actors` 
Request
{
	"dob": "1996-05-07",
    "gender": "Male",
    "name": "Name two"
}
Response
{
    "actor": {
        "age": 23,
        "gender": "Male",
        "id": 2,
        "name": "Name two"
    },
    "success": true
}   
```
```
Getting a single actor. GET `/api/v1/actors/1`
Response
{
    "actor": {
        "age": 23,
        "gender": "Male",
        "id": 1,
        "name": "Habib Ssentongo"
    },
    "success": true
}
``` 
```
Getting all actors. GET `/api/v1/actors`
Response
{
    "actors": [
        {
            "age": 23,
            "gender": "Male",
            "id": 1,
            "name": "Habib Ssentongo"
        },
        {
            "age": 23,
            "gender": "Male",
            "id": 2,
            "name": "Name two"
        }
    ],
    "success": true,
    "total-actors": 2
}
``` 
```
Updating details of an actor:   PATCH `/api/v1/actors/2`
Request
{
    "name": "Firstname Lastname"
}
Response
{
    "actor": {
        "age": 23,
        "gender": "Male",
        "id": 2,
        "name": "Firstname Lastname"
    },
    "success": true
}
```
```
Deleting an actor.   DELETE `/api/v1/actors/2`
Response
{
    "deleted": 2,
    "success": true
}
```

#### Sample data to use for movies
```
Adding a movie.     POST `/api/v1/movies`
Request
{     
   "title": "Movie 2",
   "release_date": "1995-12-11"
}
Response
{
    "movie": {
        "id": 3,
        "release_date": "1995-12-11",
        "title": "Movie 2"
    },
    "success": true
}
```
```
Response
Getting all movies. GET `/api/v1/movies`
{
    "movies": [
        {
            "id": 1,
            "release_date": "2011-11-11",
            "title": "Movie 1"
        },
        {
            "id": 2,
            "release_date": "2012-12-11",
            "title": "Prison Break"
        }
    ],
    "success": true,
    "total-movies": 2
}
```
```
Deleting a movie.   DELETE `/api/v1/movies/1`
Response
{
    "deleted": 1,
    "success": true
}
```
```
Getting a single movie. GET `/api/v1/movies/2`
Response
{
    "movie": {
        "id": 2,
        "release_date": "2012-12-11",
        "title": "Prison Break"
    },
    "success": true
}
```
```
Updating a movie:   PATCH `/api/v1/movies/2`
Request
{
	"title": "Mirrors"
}
Response
{
    "movie": {
        "id": 2,
        "release_date": "2012-12-11",
        "title": "Mirrors"
    },
    "success": true
}
```

#### Error Handling
Errors are returned as JSON objects in the following format:
```
{
   "success": False,
   "error": 404,
   "message": "resource not found"
}
```
The API will return the following error types when requests fail:

- 400: bad request
- 404: resource not found
- 422: unable to process request
- 405: method not allowed
- 500: internal server error
- Error codes 401 and 403 are returned for authentication and authorization errors respectively.


#### Link to [Hosted Application](https://habib-capstone-app.herokuapp.com)

#### Credits
- Udacity tutors
- Udacity reviewers
- Joaquim Irzelindo (Mentor)
