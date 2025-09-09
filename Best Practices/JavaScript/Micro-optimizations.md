# Micro-Optimizations

## Node.js: process.stdout vs. console.log

```javascript
// Faster for many writes
for (let i = 0; i < 1000; i++) {
  process.stdout.write(`Line ${i}\n`);
}

// Slightly slower
for (let i = 0; i < 1000; i++) {
  console.log(`Line ${i}`);
}
```

| Aspect                 | process.stdout                         |               console.log() |
|------------------------|----------------------------------------|-----------------------------|
| **Newlines**           | Doesn't add automatically (need `\n`)  | Adds newline automatically  |
| **Multiple arguments** | Doesn't support (concatenate manually) | Supports multiple arguments |
| **Formatting**         | No automatic formatting                | Supports string formatting (`%s`, `%d`, etc.) |
| **Return value**       | Returns boolean (stream success)       | Returns undefined                             |
| **Performance**        | Slightly faster (lower overhead)       | Slightly slower (more features)               |
| **Error handling**     | Can handle backpressure                | Simplified error handling                     |

### Use process.stdout
- Fine-grained control over output
- To handle backpressure in streams
- Maximum performance for high-volume output
- To avoid automatic newlines

### Use console.log()
- Convenience and readability
- Automatic formatting
- Multiple arguments support
- Debugging output (includes timestamps in some environments)
