# Middleware Continuation Function


## What next() Does

In Express.js, middleware functions follow this general shape:

```js
function example(req, res, next) {
  // work
  next();
}
```

`next()` advances a request through the Express middleware chain.
Proper use of this function supports predictable control flow, consistent error handling, and clean separation of concerns.
It hands control from the current middleware to the following one in the stack.
When `next()` is never called and no response is produced, the request stays open and eventually times out.**



## Fundamental Aspects

### Controls Request Flow

`next()` decides when the current middleware finishes its part of the pipeline
Once invoked, the next registered middleware (or route handler) becomes active.

### Allows Asynchronous Flow

Middleware that performs asynchronous tasks can call `next()` once those tasks are done.
This prevents blocking and keeps the pipeline responsive.

### Accepts An Error

Calling `next(err)` triggers Express error handling. If `err` is truthy, Express jumps to the first middleware with four parameters:

```js
function errorHandler(err, req, res, next) {
  res.status(500).send("Server error");
}
```


## Best Practices

> **Do not call `next()` after sending a response**

Once a response is sent, further calls to `next()` can cause headers to be sent twice or can trigger unexpected behavior.

> **Avoid multiple calls to `next()`**

A middleware function must call `next()` exactly once during normal operation.
Accidental repetition often leads to duplicated logs, double execution of downstream handlers, or thrown errors.

> **Return after calling `next()` when needed**

In branches where processing should stop after a `next()` call, a `return` statement prevents accidental execution of further code:

```js
if (!req.user) {
  return next();
}
// other operations
```

> **Use `next(err)` for errors**

Passing errors through `next(err)` ensures a consistent error handling pipeline. Throwing inside asynchronous code without proper handling can bypass Express entirely.

> **Keep middleware focused**

Middleware that calls `next()` should do one clear job such as logging, validation, authentication, or data augmentation. Focused middleware improves readability and reduces unintended side effects.
