---
icon: light-bulb
order: 10
---

# Core Concepts

NidamJS is a modular system that overlays a desktop environment onto standard web pages. It uses event delegation and runtime injection to manage windows and taskbars without a heavy framework lock-in.

## Architecture

NidamJS consists of a central **NidamApp** instance that orchestrates several **Managers**.

### NidamApp Instance

The "brain" of the application. It:
- Initializes the **Event Delegator** for global click/drag handling.
- Spawns and stores manager modules (`window`, `taskbar`, `refresher`).
- Runs the **Content Initializer** to wire up features in newly fetched HTML.

### Modules (Managers)

1.  **WindowManager**: The heart of the library. It handles the lifecycle (fetch, open, close) and physics (dragging, snapping, tiling) of windows.
2.  **TaskbarManager**: Syncs with the WindowManager to show which windows are open and provides visual feedback via the `is-open` class.
3.  **WindowRefresher**: Listens for server-emitted events to automatically refresh the content of specific windows or close windows whose underlying data has been deleted.
4.  **DesktopIconManager**: (Optional) Manages the grid layout and persistence of shortcuts on the background layer.

## Lifecycle & DOM Contract

Unlike component-based frameworks, NidamJS relies on a **DOM Contract**. 

When you call `windowManager.open('profile')`, the library:
1.  Fetches the HTML from the endpoint.
2.  Parses the HTML looking for an element with the `.window` class.
3.  Appends that element to the `modalContainer`.
4.  Initializes all behaviors (dragging, closing) based on `data-` attributes.

This allows you to serve window content from any backend (PHP, Python, Node) or static HTML templates.

## Event System

NidamJS uses standard DOM events for communication.

### Global Events

- `window:opened`: Fired when a window is added to the DOM and initialized.
- `window:closed`: Fired when a window is removed from the DOM.

### Internal Delegation

The `EventDelegator` listens for clicks on the `document`. When it sees an element with `data-modal="xyz"`, it automatically tells the `WindowManager` to open that endpoint. You don't need to write `onclick` handlers for most basic triggers.

## Z-Index Management

Windows are stacked dynamically. Whenever a window is clicked or opened, its `z-index` is incremented to be higher than the current maximum, ensuring it appears on top. The base z-index can be configured via `zIndexBase`.
