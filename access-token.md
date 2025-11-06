### Access Token Check

Before performing any authentication checks:

* **Scenario:** The user may not have an access token in their browser (e.g., logged out or using a different browser).
* **Action:** If `localStorage` does not contain an access token, **do not attempt to check authentication**.
* **Implementation Example:**

```javascript
const accessToken = localStorage.getItem("accessToken");
if (!accessToken) {
  // User is not authenticated, stop further checks
  return;
}
```

> ⚠️ Important: Only proceed with authentication checks if a valid access token exists.
> 
