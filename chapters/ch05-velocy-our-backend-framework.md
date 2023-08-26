[![Read Prev](/assets/imgs/prev.png)](/chapters/ch04.5-rolling-file-support.md)

# [Velocy](https://github.com/ishtms/velocy) - Our backend framework

> Note: This this chapter will give you an overview of what we'll build. This chapter includes terms that may not be familiar to you, for example a middleware, or an HTTP verb. We'll cover every basic detail in the next chapter. 

## What is a backend framework/library anyway?

A backend framework or library is a set of tools that helps developers create the server-side parts of web applications more easily and quickly. It's like a toolbox built on top of a programming language, and it simplifies the development process by hiding complex technical details and providing standard tools and methods to use.

For example, you could write performant server applications with Node.js, but that process is time consuming. Instead, you use a library which includes all the necessary features that you require for your server application.

However, in this book, we are going to take the hard route - build our own backend library.

> Note: I am going to use the terms library/framework interchangeably. In practice, they are somewhat different. 

## Core features of our backend framework

### Routing and URL Handling:

Routing is a basic part of web apps. It tells how requests are linked to specific parts of the application. Routes are essential for structuring and organizing the endpoints of our API, making it easier for clients. Here are some examples of routes:

- `GET /api/users`: Retrieves a list of users.

- `GET /api/users/:id`: Retrieves a specific user by their ID.

- `POST /api/users`: Creates a new user.

- `PUT /api/users/:id`: Updates an existing user.

- `DELETE /api/users/:id`: Deletes a user.

> We'll take a look at what `:id` means, in a bit.

In a backend library, a strong routing system makes it simpler to create and manage routes. This helps developers who use our library, create well-organized applications. Here's why getting "routing and URL handling" done right is very important:

- **Endpoint Mapping:** We should make it easy for developers to create endpoints and associate them with the right actions or handlers. Developers should be able to specify which function or method will run when a specific URL is accessed. 
  
  This is important in RESTful APIs where different HTTP methods (GET, POST, PUT, DELETE, etc.) must be linked to specific actions.

- **Parameter Extraction**: URLs sometimes have dynamic parameters, like IDs or slugs. A good routing system lets developers define placeholders in the URL and extract these parameters to use in the associated handler. This feature is important for making dynamic and data-driven applications.
  
  For example, let's take a look at an URL endpoint:
  
  ```
  GET /api/games/:type
  ```
  
  Developers who use our library should be able to configure the URLs like above, and we as a library should provide them with the ability to extract useful info for them, whenever someone makes a request like this:
  
  ```
  https://ourapi.com/api/games/multiplayer?limit=10&order=asc
  ```
  
  This should extract the `:type` "path" parameter, that is `multiplayer` and `limit`, `order` "query" parameters, which are `10` and `asc` respectively. 
  
  > There are also other type of parameters, some of them are headers, body, cookies. We'll learn in-depth about those in the next chapter.

- **Route Hierarchies**: Modern applications often have complex route hierarchies. We should allow them to build specific part of the API separately, and then merge them altogether. 
  
  For example: Usually the API endpoints are prefixed with `/api/version`, and typing them out in every single route handler is quite cumbersome. What if our library offered a functionality to nest certain routes under a a specific pattern.
  
  ```js
  let v1_router = velocy.base_route('/api/v1')
  
  let users_router = velocy.base_route('/users')
  
  let add_user = velocy.get('/', add_user_callback)
  let delete_user = velocy.delete('/:user_id', delete_user_callback)
  
  // Nest `users_router` inside `v1_router`, and `add_user`, `delete_user` inside `users_router`
  v1_router.nest(
      users_router.nest(add_user, delete_user)
  );
  ```


  This way, whatever requests that hit the endpoint `GET /api/v1/users/` will be forwarded to the `add_user_callback` function, and the requests hitting `DELETE /api/v1/users/some_id` will be forwarded to `delete_user_callback` function.

  Wouldn't this be so cool?

