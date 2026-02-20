---
icon: code
order: -10
---

# API Reference

This section provides technical details for the NidamJS public API classes, methods, and properties.

## Core

### `createNidamApp(config)`

The primary factory function used to initialize the NidamJS environment.

**Parameters**

| Name | Type | Description |
| :--- | :--- | :--- |
| `config` | `Object` | Configuration options for the app. |

**Config Object**

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `modalContainer` | `string` | `"#target"` | CSS selector for the element that will contain windows. |
| `registry` | `Array` | `[]` | List of feature definitions for automatic initialization. |
| `windowManager` | `Object` | `{}` | Configuration and adapters for the Window Manager. |
| `refreshMap` | `Object` | `null` | Map for automatic window refreshing. |

**Returns**

- `NidamApp`: An instance of the application.

---

## NidamApp Instance

The `NidamApp` instance manages the lifecycle of different modules.

### Methods

#### `initialize()`
Starts event delegation and initializes all managers. Must be called once.

#### `getModule(name)`
Retrieves a specific manager module.
- `name`: `'window'`, `'taskbar'`, `'refresher'`, or `'delegator'`.

#### `getModules()`
Returns the internal `Map` containing all active modules.

---

## WindowManager

Accessible via `app.getModule('window')`. Manages window lifecycle and interaction.

### Methods

#### `open(endpoint, force = false)`
Opens a window for a given endpoint.
- `endpoint` (string): The URL or template route to load.
- `force` (boolean): If true, bypasses cooldowns and creates a new instance even if one exists.
- **Returns**: `Promise<HTMLElement>`

#### `close(winElement)`
Closes a specific window element with animation.
- `winElement` (HTMLElement): The `.window` element to close.

#### `focus(winElement)`
Brings a window to the foreground.

#### `toggleMaximize(winElement)`
Toggles between maximized and restored states.

---

## Window DOM Contract

NidamJS windows are plain `HTMLElement`s that must follow a specific internal structure.

### Attributes

| Attribute | Description |
| :--- | :--- |
| `class="window"` | Identifies the root window element. |
| `[data-bar]` | The draggable handle (usually the title bar). |
| `[data-close]` | Trigger for closing the window. |
| `[data-maximize]` | Trigger for maximizing the window. |
| `data-endpoint` | (Internal) The unique ID assigned to the window. |

---

## TaskbarManager

Accessible via `app.getModule('taskbar')`.

The `TaskbarManager` primarily operates through event listeners and DOM attributes (`nd-taskbar`, `tb-icon`). It does not currently expose a public method API for dynamic item management.

---

## DesktopIconManager

If initialized manually or via registry:

The `DesktopIconManager` handles drag-and-drop persistence for desktop icons. It targets elements with the `.desktop-icon` class.

### Configuration

| Option | Type | Description |
| :--- | :--- | :--- |
| `storageKey` | `string` | Key used in `localStorage`. |
| `storageNamespace` | `string` | Optional prefix for the storage key. |
