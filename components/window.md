---
icon: browser
---

# Window

The `Window` component is the primary container for application content within the NidamJS desktop environment. It
mimics the behavior of a native operating system window, supporting dragging, resizing, minimizing, maximizing, and
closing.

## Overview

A window consists of a title bar (with controls) and a content body. It floats above the desktop background and other
windows, managed by a z-index stacking context.

### Features

- **Draggable**: Move the window around the desktop area by its title bar.
- **Resizable**: Windows can be resized or snapped to screen edges.
- **Stateful**: Supports maximized, tiled, and focused states.
- **Focus Management**: Clicking a window brings it to the front and applies the `.focused` class.
- **Snapping**: Windows snap to screen edges (top, left, right) for easy multitasking.

## Usage

### Basic Structure

Windows are typically served as HTML strings (e.g., from an endpoint or a `<template>`). They must contain the `.window` class and a draggable bar with the `data-bar` attribute.

```html
<div class="window window-shell animate-appearance">
  <div class="window-bar" data-bar>
    <strong class="window-title">My Window</strong>
    <div class="window-actions">
      <button class="mini-btn" data-maximize title="Maximize">
        <i class="fa fa-expand"></i>
      </button>
      <button class="mini-btn" data-close title="Close">
        <i class="fa fa-times"></i>
      </button>
    </div>
  </div>
  <div class="window-content-scrollable">
    <div class="window-content">
      <!-- Your Content Here -->
    </div>
  </div>
</div>
```

### Opening a Window

Use the `windowManager.open()` method or the `data-modal` attribute on a trigger element.

```javascript
// Via API
app.getModule("window").open("path/to/content.html");

// Via HTML
<button data-modal="path/to/content.html">Open Window</button>;
```

## Structure & Styling

NidamJS uses the following classes and attributes for window management.

| Selector              | Description                                  |
| :-------------------- | :------------------------------------------- |
| `.window`             | The root container for a window.             |
| `[data-bar]`          | The draggable area (usually the title bar).  |
| `[data-maximize]`     | Trigger for maximizing/restoring the window. |
| `[data-close]`        | Trigger for closing the window.              |
| `.focused`            | Added to the active window.                  |
| `.maximized`          | Added when the window fills the screen.      |
| `.animate-appearance` | Class for entry animations.                  |

### CSS Animations

You can customize how windows appear and disappear:

```css
@keyframes window-appear {
  from {
    opacity: 0;
    transform: scale(0.97);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.window.animate-appearance {
  animation: window-appear 180ms ease-out forwards;
}
```

## API Reference

### Configuration

When initializing `createNidamApp`, configure window behavior under `windowManager.config`:

```javascript
const app = createNidamApp({
  windowManager: {
    config: {
      maxWindows: 1,
      cooldownMs: 300,
      zIndexBase: 1000,
      taskbarHeight: 80,
    },
  },
});
```

With `maxWindows: 1`, opening a second window will show the default toast error notification.

### Window Properties (via HTML)

| Attribute           | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| `data-default-snap` | Set to `maximize`, `left`, or `right` for initial placement. |
| `data-endpoint`     | (Internal) The unique identifier for the window.             |

### Events

| Event Name      | Description                                  |
| :-------------- | :------------------------------------------- |
| `window:opened` | Triggered when a window is added to the DOM. |
| `window:closed` | Triggered when a window starts closing.      |
