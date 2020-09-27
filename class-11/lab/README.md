# LAB: Authentication

**Authentication System Phase 1:** Deploy an Express server that implements Basic Authentication, with signup and signin capabilities, using a Mongo database for storage.

## Before you begin

Refer to *Getting Started*  in the [lab submission instructions](../../../reference/submission-instructions/labs/README.md) for complete setup, configuration, deployment, and submission instructions.

Create a UML diagram of the authentication system on a whiteboard before you start

> Create a new repository for this project, called 'auth-server' and work in a branch called 'class-11'

## Business Requirements

Refer to the [Authentication System Overview](../../apps-and-libraries/auth-server/README.md) for a complete review of the application, including Business and Technical requirements along with the development roadmap.

## Phase 1 Requirements

Today, we begin the first of a 4-Phase build of an authentication system, written in Express. The following user/developer stories detail the major functionality for this phase of the project.

- As a user, I want to create a new account so that I may later login
- As a user, I want to login to my account so that I may access protected information
- As a developer, I want to create an authentication server so that our application can restrict access to certain routes
- As a developer, I want to create a authentication as a reusable module, so that I can use it in other projects
- As a developer, I want to use industry standards for http based basic authentication
- As a developer, I want to provide applications and users a token following authentication to facilitate re-authentication on subsequent requests

## Technical Requirements / Notes

Build an Express Server with the following features

- `index.js` and `server.js` should be created with standard express server scaffolding
- Work in a sub-folder folder of your `src` folder called `auth` for all of the authentication specific modules
- Connect the server to a Mongo database
- Create a `Users` Mongoose model/schema in the auth system
  - Before we save a record:
    - Hash the plain text password given before you save a user to the database
  - Create a method in the schema to authenticate a user using the hashed password
  - Create a method in the schema to generate a Token following a valid login
- Create a module to house all of routes for the authentication system.
  - Create a POST route for `/signup`
    - Accepts either a JSON object or FORM Data with the keys "username" and "password"
    - Creates a new user record in a Mongo database
  - Create a POST route for `/signin`
    - `router.post('/signin', basicAuth, (req,res) => {});`
    - Uses middleware (BasicAuthentication) to validate the user
    - When validated, send a JSON object as the response with the following properties:
      - `token`: The token generated by the users model
      - `user`: The users' database record
      - Additionally, set a Cookie and a Token header on the response, with the token as the value
  - Create a GET route for `/users` that returns a JSON object with all users
    - Stretch Goal: have this route also use the middleware for authentication so that you cannot see the user list without a valid username and password
- Basic Authentication Middleware
  - Reads the encoded username and password from the Authentication header
  - Checks the `Users` model to see if this is a valid user and the right password
  - If the user is valid, generate a token and append it to the `request` object
    - If valid, call `next()`
    - Otherwise, call `next()` with an error as an argument

### Implementation Notes

- At some future point, we will want to be able to re-use the entire `auth/` folder in other servers.
  - So, keep the `index.js` and `server.js` modules as agnostic and small as possible

### Testing

You should manually test your routes using `httpie` from the command line or an application such as Postman or Insomnia.  You are required to write automated tests as well:

- POST to /signup to create a new user
- POST to /signin to login as a user (use basic auth)
- Need tests for auth middleware and the routes
  - Does the middleware function (send it a basic header)
  - Do the routes assert the requirements (signup/signin)
- This is going to require more "end to end" testing that you've done in the past
  - To test signin, your tests actually need to create a user first, then try and login, so there's a dependency built in
- Ensure that you use supergoose to test your routes and your database

### Visual Validation

We have deployed a web application that's designed to test your API. This is a good way to ensure that your API works as expected. There's nothing to "turn in" here, this is provided for your benefit.

- Open this [Web Application](https://javascript-401.netlify.app/)
  - Click the "Module 3 (AUTH)" / Basic Auth link
  - In the form at the top of the page, enter the URL to your Authentication Server
  - If your lab is working, this app will show the JWT Token your application provides

**Engineering Note** - *Testing servers without side-effects is crucial. More critical is not having to manage starting/stopping a server in multiple environments.*

## Assignment Submission Instructions

Refer to the the [Submitting Standard Node.js Lab Submission Instructions](../../../reference/submission-instructions/labs/node-apps.md) for the complete lab submission process and expectations