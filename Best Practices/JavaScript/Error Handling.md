# Error Handling

## Strategic Use of Try/Catch

Error handling is one of the most critical aspects of writing robust JavaScript applications. However, not all approaches to try/catch are created equal. The placement and design of your error handling can dramatically impact your code's maintainability, debuggability, and overall quality.

### The Tale of Two Approaches

Consider this common scenario: you have a function that capitalizes all elements in an array. How should you handle potential errors? Let's examine two different approaches and understand why one is significantly better than the other.

### Approach A: Internal Error Swallowing

```javascript
function capAllElements(arr)
{
  try {
    arr.forEach((el, index, array) => {
      array[index] = el.toUpperCase();
    });
  } catch(error) {
    console.log(error);
  }
}

capAllElements('Incorrect argument');
```

### Approach B: External Error Handling

```javascript
function capAllElements(arr)
{
  arr.forEach((el, index, array) => {
    array[index] = el.toUpperCase();
  });
}

try {
  capAllElements('Incorrect argument');
} catch(error) {
  console.log(error);
}
```

At first glance, both approaches seem to handle errors. However, Approach B is significantly superior for several crucial reasons.

### Why External Error Handling Wins

#### 1. Maintains Single Responsibility Principle

Functions should have one clear purpose. In Approach B, `capAllElements` focuses solely on its core responsibility: capitalizing array elements. Error handling becomes the caller's responsibility, creating cleaner separation of concerns.

#### 2. Preserves Error Context

When errors are caught internally and logged (Approach A), crucial debugging information is lost. The original stack trace gets truncated, making it harder to understand where and why the error occurred. Approach B preserves the complete error context.

#### 3. Provides Caller Flexibility

Different parts of your application may need to handle the same error differently:

```javascript
// User-facing feature
try {
  capAllElements(userInput);
} catch(error) {
  showUserFriendlyMessage("Please provide a valid list of items");
}

// Background processing
try {
  capAllElements(batchData);
} catch(error) {
  logToMonitoringService(error);
  scheduleRetry(batchData);
}

// Development/testing
try {
  capAllElements(testData);
} catch(error) {
  console.error("Test failed:", error.message);
  throw error; // Re-throw for test framework
}
```

#### 4. Fails Fast and Loud

Silent failures (like in Approach A) are debugging nightmares. When something goes wrong, you want to know about it immediately. Approach B ensures errors aren't accidentally ignored.

### Best Practices for Try/Catch Placement

#### 1. Catch at the Right Level

Place try/catch blocks where you can meaningfully respond to errors:

```javascript
// Good: Catch where you can take meaningful action
async function saveUserProfile(userData)
{
  try {
    const validatedData = validateUserData(userData);
    const result = await database.save(validatedData);
    showSuccessMessage("Profile saved!");
    return result;
  } catch(ValidationError) {
    showValidationErrors(error.details);
  } catch(DatabaseError) {
    showErrorMessage("Unable to save. Please try again.");
    logError(error);
  }
}
```

#### 2. Don't Catch and Ignore

```javascript
// Bad: Swallowing errors
try {
  riskyOperation();
} catch(error) {
  // Silent failure - bugs will be hard to find
}

// Good: Log and re-throw or handle appropriately
try {
  riskyOperation();
} catch(error) {
  logger.error('Risky operation failed', { error, context });
  throw error; // Or handle appropriately for your use case
}
```

#### 3. Use Specific Error Types

When you do throw errors from your functions, make them descriptive:

```javascript
function capAllElements(arr)
{
  if (!Array.isArray(arr)) {
    throw new TypeError(`Expected array, received ${typeof arr}`);
  }
  
  if (!arr.every(el => typeof el === 'string')) {
    throw new TypeError('All array elements must be strings');
  }
  
  return arr.map(el => el.toUpperCase());
}
```

### When to Use Internal Try/Catch

There are legitimate cases for internal error handling:

#### 1. Resource Cleanup
```javascript
function processFile(filename)
{
  let file;

  try {
    file = openFile(filename);
    return processData(file.read());
  } catch(error) {
    logger.error(`Failed to process file ${filename}`, error);
    throw error; // Re-throw after logging
  } finally {
    if (file) file.close();
  }
}
```

#### 2. Fallback Strategies
```javascript
function getConfiguration()
{
  try {
    return loadFromNetwork();
  } catch(networkError) {
    logger.warn('Network config failed, using local fallback');
    return loadFromLocalStorage();
  }
}
```

