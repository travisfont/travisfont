# Post-Next (Deferred) Logging Middleware


### Example middleware

```js
function logger(req, res, next)
{
  next(); // passes control immediately
  console.log(req.method, req.url);
}
```

###C Calling next() First

Calling `next()` as the first statement hands control to the next middleware in the stack before the logging code runs.
The function continues to execute after `next()` returns, so the log line executes later in the same tick of the event loop.

This creates an effect where downstream middleware runs first, then the log appears afterward.
The request has not ended yet, so the middleware can still log information safely.

### Mounting the middleware (at the start of the stack)

```js
app.use(logger);

// .. rest of routing code
// app.get(...) 
```

Placing this as the first `app.use()` call inserts the logger at the top of the middleware stack.
Every incoming request reaches `logger` before reaching the rest of the stack. Because `next()` runs first, the request flows immediately to the next middleware.
Once `next()` completes, execution returns to the remaining statements in `logger`, producing the log output.

### Resulting Execution Order

1. Request arrives
2. `logger` runs
3. `logger` calls `next()`
4. Next middleware runs
5. Control returns to `logger` and the log statement executes
6. Response continues normally

### Pattern Use Case

This pattern allows the log to capture information *after* the next middleware has executed.
It can be useful for timing, response metadata, or observing side effects produced by later middleware, without delaying the request pipeline at the start.

