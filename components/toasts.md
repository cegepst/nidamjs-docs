---
icon: bell
---

# Toasts

NidamJS provides a flexible and easy-to-use Toast notification system. It allows you to display temporary messages to the user, with customizable positions, durations, and severity levels.

---

## Quick Start

To use toasts, you **must** include the toast stack container in your HTML. Without this, NidamJS will log a warning and won't know where to render the toasts.

```html
<!-- Usually placed near the end of the <body> -->
<div nd-toast-stack data-position="bottom-right"></div>
```

You can then import and call the toast functions:

```javascript
import { showToast } from "nidamjs"; // Use your actual import path

// A simple informative toast
showToast("info", "Your profile has been updated.");
```

---

## Core Attributes

The toast system relies on a few data attributes to manage the display and positioning of notifications.

### 1. The Stack Container (`nd-toast-stack`)

Applied to an empty `<div>`, this acts as the anchor for all incoming notifications.

### 2. The Position (`data-position`)

Defines the default position for all toasts within the stack.

Allowed values are:

- `top-right`
- `top-left`
- `bottom-right`
- `bottom-left`

---

## API Reference

The core functionality is exposed via JavaScript exports.

### showToast(type, messages, options)

The core function used to display a toast notification.

**Parameters:**

| Parameter  | Type                            | Required | Description                                                                                                 |
| ---------- | ------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| `type`     | `String`                        | Yes      | The severity of the toast (`"info"`, `"success"`, `"error"`, `"warning"`). Defaults to `"info"` if invalid. |
| `messages` | `String` \| `Array` \| `Object` | Yes      | The message(s) to display. Intelligently parses multiple structures.                                        |
| `options`  | `Object`                        | No       | Customization options for this specific toast.                                                              |

**Message Structures:**

- If a **String** is provided, a standard text toast is displayed.
- If an **Array of strings** is provided, it will render them as a bulleted list (`<ul>`).
- If an **Object** is provided, it automatically extracts message values (or an `.errors` property array if one is present).

**Options (`options` object):**

| Option     | Type      | Default | Description                                                                         |
| ---------- | --------- | ------- | ----------------------------------------------------------------------------------- |
| `duration` | `Number`  | `3000`  | Auto-dismiss timeout in milliseconds. Set to `0` or less to disable auto-dismissal. |
| `closable` | `Boolean` | `true`  | Whether to render the close (`x`) button.                                           |
| `position` | `String`  | (Stack) | Override the stack position for this specific toast (e.g., `"top-left"`).           |

**Examples:**

```javascript
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

---

## Technical Specifications

### Attribute Reference

| Attribute        | Level      | Required | Description                                                         |
| ---------------- | ---------- | -------- | ------------------------------------------------------------------- |
| `nd-toast-stack` | Container  | Yes      | Identifies the main toast stacking element.                         |
| `data-position`  | Container  | No       | The default corner to stack toasts.                                 |
| `data-type`      | Toast Item | No       | Evaluated visually (`"info"`, `"success"`, `"warning"`, `"error"`). |

---

## Styling & Customization

Toasts use the `.nd-toast` class, with the `data-type` attribute representing their visual states. You can override styles using regular CSS.

### Technical Classes Reference

| Class / Selector                 | Description                                                     |
| -------------------------------- | --------------------------------------------------------------- |
| `[nd-toast-stack]`               | The main stack container.                                       |
| `.nd-toast`                      | The individual toast styling wrapper.                           |
| `.nd-toast[data-type="success"]` | State-based styling variations (success, error, warning, info). |
| `.nd-toast-list`                 | Unordered list styling for multiple messages.                   |
| `.nd-toast-close`                | The 'x' close button.                                           |

**Example CSS Overrides:**

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
```
