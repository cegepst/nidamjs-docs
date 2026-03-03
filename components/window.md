---
icon: browser
---

# Windows

Windows are the core containers of the NidamJS desktop environment.  
They provide draggable, focusable, resizable, tileable, and dynamically loaded UI panels — fully managed by the internal `WindowManager`.

The system is declarative and attribute-driven, requiring little to no manual JavaScript wiring.

---

## Quick Start

A basic window structure looks like this:

```html
<div nd-window nd-window-endpoint="examples/shared/page-one.html">
  <div nd-window-header>
    <span>Page One</span>
    <button nd-window-button="maximize" title="Maximize">[ ]</button>
    <button nd-window-button="close" title="Close">X</button>
  </div>

  <div nd-window-content>
    <p>
      This window is served by
      <code>GET /examples/shared/page-one.html</code>.
    </p>

    <button data-modal="examples/shared/page-two.html">
      Open Page Two
    </button>
  </div>
</div>
````

To open it from anywhere:

```html
<button data-modal="examples/shared/page-one.html">
  Open Page One
</button>
```

The `WindowManager` automatically detects the `data-modal` attribute and loads the window content.

---

## Core Attributes

The window system is powered by declarative `nd-` attributes.

### 1. The Window Container (`nd-window`)

Applied to the root element of a window.

| Attribute         | Description                           |
| ----------------- | ------------------------------------- |
| `nd-window`       | Marks the element as a managed window |
| `nd-window="sm"`  | Small preset (480x320)                |
| `nd-window="lg"`  | Large preset (1024x640)               |
| `nd-window="xl"`  | Extra large preset (1280x800)         |
| `nd-window="2xl"` | Ultra preset (1440x900)               |

If no size is specified, the default is **768x480**.

---

### 2. The Endpoint (`nd-window-endpoint`)

**Required for dynamic loading & lifecycle management.**

* Identifies the window content source.
* Used internally as the unique key in the `WindowManager`.
* Matches the `data-modal` trigger.

```html
<div nd-window nd-window-endpoint="settings/profile.html">
```

---

### 3. Header (`nd-window-header`)

Defines the draggable top bar.

* Dragging starts only from this element.
* Automatically handles focus on interaction.
* Buttons must live inside the header.

---

### 4. Window Buttons (`nd-window-button`)

Control window actions.

| Value        | Action                    |
| ------------ | ------------------------- |
| `"maximize"` | Toggle maximize / restore |
| `"close"`    | Close the window          |

Example:

```html
<button nd-window-button="maximize">[ ]</button>
<button nd-window-button="close">X</button>
```

---

### 5. Content (`nd-window-content`)

The scrollable inner content container.

```html
<div nd-window-content>
  <!-- your app content -->
</div>
```

Padding and styling are applied automatically.

---

## Features & Interactions

### Focus Management

* Clicking a window brings it to the front.
* Z-index is managed internally.
* The focused window receives the `.focused` class.

---

### Dragging

* Drag starts from `nd-window-header`
* Automatically restores from maximized when dragging
* Saves position ratios for responsive resizing
* Snap detection is built-in

---

### Snap & Tiling

When dragging near screen edges:

* Left → Tile left
* Right → Tile right
* Top → Maximize
* Corners → Quadrant tiling (if enabled in config)

A `.snap-indicator` overlay appears to preview placement.

---

### Maximize / Restore

Handled via:

```js
manager.toggleMaximize(winElement)
```

CSS class applied:

```
.maximized
```

This sets:

* `top: 0`
* `left: 0`
* `width: 100%`
* `height: 100%`

---

### Keyboard Support

* `Escape` closes the topmost window.

---

### Animations

Two appearance states are supported:

| Class                    | Description           |
| ------------------------ | --------------------- |
| `.animate-appearance`    | Plays open animation  |
| `.animate-disappearance` | Plays close animation |

Animations use scale + opacity transitions.

---

## Lifecycle Events

The root container dispatches CustomEvents:

| Event            | Trigger                 |
| ---------------- | ----------------------- |
| `window:opened`  | A new window is created |
| `window:focused` | A window gains focus    |
| `window:closed`  | A window is closed      |

Example:

```js
root.addEventListener("window:opened", (e) => {
  console.log("Opened:", e.detail.endpoint);
});
```

---

## Technical Specifications

### Attribute Reference

| Attribute            | Level    | Required | Description                   |
| -------------------- | -------- | -------- | ----------------------------- |
| `nd-window`          | Root     | Yes      | Marks the element as a window |
| `nd-window-endpoint` | Root     | Yes*     | Unique endpoint key           |
| `nd-window-header`   | Child    | Yes      | Draggable header              |
| `nd-window-button`   | Child    | No       | Window control buttons        |
| `nd-window-content`  | Child    | Yes      | Content container             |
| `data-modal`         | Anywhere | No       | Opens a window                |

* Required for dynamic windows.

---

## Styling & Customization

Like Icons, Windows use **Design Tokens (CSS Variables)**.

All tokens are defined under `:root`.

---

### Window Base Tokens

| Variable                    | Default         | Description       |
| --------------------------- | --------------- | ----------------- |
| `--nd-window-bg`            | `#ffffff`       | Window background |
| `--nd-window-border`        | rgba(...)       | Border color      |
| `--nd-window-shadow`        | 0 12px 32px ... | Box shadow        |
| `--nd-window-bg-glass`      | rgba(...)       | Glass background  |
| `--nd-window-backdrop-blur` | 16px            | Blur intensity    |

