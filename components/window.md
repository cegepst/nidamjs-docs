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
    <!-- Your window content here -->
  </div>
</div>
```

To open it from anywhere:

```html
<button data-modal="examples/shared/page-one.html">Open Page One</button>
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

- Identifies the window content source.
- Used internally as the unique key in the `WindowManager`.
- Matches the `data-modal` trigger.

```html
<div nd-window nd-window-endpoint="settings/profile.html"></div>
```

---

### 3. Header (`nd-window-header`)

Defines the draggable top bar.

- Dragging starts only from this element.
- Automatically handles focus on interaction.
- Buttons must live inside the header.

---

### 4. Window Buttons (`nd-window-button`)

Control window actions.

| Value        | Action                    |
| ------------ | ------------------------- |
| `"maximize"` | Toggle maximize / restore |
| `"close"`    | Close the window          |

Example:

```html
<button nd-window-button="maximize">[ ]</button> <button nd-window-button="close">X</button>
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

- Clicking a window brings it to the front.
- Z-index is managed internally.
- The focused window receives the `.focused` class.

---

### Dragging

- Drag starts from `nd-window-header`
- Automatically restores from maximized when dragging
- Saves position ratios for responsive resizing
- Snap detection is built-in

---

### Snap & Tiling

When dragging near screen edges:

- Left → Tile left
- Right → Tile right
- Top → Maximize
- Corners → Quadrant tiling (if enabled in config)

A `.snap-indicator` overlay appears to preview placement.

---

### Maximize / Restore

Handled via:

```js
manager.toggleMaximize(winElement);
```

CSS class applied:

```
.maximized
```

This sets:

- `top: 0`
- `left: 0`
- `width: 100%`
- `height: 100%`

---

### Keyboard Support

- `Escape` closes the topmost window.

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
root.addEventListener('window:opened', (e) => {
  console.log('Opened:', e.detail.endpoint);
});
```

---

## Technical Specifications

### Attribute Reference

| Attribute            | Level    | Required | Description                   |
| -------------------- | -------- | -------- | ----------------------------- |
| `nd-window`          | Root     | Yes      | Marks the element as a window |
| `nd-window-endpoint` | Root     | Yes\*    | Unique endpoint key           |
| `nd-window-header`   | Child    | Yes      | Draggable header              |
| `nd-window-button`   | Child    | No       | Window control buttons        |
| `nd-window-content`  | Child    | Yes      | Content container             |
| `data-modal`         | Anywhere | No       | Opens a window                |

- Required for dynamic windows.

---

## Styling & Customization

Like Icons, Windows use **Design Tokens (CSS Variables)**.

All tokens are defined under `:root`.

---

### Window Base Tokens

| Variable                    | Light Default                  | Dark Default                  |
| --------------------------- | ------------------------------ | ----------------------------- |
| `--nd-window-bg`            | `#ffffff`                      | `#1f2937`                     |
| `--nd-window-border`        | `rgba(0,0,0,0.08)`             | `rgba(255,255,255,0.08)`      |
| `--nd-window-shadow`        | `0 12px 32px rgba(0,0,0,0.15)` | `0 20px 40px rgba(0,0,0,0.5)` |
| `--nd-window-bg-glass`      | `rgba(255,255,255,0.7)`        | (inherits bg)                 |
| `--nd-window-backdrop-blur` | `16px`                         | `16px`                        |

---

### Header Tokens

| Variable                 | Light                        | Dark                               |
| ------------------------ | ---------------------------- | ---------------------------------- |
| `--nd-header-bg`         | `#ffffff`                    | `#1f2937`                          |
| `--nd-header-border`     | `rgba(0,0,0,0.06)`           | `rgba(255,255,255,0.08)`           |
| `--nd-header-text`       | `#111827`                    | `#f3f4f6`                          |
| `--nd-header-bg-focused` | `#f9fafb`                    | `#111827`                          |
| `--nd-header-focus-ring` | `0 0 0 1px rgba(0,0,0,0.06)` | `0 0 0 1px rgba(255,255,255,0.08)` |

---

### Button Tokens

#### Light Defaults

| Variable                    | Value                  |
| --------------------------- | ---------------------- |
| `--nd-btn-bg`               | `transparent`          |
| `--nd-btn-border`           | `rgba(0,0,0,0.08)`     |
| `--nd-btn-text`             | `inherit`              |
| `--nd-btn-hover-bg`         | `rgba(0,0,0,0.05)`     |
| `--nd-btn-hover-border`     | `rgba(0,0,0,0.15)`     |
| `--nd-btn-close-hover-bg`   | `rgba(220,38,38,0.12)` |
| `--nd-btn-close-hover-text` | `#b91c1c`              |
| `--nd-btn-max-hover-bg`     | `rgba(0,0,0,0.08)`     |

#### Dark Overrides

| Variable                    | Value                    |
| --------------------------- | ------------------------ |
| `--nd-btn-border`           | `rgba(255,255,255,0.08)` |
| `--nd-btn-hover-bg`         | `rgba(255,255,255,0.08)` |
| `--nd-btn-hover-border`     | `rgba(255,255,255,0.15)` |
| `--nd-btn-close-hover-bg`   | `rgba(248,113,113,0.18)` |
| `--nd-btn-close-hover-text` | `#fca5a5`                |
| `--nd-btn-max-hover-bg`     | `rgba(255,255,255,0.12)` |

---

### Snap Tokens

| Variable                 | Light              | Dark                     |
| ------------------------ | ------------------ | ------------------------ |
| `--nd-snap-border`       | `rgba(0,0,0,0.25)` | `rgba(255,255,255,0.35)` |
| `--nd-snap-bg`           | `rgba(0,0,0,0.08)` | `rgba(255,255,255,0.08)` |
| `--nd-snap-border-width` | `2px`              | `2px`                    |
| `--nd-snap-radius`       | `0.85rem`          | `0.85rem`                |

---

## Variants

### Glass Window Variant

```html
<div nd-window nd-variant="glass"></div>
```

Uses backdrop blur + transparency.

---

## Architecture Overview

The `WindowManager` orchestrates:

- Lifecycle (`WindowLifecycle`)
- Dragging (`WindowDrag`)
- Tiling (`WindowTiling`)
- State persistence (`WindowState`)
- Dynamic loading (`WindowLoader`)

You typically **do not interact with these directly** — use the public API:

```js
manager.open(endpoint);
manager.close(win);
manager.toggleMaximize(win);
manager.focus(win);
manager.getWindows();
```

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
