---
icon: file
---

# Icons

Icons are the primary interactive elements on the desktop surface. They represent applications, files, shortcuts, or
system actions. NidamJS provides a grid-based icon system with support for drag-and-drop, selection, and execution.

## Defining Icons

Desktop icons are rendered as HTML elements within the desktop container. They use the `.desktop-icon` class and grid position classes like `col-start-X` and `row-start-X`.

### Structure

Each icon consists of a container with its grid position, an image/icon, and a label.

```html
<div class="desktop-icon col-start-1 row-start-1" data-modal="my-app">
  <div class="icon-image">
    <img src="/assets/icons/computer.png" alt="My PC" />
  </div>
  <span class="icon-label">My PC</span>
</div>
```

### Dynamic Management

You can use the `DesktopIconManager` to handle dragging and layout persistence.

```javascript
import { DesktopIconManager } from "nidamjs";

const iconManager = new DesktopIconManager(desktopElement, delegator, {
  storageKey: "my_desktop_layout",
});
```

## Icon Properties

Each icon element typically uses these attributes:

| Property | Type | Description |
| :--- | :--- | :--- |
| `class` | `string` | Must include `.desktop-icon` and `col-start-X`, `row-start-X`. |
| `data-modal` | `string` | The window endpoint to open on click/double-click. |

## Layout and Grid

Icons are automatically snapped to a grid to keep the desktop organized.

### Grid Positioning

Position icons by applying grid classes:

- `col-start-1` to `col-start-N`
- `row-start-1` to `row-start-N`

### Persistence

The `DesktopIconManager` automatically saves the layout to `localStorage` when an icon is dragged to a new position.

```javascript
const iconManager = new DesktopIconManager(container, delegator, {
  storageNamespace: "user_123", // Optional prefix for storage keys
});
```

## Interaction

### Drag and Drop

Icons can be dragged around the desktop.

- **Dragging**: When dragging starts, the `.dragging` class is added.
- **Snapping**: Dropping an icon snaps it to the nearest grid cell.
- **Collisions**: If a cell is already occupied, the icon returns to its original position.

### Execution

The `WindowManager` automatically listens for clicks on elements with the `data-modal` attribute and opens the corresponding window.

```html
<div class="desktop-icon" data-modal="terminal">...</div>
```

## Styling

Icons are rendered as HTML elements and can be styled using CSS.

```css
/* Container for the icon and label */
.desktop-icon {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  padding: 0.5rem;
  transition: background 0.2s ease;
}

/* Hover state */
.desktop-icon:hover {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 0.5rem;
}

/* Image styling */
.desktop-icon img {
  width: 48px;
  height: 48px;
  object-fit: contain;
}

/* Label styling */
.desktop-icon span {
  font-size: 0.8rem;
  text-align: center;
  color: #fff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.8);
}

/* Dragging state */
.desktop-icon.dragging {
  opacity: 0.6;
  z-index: 1000;
  pointer-events: none;
}
```