- **Handling HTTP Verbs:** HTTP verbs, like `GET`, `POST`, `PATCH`, `DELETE`, `PUT` etc. play a crucial role in specifying the intended action for a request. Our library's routing system should allow developers to associate different HTTP verbs with appropriate route handlers. This ensures that the application responds correctly to different types of requests. We'll talk more about HTTP verbs in the next chapter.

- **Regex Support:** With regular expressions (regex), developers can create a flexible and powerful path matching mechanism that dynamically routes incoming requests to the appropriate handlers based on the URL structure. 

  However, certain regex patterns may need to go through the input string an exponential number of times, taking O(2^n) time. This won't be an issue with small URLs, but an attacker might try to exploit this behavior by providing specially crafted input strings that trigger excessive backtracking, leading to a significant slowdown or even crashing of the application. This is known as a[ ReDoS (Regex Denial of Service) attack.](https://en.wikipedia.org/wiki/ReDoS)

- **Request and Response Handling:** Our routing system should provide an abstraction for handling incoming requests and generating appropriate responses. This could involve parsing request data, handling headers, and sending back structured responses.

### Middlewares

Middleware is an important concept. It lets developers add their own code to the process of handling requests and responses. Middleware functions are like a middleman between the incoming request and the final response. They let developers do different things before and after the main application code runs. 

For example,

```js
let fetch_tweets = velocy.get('/tweets', rate_limiter, auth_middleware, fetch_tweets_handler)
```

Before executing the main function `fetch_tweets_handler` the request will go through a series of middlewares. In the case above, the middlewares are `rate_limiter` and `auth_middleware`. These middlewares can be reused. The request that hits the `/tweet` endpoint, first goes through the `rate_limiter` middleware function, it can either approve the request, or reject.

If the request is rejected, it does not go to the next middleware, or the main function.

### Building our own database

We will also create a basic in-memory key-value database. This mini in-memory database will provide users with a lightweight and efficient solution for storing and retrieving data within their applications. 

While it won't have the full range of capabilities found in dedicated databases, it will serve as a valuable tool for scenarios where a simple and fast data storage option is needed, without relying on other third party tools.

Our mini in-memory database with index support has the following key features:

#### Data Storage and Retrieval:

- Stores structured data in tables.
- Quickly retrieves data based on primary key or indexed fields.

#### Indexing:

- Supports indexing of key fields to speed up data retrieval.
- Has basic indexing mechanisms to optimize query performance.

> We won't focus on this target in the initial chapters, but we'll cover it as we approach the end of the book.

#### CRUD Operations:

- Performs Create, Read, Update, and Delete operations for managing data.
- Has a simplified interface for these operations.

#### Querying:

- Allows basic filtering and sorting operations to retrieve data based on specified criteria.
- Has support for simple filtering and sorting operations.

### Caching

Caching means saving often-used data in a memory. When the data is needed again, the app can get it from the cache instead of doing the calculations or getting it from the original source. This makes things much faster and smoother for the user.

Caching can help lessen the work on the database server. Instead of asking the database for the same data many times, the application can get it from the cache. This not only makes things faster but also makes the load very less on the database.

This also helps significantly when your web server is under huge load, caching improves your server's ability to handle many requests if the same piece of data is requested over and over again. However, caching also result in stale data. We'll address this in the later chapters of this book.

### Rate limiting

API rate limiting is a way to control how often clients, like apps or users, can ask an API for things. This is important to stop people from using too much of the API and to keep the API and server working well.

### Some other features that we will be implementing

- Shared state

- File uploads

- Static file serving

- Multi-part data

- Websockets

- Logging (using [`logtar`](https://github.com/ishtms/logtar))

- Monitoring

We will begin building our backend library/framework in the upcoming chapters. However, before doing so, we need to have a strong understanding of HTTP. Let's tackle that first in the next chapter.

[![Read Next](/assets/imgs/next.png)](/chapters/ch06.0-http-deep-dive.md)

![](https://uddrapi.com/api/img?page=Velocy_5.0)