I have two branches: master and dev

I want to create a "feature branch" from the dev branch.

1) $ git checkout -b myFeature dev
2) $ git commit -am "Your message"

# Logging Guidelines

To improve log clarity and reduce unnecessary clutter in production, follow these logging best practices:

## 1. Logging Non-500 Errors
- **Issue:** Logging `ERROR` for all non-500 HTTP responses, even when they are expected failures (e.g., validation errors, unauthorized access).
- **Solution:**
  - Use `WARN` or `INFO` if visibility is needed.
  - Use `DEBUG` for troubleshooting purposes.

**Example:**
```java
// Before (incorrect)
logger.error("Invalid request: {}", requestId);

// After (correct)
logger.warn("Invalid request: {}", requestId);  // Expected issue, not an error
```

---

## 2. Logging Non-Critical Events
- **Issue:** Logging `INFO` for system events that are not controller entry/exit points, leading to log clutter.
- **Solution:**
  - Use `DEBUG` to prevent excessive logs in production.
  - Only log at `INFO` if critical for system monitoring.

**Example:**
```java
// Before (incorrect)
logger.info("Cache refreshed for key: {}", cacheKey);

// After (correct)
logger.debug("Cache refreshed for key: {}", cacheKey);  // Debug-level detail
```

---

## 3. Logging Controller Entry/Exit Points
- **Issue:** Logging `INFO` when entering/exiting controllers adds noise to logs.
- **Solution:**
  - Avoid logging controller entry/exit unless explicitly required.
  - Use `DEBUG` if necessary.

**Example:**
```java
// Before (incorrect)
logger.info("Entering getUserProfile()");

// After (correct)
logger.debug("Entering getUserProfile()");  // Debug is sufficient
```

---

## 4. Logging Exceptions Without Stack Traces
- **Issue:** Logging full stack traces for exceptions makes it harder to locate the actual issue.
- **Solution:**
  - Log only the exception message at `ERROR` level.
  - Include stack traces at `DEBUG` level if needed.

**Example:**
```java
// Before (incorrect)
try {
    processPayment();
} catch (PaymentException e) {
    logger.error("Payment failed", e);  // Full stack trace (too much info)
}

// After (correct)
try {
    processPayment();
} catch (PaymentException e) {
    logger.error("Payment failed: {}", e.getMessage());  // Logs only message
    logger.debug("Payment failure details", e);  // Stack trace only in debug mode
}
```

---

## Summary of Changes

| Scenario | Old Logging Level | New Logging Level |
|----------|------------------|------------------|
| Non-500 errors | `ERROR` | `WARN` or `INFO`, `DEBUG` if troubleshooting |
| System events (not controller entry/exit) | `INFO` | `DEBUG` |
| Controller entry/exit points | `INFO` | Avoid logging, or use `DEBUG` |
| Exceptions | Full stack trace (`ERROR`) | Message only (`ERROR`), full trace in `DEBUG` |

---

Following these guidelines ensures logs remain meaningful and easy to analyze in production. 🚀

