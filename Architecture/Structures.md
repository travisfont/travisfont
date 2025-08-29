# Structure (struct)

| Feature                 | Struct with methods                                                                         | Object (Class instance)                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Semantics**           | Value type → copied by value (unless reference explicitly used). Each copy is independent.  | Reference type → variables point to the same underlying object.                                            |
| **Purpose**             | Simple data grouping                  | Complex entities with data + behavior  |
| **Inheritance**         | Typically not supported (can’t subclass).                                                   | Fully supported (inheritance, polymorphism, abstract classes, etc.).                                       |
| **Interfaces / Traits** | Can often *implement* interfaces/traits (e.g. C#, Go, Rust).                                | Can *implement and inherit* classes and interfaces.                                                        |
| **Identity**            | Doesn’t usually have “identity” — if two structs have the same field values, they’re equal. | Objects have identity — two objects with the same data are still different if they’re different instances. |
| **Memory**              | Often stack-allocated (though can be boxed on heap if needed).                              | Usually heap-allocated with references.                                                                    |
| **Use case**            | Lightweight, immutable, small data types with simple methods.                               | Complex, stateful entities with lifecycle and behavior.                                                    |
| **Methods**             | Usually none (except in modern langs) | Always can have methods                             | Always can have methods                                                                                    |

**Even with methods, structs are still usually:**
- **Value types** without inheritance.
- Better for small, immutable data structures.
- Different from **objects**, which emphasize identity, reference semantics, and OOP features like polymorphism.
- Note; In C++, structs and classes are almost the same—the only difference is the default visibility (struct members are public, class members are private).