---

### Header Tokens

| Variable                 | Description       |
| ------------------------ | ----------------- |
| `--nd-header-bg`         | Header background |
| `--nd-header-text`       | Header text color |
| `--nd-header-border`     | Divider border    |
| `--nd-header-bg-focused` | Focused header bg |

---

### Button Tokens

| Variable                  | Description        |
| ------------------------- | ------------------ |
| `--nd-btn-hover-bg`       | Hover background   |
| `--nd-btn-close-hover-bg` | Close button hover |
| `--nd-btn-max-hover-bg`   | Maximize hover     |

---

### Snap Tokens

| Variable           | Description             |
| ------------------ | ----------------------- |
| `--nd-snap-border` | Snap outline color      |
| `--nd-snap-bg`     | Snap preview background |
| `--nd-snap-radius` | Border radius           |

---

## Customization Examples

### 1. Glass Window Variant

```html
<div nd-window nd-variant="glass">
```

Uses backdrop blur + transparency.

---

### 2. Custom Dark Theme

```css
:root {
  --nd-window-bg: #0f172a;
  --nd-window-border: rgba(255,255,255,0.1);
  --nd-header-bg: #111827;
  --nd-header-text: #e5e7eb;
}
```

---

### 3. Disable Rounded Corners When Maximized

Already handled automatically:

```css
[nd-window].maximized {
  border-radius: 0 !important;
}
```

---

## Technical Classes Reference

| Selector              | Purpose                |
| --------------------- | ---------------------- |
| `[nd-window]`         | Base window            |
| `[nd-window-header]`  | Draggable header       |
| `[nd-window-content]` | Content container      |
| `[nd-window-button]`  | Window control buttons |
| `.focused`            | Active window          |
| `.maximized`          | Fullscreen state       |
| `.tiled`              | Snapped state          |
| `.snap-indicator`     | Visual snap preview    |

---

## Architecture Overview

The `WindowManager` orchestrates:

* Lifecycle (`WindowLifecycle`)
* Dragging (`WindowDrag`)
* Tiling (`WindowTiling`)
* State persistence (`WindowState`)
* Dynamic loading (`WindowLoader`)

You typically **do not interact with these directly** — use the public API:

```js
manager.open(endpoint)
manager.close(win)
manager.toggleMaximize(win)
manager.focus(win)
manager.getWindows()
```

---

## Best Practices

* Always define `nd-window-endpoint`
* Keep header minimal (title + buttons)
* Avoid heavy layout logic inside header
* Use `data-modal` instead of manual JS when possible
* Let the manager control z-index — never override it manually

---

## Summary

The NidamJS Window system provides:

✔ Declarative HTML structure
✔ Built-in drag & snap
✔ Focus & stacking management
✔ Dynamic content loading
✔ Persistent responsive positioning
✔ Design token theming

It behaves like a real desktop OS — inside the browser.

---
