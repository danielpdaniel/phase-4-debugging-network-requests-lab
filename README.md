# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
    -tried adding a new toy, checked Network tab of the browser tools and saw there was a 500 error "uninitialized constant ToysController::Toys"
    -adding a byebug to the create method in the toys_controller, i could see that the params were being sent and the private method toy_params was being sent through, but we were still getting a name error for "Toys", I then realized Toys was plural and since we're talking about the model and not the table, it should be "Toy" singular!

- Update the number of likes for a toy

  - How I debugged:
    -ok this one is giving a status code of 204:No Content. The payload tab of the Network tab makes it look like it knows what we're updating and is sending the right number of likes to update. I'm thinking we just forgot to render the proper json in the patch route handler in the toys_controller file
    -looks like that was the problem! I added the render json: toy to the update method in toys_controller and it seems to be working now!

- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:
    -this time we got 404 not found error, checking the network panel it says "ActionController::RoutingError: No route matches [DELETE]" making me think we gotta check our routes file in the config folder to make sure we're including the delete path in the routes
    -added :delete to the routes that were included in the routes file's resources, still wasn't working, and wasnt triggering the byebug, checked "rails routes" in the terminal saw that it wasnt showing up then realized id put :delete as the method name instead of :destroy! 
