<h2>Mediator / Middleware Pattern: Use a central mediator object to handle communication between components</h2>

1. Definitions:
The mediator pattern makes it possible for components to interact with each other through a central point: the mediator. Instead of directly talking to each other, the mediator receives the requests, and sends them forward! In JavaScript, the mediator is often nothing more than an object literal or a function.


2. Case study: Express JS

`Express.js` is a popular web application server framework. We can add callbacks to certain routes that the user can access. Say we want add a header to the request if the user hits the root /. We can add this header in a middleware callback.

```js
  const app = require("express")()

  app.use("/", (req, res, next) => {
    req.headers["test-header"] = 1234;
    next()
  })
```

The `next` method calls the next callback in the request-response cycle. We'd effectively be creating a chain of middleware functions that sit between the request and the response, or vice versa.

No more details