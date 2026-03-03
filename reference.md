---
icon: book
---

# API Reference

NidamJS exposes a carefully curated set of public functions and classes designed for interacting with its windowing system, desktop icons, and event delegation.

---

## App Initialization

### `initNidamApp(config)`

Initializes the `NidamApp` with an optional configuration object and returns the fully initialized app instance.

> **Note:** NidamJS will automatically call `initNidamApp()` on script load unless you add a script tag with the `[data-nd-init]` attribute to your document to signify manual initialization.

**Parameters:**

- `config` _(Object | string)_ (Optional): The NidamJS configuration object or JSON string.

**Returns:**

- [`NidamApp`](#nidamapp): The initialized application instance.

**Example:**

```javascript
import { initNidamApp } from "nidamjs";

const app = initNidamApp({
  windowManager: { layoutStabilizationMs: 450 },
});
```

---

## `NidamApp`

The main instance returned by initialization. It coordinates all modules in the NidamJS ecosystem.

### `getModule(name)`

Retrieves a specific initialized module by its registered name. This is the primary way to programmatically interact with NidamJS at runtime.

**Parameters:**

- `name` _(string)_: The name of the module. Valid module names are `"window"`, `"delegator"`, `"taskbar"`, `"refresher"`, and `"icon"`.

**Returns:**

- The requested module instance, or `undefined` if not found.

**Example:**

```javascript
const app = initNidamApp();

// Access the window manager
const windowManager = app.getModule("window");
windowManager.open("my-settings-modal");
```

### `getModules()`

Retrieves all initialized modules.

**Returns:**

- `Map<string, object>`: A JavaScript `Map` containing all available module instances.

---

## Modules

After initializing NidamJS, you can access the core modules to manually trigger actions like opening windows, refreshing content, or listening to global events.

### 1. Window Manager (`"window"`)

Controls the lifecycle, focus, and state of windows/modals. Accessible via `app.getModule("window")`.

- **`open(endpoint, force = false, focusSelector = null, activate = true)`**
  Opens a new window based on its endpoint or focuses an existing one. Returns a `Promise` tracking the DOM injection and initialization.
- **`close(winElement)`**
  Closes the specified window element. Triggered automatically by GUI buttons.
- **`closeTopmost()`**
  Closes the active (topmost/focused) window. Often mapped internally to the "Escape" key behavior.
- **`getWindows()`**
  Returns an array of `[endpoint, winElement]` pairs representing all currently open windows.
- **`focus(winElement)`**
  Brings the specified window to the front by updating its `z-index` position.
- **`toggleMaximize(winElement)`**
  Toggles the given window between its maximized state and its previous restored bounds.
- **`drag(e, winElement)`**
  Manually triggers a window drag operation, consuming a `MouseEvent`.

### 2. Event Delegator (`"delegator"`)

Handles centralized root-level HTML event delegation (like `click`, `mousedown`, `keydown`).

- **`on(eventType, selector, handler, options = { group })`**
  Registers a delegated event handler on the app's root. If `selector` is `null`, it works as a global handler. Returns a handler reference object.
- **`off(eventType, ref)`**
  Removes a previously registered event handler using the reference object returned by `.on()`.

### 3. Window Refresher (`"refresher"`)

Reacts to generic or server-sent events to intelligently refresh window content or close windows.

- **`setRefreshMap(refreshMap)`**
  Dynamically updates the app's rule map that determines which window paths refresh upon receiving specific events.
- **`handleEvent(eventName, payload)`**
  Processes an incoming event (e.g., `user:deleted`) and triggers the appropriate window refreshes/closures. Expects the payload to provide an `id` to determine destruction chains.

### 4. Taskbar Manager (`"taskbar"`)

Synchronizes the HTML taskbar layer functionality and icon rendering with the currently open modal instances. Interaction is primarily automatic across `$window:opened` and `$window:closed` events.

### 5. Icon Manager (`"icon"`)

Controls desktop icon grid positioning constraints, snapping, and layout memory functionality on the DOM `[nd-icons]` container.
