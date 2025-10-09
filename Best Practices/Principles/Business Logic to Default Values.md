# business logic to default values

Default values should be simple, safe fallbacks and not in places where significant logic happens. This avoids hiding important decision-making or business rules inside default parameter values.

Asking the following question: Is this default making a business decision, or just providing a reasonable technical fallback?


## Why this matters

1. **Hidden decisions**: When defaults contain business logic, important decisions happen invisibly. Someone calling the function might not realize they're making a significant business choice by omitting an argument.
2. **Hard to change**: Business rules change frequently. When they're buried in default values across your codebase, they're difficult to find and update consistently.
3. **Testing challenges**: It's easy to miss testing scenarios when the business logic is implicit in defaults rather than explicit in your code flow.
4. **Maintainability**: Future developers (including yourself) may not realize that changing a default value is actually changing business behavior.

## When defaults are fine

Defaults work well for truly technical or cosmetic choices:
- `fetch_data(timeout=30)` - **technical setting**
- `format_report(page_size="A4")` - **display preference**
- `log_message(level="INFO")` - **operational detail**

## Practical Showcase

**Business logic in defaults (Bad):**
```cpp
// The default assumes customer is premium - that's a business decision!
double calculateDiscount(double amount, std::string customerType = "premium") {
    if (customerType == "premium") {
        return amount * 0.20;
    }
    return amount * 0.05;
}

// Hidden decision - caller might not realize they're defaulting to premium
double discount = calculateDiscount(100.0);
```

**Explicit business logic (Good):**
```cpp
double calculateDiscount(double amount, std::string customerType) {
    // Caller must explicitly decide customer type
    if (customerType == "premium") {
        return amount * 0.20;
    }
    return amount * 0.05;
}

// Business decision is explicit and visible
double discount = calculateDiscount(100.0, user.isPremium() ? "premium" : "regular");
```

