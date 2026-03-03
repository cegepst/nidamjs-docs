---
icon: bell
---

# NidamJS Toasts Documentation

NidamJS provides a flexible and easy-to-use Toast notification system. It allows you to display temporary messages to the user, with customizable positions, durations, and severity levels.

## Prerequisites

To use toasts, you **must** include the toast stack container in your HTML. Without this, NidamJS will log a warning and won't know where to render the toasts.

```html
<!-- Usually placed near the end of the <body> -->
<div nd-toast-stack data-position="bottom-right"></div>
```

The `data-position` attribute defines the default position for all toasts. Allowed values are:

- `top-right`
- `top-left`
- `bottom-right`
- `bottom-left`

## Basic Usage

You can import the toast functions from the NidamJS index:

```javascript
import { showToast, toastNotify, createToastNotify } from "nidamjs"; // Use your actual import path
```

### [showToast(type, messages, options)](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/utils/toast.js#188-221)

The core function used to display a toast notification.

**Parameters:**

1. `type` _(String)_: The severity of the toast. Must be one of `"info"`, `"success"`, `"error"`, or `"warning"`. (Defaults to `"info"` if invalid).
2. `messages` _(String | Array | Object)_: The message(s) to display. It intelligently parses multiple structures:
   - If a **String** is provided, a standard text toast is displayed.
   - If an **Array of strings** is provided, it will render them as a bulleted list (`<ul>`).
   - If an **Object** is provided, it automatically extracts message values (or an `.errors` property array if one is present).
3. `options` _(Object, optional)_: Customization options for this specific toast.
   - `duration` _(Number)_: Auto-dismiss timeout in milliseconds. Default is `3000`. Set to `0` or less to disable auto-dismissal.
   - `closable` _(Boolean)_: Whether to render the close (`x`) button. Default is `true`.
   - `position` _(String)_: Override the stack position for this specific toast (e.g., `"top-left"`).

**Examples:**

```javascript
// A simple informative toast
showToast("info", "Your profile has been updated.");

// A success toast with a custom duration
showToast("success", "Operation successful!", { duration: 5000 });

// An error toast that stays until the user manually closes it
showToast("error", "Failed to connect to the server.", {
  duration: 0,
  closable: true,
});

// Displaying multiple errors, rendering beautifully as an unordered list
showToast("warning", ["Password is too short", "Email is invalid"]);
```

### `toastNotify(level, message)`

A simplified helper function that maps common log levels to their appropriate toast types and uses the default options.

**Parameters:**

- `level` _(String)_: Can be `"error"`, `"warn"`/`"warning"`, or `"success"`. Any other value will default to `"info"`.
- `message`: The message payload to display.

**Example:**

```javascript
toastNotify("success", "Item added to cart.");
toastNotify("warn", "Storage is almost full.");
```

### [createToastNotify(defaultOptions)](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/utils/toast.js#233-244)

Creates a new notification utility function (similar to `toastNotify`) configured with your own predefined default options.

**Parameters:**

- `defaultOptions` _(Object)_: A configuration parameter containing default overrides for `duration`, `closable`, and `position`.

**Example:**

```javascript
// Create a custom notifier that always puts toasts on the top-left and disables auto-close entirely
const myNotifier = createToastNotify({
  position: "top-left",
  duration: 0,
  closable: true,
});

// Now you can easily reuse these settings anywhere!
myNotifier("error", "Critical system failure!");
myNotifier("info", "System is down for maintenance.");
```

## Styling and Customization

Toasts use the `.nd-toast` class, with the `data-type` attribute representing their visual states. You can override styles using regular CSS:

```css
/* Styling the main stack container */
[nd-toast-stack] {
  /* position, z-index, flexbox gap... */
}

/* Individual toast styling */
.nd-toast {
  /* padding, background, border-radius, box-shadow... */
}

/* Severity based colors */
.nd-toast[data-type="success"] { ... }
.nd-toast[data-type="error"] { ... }
.nd-toast[data-type="warning"] { ... }
.nd-toast[data-type="info"] { ... }

/* Unordered list styling for multiple messages */
.nd-toast-list { ... }

/* The 'x' close button */
.nd-toast-close { ... }
```
