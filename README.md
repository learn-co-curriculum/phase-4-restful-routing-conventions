# RESTful Routing Conventions

## Learning Goals

- Explain the benefits of the REST design pattern
- Understand the uses for different HTTP verbs
- List the 5 RESTful routes and their corresponding Rails actions

## What is REST?

In 2000, Roy Thomas Fielding was frustrated by the haphazard ways in which web
applications were using the HTTP standard. Specifically, he was frustrated with
how URLs and their corresponding HTTP verbs were used differently for every
single application. So, in his PhD dissertation, he came up with **REST**
(**RE**presentational **S**tate **T**ransfer) as a standard way web apps should
structure their URLs.

Fielding also noticed the rise in web applications communicating with each
other. He hoped that inter-application communication would get much easier if
there was a standard way of forming URLs to access resources.

If you have been building applications for a while, there is a good chance that
you have already worked with RESTful APIs. `json-server` follows REST
conventions very strongly. If your application posts to Twitter, pulls in a feed
of images from Instagram, or calls a list of locations from Google Maps, you are
using a RESTful API to communicate between applications.

## Example REST Workflow

For a real world case study, let us pretend that you have a newsletter
application. The following is a high-level view of how such an app might work:

1. You fill out the form on the 'New Newsletter' page and click submit.

2. Data concerning you as the author, your newsletter content, and any
   additional information such as media items is sent to the application server.

3. The server interprets the information, recognizes that the request is for a
   new newsletter, generates the new record in the database, and performs
   background tasks (updating the newsletter counter, possibly sending
   notification emails, etc).

4. Next, the server sends a response back to the client. This does not
   necessarily mean that the newsletter was posted. The response could be that
   there was an error posting or something like that. However, in this case we
   will say that the newsletter post went through properly, so the server sends
   a success message and tells the browser which page to go to and render.

5. Lastly, the browser receives the server information and shows you a message
   saying that your newsletter was successfully posted.

## RESTful Conventions in Rails

Rails has RESTful principles built into its core, so, whether you are utilizing
the built-in view rendering system or using the application purely as an API,
you will have the tools necessary to follow standardized routing procedures.

Let's take a look at how this would work for our newsletter application. RESTful
routes fall into four general categories: Create, Read, Update, and Delete,
commonly known as 'CRUD' actions. The CRUD acronym gives us a convenient way to
refer to the set of five actions that RESTful APIs potentially include:

| CRUD category | Action(s) |
| --- | --- |
| READ | Display a list of all newsletters |
| | Display an individual newsletter |
| CREATE | Create the new newsletter instance |
| UPDATE | Update the newsletter instance |
| DESTROY | Delete an existing newsletter instance |

To implement each of the above actions, we combine an _HTTP verb_ (`GET`,
`POST`, etc.) with a route (e.g., `/newsletters`). Rails then maps each HTTP
verb/route combination to the appropriate _action_ (`show`, `edit`, etc.) in the
`NewsletterController`. The table below shows the HTTP verb, route, and
controller action names we would use for our RESTful newsletter app:

| HTTP Verb | Route | Controller#Action | Description |
| --- | --- | --- | --- |
| GET | /newsletters | newsletters#index | Show all newsletters |
| POST | /newsletters | newsletters#create | Create a new newsletter |
| GET | /newsletters/:id | newsletters#show | Show a specific newsletter |
| PATCH or PUT | /newsletters/:id | newsletters#update | Update a specific newsletter |
| DELETE | /newsletters/:id | newsletters#destroy | Delete a specific newsletter |

Note that even though we have five separate actions, there are only two routes
in the table above: `/newsletters` and `/newsletters/:id`. It is the
**combination** of the _route_ and the _HTTP verb_ that tells Rails which
controller action to use.

Rails does a great job of integrating RESTful routes into its system. If you can
understand routes in Rails, you can understand REST in general. Rails even gives
us some built-in functionality to encourage using RESTful routing conventions,
through the concept of [Resource Routing][resource-routing].

Here is a diagram that shows how the routes, HTTP verbs, and controller actions
are all mapped together:

![REST Diagram](https://curriculum-content.s3.amazonaws.com/phase-4/phase-4-restful-routing-conventions/rest-diagram.png)

In analyzing the diagram, you can see the flow of data as follows:

1. The HTTP request contains an HTTP verb and hits a specific URL path.

2. The router in the application processes the request and 'routes' the request
   data to the proper controller action.

3. The controller action either performs a task — creating, updating, or
   deleting a record in the database — or it runs a database query and returns
   the requested information to the client.

## Definition of HTTP Verbs

So what do `GET`, `POST`, et al. represent? Those are HTTP verbs that give
each HTTP request unique behavior. Below is an explanation of each verb:

- **GET**: The GET method retrieves whatever information is identified by the
  Request URI. This means if you go to `/posts`, you will get all of the posts
  that the application has.

- **POST**: The POST method is used to send data enclosed in the request to the
  server. The server is expected to use this data to create some new resource.

- **PATCH/PUT**: The PATCH and PUT methods are both used to update existing
  resources. Sending either a `PATCH` or `PUT` request to `/posts/1` will update
  the post with an `id` of 1. `PUT` is used when we want to replace an entire
  resource. `PATCH` is used when we want to update a specific part of a
  resource. Check out this explanation of the [difference between the
  two][put-v-patch].

- **DELETE**: The DELETE method requests that the server delete the resource
  identified by the Request URI. This means… that it deletes the record. It's
  nice and explicit.

## Conclusion

Below are a few keys to remember when thinking about REST:

- REST is an architectural design pattern, not a framework or code in itself.
  Many other web frameworks utilize RESTful design principles in some form or
  another. By using RESTful principles, Rails apps are able to have a clear and
  standardized naming structure for routes and actions.

- RESTful routes have a clear mapping between the URL resource and the
  corresponding controller actions.

- There are five RESTful route options we will commonly use as API developers.

## Check For Understanding

Before you move on, make sure you can answer the following questions:

1. What are the CRUD actions?
2. What HTTP verb(s) are associated with each CRUD action?
3. What Rails controller action(s) are associated with each CRUD action?
4. What two pieces of information need to be combined to create an HTTP request?

## Resources

- [What RESTful Actually Means](https://codewords.recurse.com/issues/five/what-restful-actually-means)
- [Resource Routing in Rails][resource-routing]
- [RESTacular Routing Resource](https://restacular.netlify.app/)

[put-v-patch]: https://rapidapi.com/blog/put-vs-patch/
[resource-routing]: https://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default
