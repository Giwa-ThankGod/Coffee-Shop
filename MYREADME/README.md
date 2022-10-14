### Getting Started

# BACKEND SETUP

## Setting up the Backend

### Install Dependencies

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

2. **Virtual Environment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

3. **PIP Dependencies** - Once your virtual environment is setup and running, install the required dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

#### Key Pip Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use to handle the lightweight SQL database. You'll primarily work in `app.py`and can reference `models.py`.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross-origin requests from our frontend server.

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

## Running the server

From within the `./src` directory first ensure you are working using your created virtual environment.

Each time you open a new terminal session, run:

```bash
export FLASK_APP=api.py;
```

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## Getting Setup

> _tip_: this frontend is designed to work with [Flask-based Backend](../backend). It is recommended you stand up the backend first, test using Postman, and then the frontend should integrate smoothly.

### Installing Dependencies

#### Installing Node and NPM

This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (the download includes NPM) from [https://nodejs.com/en/download](https://nodejs.org/en/download/).

#### Installing Ionic Cli

The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI is in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

#### Installing project dependencies

This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the `frontend` directory of this repository. After cloning, open your terminal and run:

```bash
npm install
```

> _tip_: **npm i** is shorthand for **npm install**

## Required Tasks

### Configure Environment Variables

Ionic uses a configuration file to manage environment variables. These variables ship with the transpiled software and should not include secrets.

- Open `./src/environments/environments.ts` and ensure each variable reflects the system you stood up for the backend.

## Running Your Frontend in Dev Mode

Ionic ships with a useful development server which detects changes and transpiles as you work. The application is then accessible through the browser on a localhost port. To run the development server, cd into the `frontend` directory and run:

```bash
ionic serve
```

> _tip_: Do not use **ionic serve** in production. Instead, build Ionic into a build artifact for your desired platforms.
> [Checkout the Ionic docs to learn more](https://ionicframework.com/docs/cli/commands/build)

## Key Software Design Relevant to Our Coursework

The frontend framework is a bit beefy; here are the two areas to focus your study.

### Authentication

The authentication system used for this project is Auth0. `./src/app/services/auth.service.ts` contains the logic to direct a user to the Auth0 login page, managing the JWT token upon successful callback, and handle setting and retrieving the token from the local store. This token is then consumed by our DrinkService (`./src/app/services/drinks.service.ts`) and passed as an Authorization header when making requests to our backend.

### Authorization

The Auth0 JWT includes claims for permissions based on the user's role within the Auth0 system. This project makes use of these claims using the `auth.can(permission)` method which checks if particular permissions exist within the JWT permissions claim of the currently logged in user. This method is defined in  `./src/app/services/auth.service.ts` and is then used to enable and disable buttons in `./src/app/pages/drink-menu/drink-form/drink-form.html`.


## API Reference

### Getting Started with the API
- Base URL: At present this app can only be run locally and is not hosted as a base URL. The backend app is hosted at the default, `http://127.0.0.1:5000/`, which is set as a proxy in the frontend configuration. 
- Authentication: This version of the application requires and utilizes a third party authentication(Auth0). 
Get access token by visting this domain.
`https://coffee-shopp.us.auth0.com/authorize?audience=Coffee-Shop&scope=SCOPE&response_type=token&client_id=Jo5X0tzmyjT1z43PWf4rv8OiiJwNuikZ&redirect_uri=https://127.0.0.1:8100/&state=STATE`

### Error Handling
Errors are returned as JSON objects in the following format:
```
{
    "success": False, 
    "error": 400,
    "message": "bad request"
}
```
The API will return Six error types when requests fail:
- 400: Bad Request
- 401: Unauthorized Access
- 404: Resource Not Found
- 405: Method Not Allowed
- 422: Not Processable
- AuthError: Custom Error Class

### Endpoints 
#### GET /drinks
- General:
    - Public endpoint, requires no authentication
    - Returns a list of drinks with a shortend information about drink recipe.

- Sample: `curl http://127.0.0.1:5000/drinks`

```
{
  "drinks": [
    {
      "id": 1,
      "recipe": [
        {
          "color": "blue",
          "parts": 1
        }
      ],
      "title": "water"
    }
  ],
  "success": true
}
```

#### GET /drinks-detail
- General:
    - Requires a valid authorization token from Auth0.
    - Authorization token must contain the permission 'get:drinks-detail'.
    - Returns a list of drinks with a full information about drink recipe.

- Sample: `curl -H "Authorization: Bearer {your token}" http://127.0.0.1:5000/drinks-detail`

```

```

#### POST /drinks
- Requires a valid authorization token from Auth0.
- Authorization token must contain the permission 'post:drinks'.
- Creates a new drink using the submitted title and recipe parameter. 
- Returns a list of drinks with a full information about drink recipe.
- `curl http://127.0.0.1:5000/drinks -X POST -H "Authorization: Bearer {your token}, Content-Type: application/json" -d '{"title": "wine","recipe": [{"color": "red","name": "red-wine","parts": 2}]}'`

```

```


#### PATCH /drinks/{drink_id}
- Requires a valid authorization token from Auth0.
- Authorization token must contain the permission 'patch:drinks'.
- Updates an existing drink using the submitted title and recipe parameter. 
- Returns a list of drinks with a full information about drink recipe.
- `curl http://127.0.0.1:5000/drinks/2 -X PATCH -H "Authorization: Bearer {your token}, Content-Type: application/json" -d '{"title": "wine","recipe": [{"color": "red","name": "champagne","parts": 2}]}'`

```

```

#### DELETE /drinks/{drink_id}
- Requires a valid authorization token from Auth0.
- Authorization token must contain the permission 'delete:drinks'.
- Deletes the drink of the given ID if it exists. Returns the id of the deleted drink and the success value. 
- `curl -X DELETE -H "Authorization: Bearer {your token}" http://127.0.0.1:5000/drinks/1`

```
{
  "delete": 1,
  "success": true
}
```