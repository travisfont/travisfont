# Prototypes

- A **prototype** is a **template object** that other objects can inherit from.
- In prototype-based languages (like **JavaScript**), objects don’t always come from “classes.” Instead, they inherit directly from other objects via the prototype chain.
- The prototype provides **shared properties and methods** to objects that inherit from it.
- Not a data type. A prototype is just an object.

**In conceptual terms**

- A prototype is an inheritance mechanism — a way for objects to share properties and methods.
- It’s more about how objects are linked together than about a “new kind” of thing.
= So conceptually, it’s not a data type like “object,” “string,” or “integer.”

* Example in JavaScript:

  ```js
  function Car(brand) {
    this.brand = brand;
  }

  Car.prototype.drive = function() {
    console.log(this.brand + " goes vroom!");
  };

  const myCar = new Car("Toyota");
  myCar.drive(); // "Toyota goes vroom!"

  console.log(typeof Car.prototype); // "object"
  ```

  * `Car.prototype` is a **prototype object**.
  * `myCar` is an **object** (an instance) that inherits from `Car.prototype`.

---

### **Key Differences from an object**

| **Aspect**        | **Object**                                                   | **Prototype**                                                       |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------------- |
| **Definition**    | A specific data instance with its own properties and values. | A “template” object used for inheritance.                           |
| **Existence**     | Exists at runtime as a concrete entity.                      | Exists as a link/blueprint to share methods/properties.             |
| **In JavaScript** | Any `{}` or instance created with `new`.                     | The object assigned to `Function.prototype` (used for inheritance). |
| **Analogy**       | A house you live in.                                         | The architectural blueprint used to build many houses.              |

---

## Well-known Prototype-based Languages

1. **Self**

   * The origin of prototype-based programming (developed at Xerox PARC in the late 1980s).
   * Everything is an object, and inheritance is done entirely through prototypes.
   * Inspired many later languages, including JavaScript.

2. **Io**

   * A small, minimal, highly dynamic prototype-based language.
   * Known for concurrency models and message-passing semantics.

3. **Lua**

   * Not strictly prototype-based, but uses **metatables** which can implement prototype-like inheritance.
   * Common in game scripting (e.g., Roblox, World of Warcraft mods).

4. **JavaScript**

   * The most widely used prototype-based language today.
   * Classes in modern JS (`class` keyword) are *syntactic sugar* over prototypes.

