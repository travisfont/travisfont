# Functions (JavaScript)

##  Arrow Function / High-Order Function

Using parentheses around the arrow function body makes it more readable and clear.

**This is much better:**
```javascript
products.find((product) => (product.name === "Blue Notebook"));
```

**Other Alternatives:**

1. **With explicit return and curly braces** (good for complex logic):
```javascript
products.find((product) => {
    return product.name === "Blue Notebook";
});
```

1. **Without extra parentheses** (less readable):
```javascript
products.find(product => product.name === "Blue Notebook");
```

The version with parentheses strikes a nice balance of cleanliness but makes the return value explicit.

_The key point is that **all of these work correctly** and choosing the most readable style is a matter of preference and team conventions._
