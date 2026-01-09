# The Confusing World of Array Deletion in JavaScript
> December 27, 2025 @ [dev.to/travisfont](https://dev.to/travisfont/the-confusing-world-of-array-deletion-in-javascript-2gn2)

One of JavaScript's most controversial, yet quietly discussed subjects which goes beyond implicit type coercion (e.g., [] + {}) is handling arrays natively, specifically for programmers of other languages learning with the goal of actually understanding JavaScript. While it's one of the most popular programming languages in the world, its design choices around array manipulation have frustrated programmers for decades, including newcomers. This is especially clear when trying to do something simple, such as removing an item from an array.

Let's get into the different approaches!
For the entirety of this article, consider a simple basket of fruit:

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape', 'mango'];
```

Now, imagine wanting to remove 'orange' from this basket. JavaScript offers several ways to do this, each with its own quirks, return values, and gotchas.

## Approach 1: Using the Splice Function

The most common way to remove an element is using **splice()**:

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape', 'mango'];
const index  = fruits.indexOf('orange'); // returns 2

if (index > -1)
{
  const removed = fruits.splice(index, 1);
  console.log(removed);  // returns an array ['orange']
  console.log(fruits);   // ['apple', 'banana', 'grape', 'mango']
}
```

And here's where things get interesting. The splice() function doesn't just remove the element, it also mutates the original array and returns an array of the removed element(s). Not just the element itself, but an array containing it. This is where the second parameter (set to 1 in this example) comes into play: if you increase it (e.g., to 2 or 3), those array elements will also be returned, such as 'grape' and/or 'mango'.

This return value makes sense when removing multiple items:

```javascript
const fruits  = ['apple', 'banana', 'orange', 'grape', 'mango'];
const removed = fruits.splice(1, 3); // remove 3 items starting at index 1

console.log(removed);  // ['banana', 'orange', 'grape']
console.log(fruits);   // ['apple', 'mango']
```

But for a single element, getting back a one-item array feels unnecessarily wrapped.

### The splice() vs. slice() Naming Confusion

Perhaps the most infamous naming confusion in JavaScript, particularly within arrays, is between **splice()** and **slice()**. They differ by just one letter, however do completely opposite things and describe very little. Let's take a look:

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape', 'mango'];

// slice sounds destructive but it isn't
const sliced = fruits.slice(1, 3);
console.log(sliced);  // new array: ['banana', 'orange']
console.log(fruits);  // unchanged array: ['apple', 'banana', 'orange', 'grape', 'mango']

// splice sounds like joining but does is destructive
const spliced = fruits.splice(1, 3);
console.log(spliced);  // new array: ['banana', 'orange', 'grape'] 
console.log(fruits);   // changed array: ['apple', 'mango']
```

This mockery here is painful. "Slice" sounds like cutting or removing, yet it leaves the original array intact, whereas "Splice" sounds like joining things together (like splicing rope or film). Yet, it actually removes elements and mutates the original array. Also, note that the returned arrays are different when both functions are called!

This single-letter difference has caused countless bugs and hours of debugging frustration, and it's considered one of the worst naming decisions in JavaScript's history.

### Removing Multiple Elements

When removing multiple elements with the same value using splice(), the approach changes significantly because splice() only removes elements at specific index positions and not by value. As an example here, we now have orange twice:

```javascript
const fruits = ['apple', 'orange', 'banana', 'orange', 'grape', 'orange'];

const index = fruits.indexOf('orange');
// indexOf() only finds the FIRST occurrence

fruits.splice(index, 1);  // ['apple', 'banana', 'orange', 'grape', 'orange']
console.log(fruits);      // only removed the first 'orange'
```

To remove multiple occurrences, the code needs to loop backwards through the array:

```javascript
const fruits = ['apple', 'orange', 'banana', 'orange', 'grape', 'orange'];

// loop backwards to avoid index shifting issues
for (let i = fruits.length - 1; i >= 0; i--)
{
  if (fruits[i] === 'orange')
  {
    fruits.splice(i, 1);
  }
}

console.log(fruits);  // ['apple', 'banana', 'grape']
```

Unfortunately, its readability is confusing, so the question arises:
Why loop backwards?

When removing elements while looping forward, the indices shift, and some elements get skipped. Let's see how this would look forward (which is wrong):

```javascript
const fruits = ['orange', 'orange', 'banana'];

for (let i = 0; i < fruits.length; i++) // normal forward iteration
{
  if (fruits[i] === 'orange')
   {
     fruits.splice(i, 1);
     // after removing index 0, the second 'orange' moves to index 0
     // but i becomes 1, so it gets skipped
  }
}

console.log(fruits);  // still has an orange: ['orange', 'banana']
```

This complexity is one reason why **filter()** is often the preferred native solution for removing multiple values, though it operates differently (more on that later).

## Method 2: The Delete Operator

This one is a trap, and isn't so clear at first glance! When try using the **delete** operator:

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape', 'mango'];
const index = fruits.indexOf('orange');

delete fruits[index];

console.log(fruits);        // ['apple', 'banana', empty, 'grape', 'mango']
console.log(fruits.length); // still contains 5 elements
console.log(fruits[2]);     // undefined
```

As noticed (or not), the delete operator doesn't actually remove the element, but rather. it sets that position to **_undefined_**, leaving a hole in the array. The length remains the same, but iteration becomes problematic, resulting in what's known as a "sparse array."

Using this method with arrays is best avoided. It lingers from JavaScript's object-oriented roots and often surprises programmers with its unexpected behavior.

## Method 3: The Filter Function

This alternative, easily promising and for a completely different approach, the **filter()** creates a new array without the unwanted element:

```javascript
const fruits   = ['apple', 'banana', 'orange', 'grape', 'mango'];
const filtered = fruits.filter(fruit => (fruit !== 'orange'));
                 // arrow function keeps it simple (low logic)

console.log(filtered);  // ['apple', 'banana', 'grape', 'mango']
console.log(fruits);    // ['apple', 'banana', 'orange', 'grape', 'mango'] 
```

As noticed, _fruits_ is unchanged!
This method follows an entirely different design. Instead of mutating the original array, it returns a brand new array. This immutable approach is safer and more predictable, but it comes with a performance cost for large arrays.

Another key difference is that filter() removes multiple occurrences of 'orange', not just the first one as previously seen with splice().


```javascript
const fruits   = ['apple', 'orange', 'banana', 'orange', 'grape'];
const filtered = fruits.filter(fruit => (fruit !== 'orange'));

console.log(filtered);  // ['apple', 'banana', 'grape']
```

And if one is wondering, how about a mutation design of this straightforward function? Well, JavaScript doesn't have a built-in mutable version of `filter()`. If mutation of the original array is needed, then here are some options.

**Mutable Option 1: Reassign after filtering**

```javascript
let fruits = ['apple', 'orange', 'banana', 'orange', 'grape'];
fruits     = fruits.filter(fruit => (fruit !== 'orange'));

console.log(fruits);  // ['apple', 'banana', 'grape']
```

This reassigns the variable but technically creates a new array rather than mutating the original, yet it's readable and straightforward.

**Mutable Option 2: Backwards loop with splice()**

```javascript
const fruits = ['apple', 'orange', 'banana', 'orange', 'grape'];

for (let i = fruits.length - 1; i >= 0; i--)
{
  if (fruits[i] === 'orange')
  {
    fruits.splice(i, 1);
  }
}

console.log(fruits);  // ['apple', 'banana', 'grape']
```

This is truly mutable, as it actually mutates the original array in place. But as previously pointed out, backwards iteration isn't readable at first glance.

**Mutable Option 3: Clear and refill**

```javascript
const fruits   = ['apple', 'orange', 'banana', 'orange', 'grape'];
const filtered = fruits.filter(fruit => (fruit !== 'orange'));

fruits.length = 0;         // clear the array
fruits.push(...filtered);  // refill it

console.log(fruits);  // ['apple', 'banana', 'grape']
```

This is mutable but clunky; its readability is fair, but it requires additional external steps and is unnecessary unless memory becomes the primary constraint.

## Additional Methods: The Pop & Shift Functions

Although these additional methods do not remove a specific element, they do, however, remove elements from the end or beginning of an array, which should be recognized:

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape', 'mango'];

const lastFruit = fruits.pop();
console.log(lastFruit);  // returns the removed  element 'mango'
console.log(fruits);     // ['apple', 'banana', 'orange', 'grape']

const firstFruit = fruits.shift();
console.log(firstFruit); // returns the removed  element  'apple'
console.log(fruits);     // ['banana', 'orange', 'grape']
```

Notice the inconsistency? Unlike splice(), these methods return the actual element, not an array containing it. Both methods mutate the original array, but their return values follow a different pattern.

The naming is also strange. While pop() makes intuitive sense (like popping something off a stack), shift() is less clear. The term "shift" suggests moving elements around, not removing the first one. Many programmers wish it had been called popFront() or removeFirst() instead, as seen in languages like C++ and Swift.

**And, so, as a final conclusion:**

The filter function always returns a new array, leaving the original unchanged. JavaScript doesn't have a native method that filters in place while mutating the original array, which is why the backwards loop with the splice function is the go-to solution when true mutation is required. 

- In most cases, the **filter()** is the best choice.
  - It's clear, predictable, removes all matching elements in one pass, and avoids the pitfalls of array mutation.
  - The immutable approach also prevents bugs in larger applications where unexpected mutations can cause problems.
- The backwards loop with **splice()** is the right method when working with very large arrays where performance matters.
  - Or when the original array reference must be preserved (for example, when other parts of the code hold references to it).
- And **delete**, never use it for arrays.

**#HappyCoding**

### References:
- MDN Docs - [Array.prototype.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
  - GeeksForGeeks - [JavaScript Array splice() Method](https://www.geeksforgeeks.org/javascript/javascript-array-splice-method/)
  - Codecademy - [JavaScript .splice()](https://www.codecademy.com/resources/docs/javascript/arrays/splice)
  - Mimo - [JavaScript Splice Method: Modifying Arrays](https://mimo.org/glossary/javascript/splice)
  - W3Schools - [JavaScript Array splice()](https://www.w3schools.com/jsref/jsref_splice.asp)
- MDN Docs - [Array.prototype.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
  - GeeksForGeeks - [JavaScript Array slice() Method](https://www.geeksforgeeks.org/javascript/javascript-array-slice-method/)
  - Mimo - [JavaScript Array slice() Method: Syntax, Usage, and Examples](https://mimo.org/glossary/javascript/array-slice)
  - W3Schools - [JavaScript Array slice()](https://www.w3schools.com/jsref/jsref_slice_array.asp)
- MDN Docs - [Array.prototype.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
  - GeeksForGeeks - [JavaScript Array filter() Method](https://www.geeksforgeeks.org/javascript/javascript-array-filter-method/)
  - Codecademy - [JavaScript .filter()](https://www.codecademy.com/resources/docs/javascript/arrays/filter)
  - W3Schools - [JavaScript Array filter()](https://www.w3schools.com/jsref/jsref_filter.asp)
- MDN Docs - [Array.prototype.indexOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
  - Codecademy - [JavaScript .indexOf()](https://www.codecademy.com/resources/docs/javascript/arrays/indexOf)
  - Mimo - [JavaScript Array indexOf: Syntax, Usage, and Examples](https://mimo.org/glossary/javascript/array-indexof-method)
- MDN Docs - [Expressions and operators: delete ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete)
- GeeksForGeeks - [What is the difference between Array.slice() and Array.splice() in JavaScript](https://www.geeksforgeeks.org/javascript/what-is-the-difference-between-array-slice-and-array-splice-in-javascript/)
- Mimo - [JavaScript Removing an Element: Syntax, Usage, and Examples](https://mimo.org/glossary/javascript/removing-an-element)
