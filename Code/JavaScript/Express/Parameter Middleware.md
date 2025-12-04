# Parameter Middleware
> Express.js

### Scopes

| Feature          | `app.param()`          | `router.param()`                               |
| ---------------- | ---------------------- | ---------------------------------------------- |
| Applies to       | The entire Express app | Only to routes defined on that specific router |
| Global or local? | **Global**             | **Scoped / Local**                             |

**Example:**
If you define `app.param('id', ...)`, *every route in the whole app* using `:id` will trigger that param middleware.
If you define `router.param('id', ...)`, then only routes inside that router use it.


## Use Cases

### Using app.param()

* You want **centralized** parameter processing.
* All your routers share the same logic for that parameter.
* Example: load a user by ID for any `/users/:id`, `/posts/:id`, etc.

### Using router.param()

* The parameter logic should be **specific to one router**.
* You have multiple routers that use parameters with the same name but **different semantics**.


## Example Comparison

### app.param() example (global)

```js
app.param('userId', (req, res, next, userId) => {
  req.user = db.users.find(userId);
  next();
});

app.get('/users/:userId', (req, res) => {
  res.send(req.user);
});
```

This *automatically* runs the middleware for **any** route with `:userId`.

---

### router.param() example (scoped)

```js
const router = express.Router();

router.param('productId', (req, res, next, productId) => {
  req.product = db.products.find(productId);
  next();
});

router.get('/products/:productId', (req, res) => {
  res.send(req.product);
});

app.use('/api', router);
```

Only routes **inside this router** will use that parameter logic.


## Important Implementation Detail

Both functions attach middleware that looks like this internally:

```
paramName → callback(req, res, next, value)
```

However **`router.param` is not inherited by `app`, nor vice-versa**—each one attaches to its own layer.


##  When They Conflict

If you have **both**:

```js
app.param('id', ...)
router.param('id', ...)
```

Then for routes under `router`:

1. `app.param('id')` runs first (global)
2. `router.param('id')` runs second (router-specific)

This lets you override global logic at the router level.


## Summary

| Category          | `app.param()`              | `router.param()`               |
| ----------------- | -------------------------- | ------------------------------ |
| Scope             | Global app                 | Specific router                |
| Affects           | All routes with that param | Only routes within that router |
| Good for          | Shared parameter logic     | Encapsulated router logic      |
| Runs before/after | Runs first                 | Runs after app param           |