#### 3. Converting Error Types
```javascript
function apiCall(data)
{
  try {
    return httpClient.post('/api/data', data);
  } catch(httpError) {
    // Convert technical HTTP error to domain-specific error
    throw new BusinessError(`Operation failed: ${httpError.message}`);
  }
}
```

### Fundmental Principles

1. **Let errors bubble up** unless you can meaningfully handle them
2. **Catch errors where you can take appropriate action**
3. **Never silently swallow errors** without good reason
4. **Preserve error context** for debugging
5. **Make error messages actionable** for developers and users
6. **Use specific error types** to help callers handle different scenarios

### Overall Conclusion

Effective error handling isn't about catching every possible error—it's about catching the right errors at the right place. By following these principles, you'll write more maintainable, debuggable, and robust JavaScript applications.

Remember: good error handling is like a good safety net. It should be there when you need it, invisible when you don't, and should never interfere with the main performance.

-------------------------------------

## Working with Asynchronous Functions

### Approach A: try/catch **inside** the async function

This is the most common and recommended approach:

```js
async function fetchData() {
  try {
    let res = await fetch("https://api.example.com/data");
    let data = await res.json();
    return data;
  } catch (err) {
    console.error("Error inside fetchData:", err);
    throw err; // rethrow if you want the caller to handle it too
  }
}

(async () => {
  try {
    const result = await fetchData();
    console.log(result);
  } catch (err) {
    console.error("Handled in caller:", err);
  }
})();
```

**Advantages:**
- You can handle errors *close to where they occur*.
- You can add fallback logic, retries, or logging locally.
- Keeps responsibilities clear: the function decides what to do with its own errors.

### Approach B: try/catch **outside** the async function

If you don’t use try/catch inside, you can still catch the error where the function is awaited:

```js
async function fetchData() {
  let res = await fetch("https://api.example.com/data");
  return res.json(); // if this fails, it rejects
}

(async () => {
  try {
    const result = await fetchData();
    console.log(result);
  } catch (err) {
    console.error("Handled in caller:", err);
  }
})();
```

**Advantages:**
- Keeps the async function “clean” and lets the caller decide how to handle errors.
- Useful when you want to **propagate errors upward** (like in a service layer).

### Overall Conclusion & Summmary

With **async functions**, `try/catch` is most useful **inside the function**, because async functions return a `Promise`, and exceptions inside them will reject the `Promise`.

- **Put `try/catch` inside an async function** when the function itself can recover, retry, or needs to log errors.
- **Leave `try/catch` outside** if you want to propagate errors to the caller and let them decide how to handle it.
- You can mix both: handle recoverable errors inside, rethrow critical ones to be caught outside.

---

## Asynchronous Function Error Handling (Decision Tree)

Now let's continue expending on Example 2.
Asking 3 questions using a *decision tree**, let's follow when deciding where to put `try/catch` in async functions:

### Question 1: Can the function itself handle/recover from the error
Put `try/catch` **inside** the function.
For retry, fallback, default value, logging, cleanup, etc.

  ```js
  async function fetchData() {
    try {
      const res = await fetch("https://api.example.com/data");
      return await res.json();
    } catch (err) {
      console.error("fetchData failed, using fallback.");
      return []; // fallback result
    }
  }
  ```

### Question 2: Should the caller decide how to handle the error
Let the error bubble up, and use `try/catch` **outside**.
WIthin in service layers, controllers, or top-level handlers.

  ```js
  async function fetchData() {
    const res = await fetch("https://api.example.com/data");
    return res.json(); // throws if invalid
  }

  (async () => {
    try {
      const data = await fetchData();
      console.log(data);
    } catch (err) {
      console.error("Caller handles error:", err);
    }
  })();
  ```

### Quation 3: Wanting both local handling *and* caller awareness
Use `try/catch` **inside**, then `throw` again.
To log or partially handle, but still let the caller know.

  ```js
  async function fetchData() {
    try {
      const res = await fetch("https://api.example.com/data");
      return await res.json();
    } catch (err) {
      console.error("Error in fetchData:", err);
      throw err; // rethrow to caller
    }
  }
  ```

### Overall Conclusion & Summary

* **Inside**: if the function can **recover, log, or wrap the error**.
* **Outside**: if the caller should **control the error policy**.
* **Both**: if **logging + caller awareness** is needed.
