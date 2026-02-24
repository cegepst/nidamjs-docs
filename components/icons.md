---
icon: file
---

# Icons

Icons are the primary interactive elements of the NidamJS desktop. They represent applications, files, or system actions. The system uses a declarative, attribute-based grid that handles positioning, drag-and-drop, and layout persistence automatically.

## Quick Start

To create a functional desktop icon grid, wrap your icons in a container using the `nd-icons` attribute.

```html
<section nd-icons="5:3">
  <div nd-icon="1:1" nd-id="browser" data-modal="web-browser">
    <img src="/icons/world.png" alt="Browser">
    <span>Web Browser</span>
  </div>
  <div nd-icon="2:1" nd-id="trash" data-modal="trash-bin">
    <img src="/icons/trash.png" alt="Trash">
    <span>Trash</span>
  </div>
</section>

```

---

## Core Attributes

The icon system is controlled via three main `nd-` attributes. You generally do not need to write JavaScript to manage these; the internal `IconManager` detects them automatically.

### 1. The Grid Container (`nd-icons`)

Applied to the parent element to define the grid dimensions.

* **Format**: `nd-icons="COLUMNS:ROWS"`
* **Example**: `nd-icons="8:5"` creates a grid with 8 columns and 5 rows.

### 2. The Icon Component (`nd-icon`)

Applied to individual icon elements to set their initial or current position.

* **Format**: `nd-icon="COL:ROW"`
* **Behavior**: If a position is already occupied, the system will automatically find the nearest available spot.

### 3. The Unique Identifier (`nd-id`)

**Required for persistence.** * **Purpose**: This ID is used as the key when saving the icon's position to local storage. Without an `nd-id`, the icon will reset to its default HTML position on page refresh.

---

## Technical Specifications

### Attribute Reference

| Attribute | Level | Required | Description |
| --- | --- | --- | --- |
| `nd-icons` | Parent | Yes | Defines the grid size (e.g., `10:6`). Sets CSS variables `--nd-cols` and `--nd-rows`. |
| `nd-icon` | Child | Yes | The `X:Y` coordinate of the icon. Updates dynamically after a drag. |
| `nd-id` | Child | Yes | Unique string used to save/load the position from storage. |
| `data-modal` | Child | No | The ID of the window/modal to trigger on click. |

---

## Features & Interactions

### Drag and Drop

Icons feature built-in drag-and-drop logic:

* **Deadzone & Delay**: A small movement "deadzone" and time delay prevent accidental drags during simple clicks.
* **Ghosting**: While dragging, a transparent "ghost" element follows the cursor (`.nd-icon-ghost`), while the original icon remains dimmed (`.is-dragging`).
* **Auto-Snapping**: Upon release, the icon snaps to the nearest valid grid cell.
* **Collision Detection**: Icons cannot overlap. If you drop an icon on an occupied cell, the move is canceled.

### Persistence

The layout is automatically saved to `localStorage` under the key `nd-icons-layout`.

1. On page load, the system checks storage for coordinates matching the icon's `nd-id`.
2. If found, the `nd-icon` attribute is updated, and the icon is moved to its last saved position.

---

## Styling & Customization

NidamJS icons are built with **Design Tokens** (CSS Variables). You can easily customize the entire desktop experience by overriding these variables in your own stylesheet.

### Theming Hooks (CSS Variables)

All variables are defined under `:root` but can be scoped to specific `[nd-icons]` containers if you want different styles for different desktop areas.

| Category | Variable | Default Value | Description |
| --- | --- | --- | --- |
| **Layout** | `--nd-gap` | `12px` | Spacing between grid cells. |
|  | `--nd-padding` | `16px` | Inner padding of the desktop container. |
| **Icons** | `--nd-icon-size` | `clamp(48px, 50%, 96px)` | Responsive sizing of the icon image. |
|  | `--nd-icon-hover-scale` | `1.15` | The scale factor when hovering an icon. |
|  | `--nd-icon-transition` | `transform 0.18s ease...` | Controls the smoothness of animations. |
| **Labels** | `--nd-label-bg` | `rgba(0, 0, 0, 0.12)` | The background color of the icon text. |
|  | `--nd-label-blur` | `12px` | Intensity of the glassmorphism backdrop. |
|  | `--nd-label-radius` | `4px` | Roundness of the label background. |
| **States** | `--nd-ghost-opacity` | `0.6` | Transparency of the icon following the mouse. |
|  | `--nd-dragging-opacity` | `0.3` | Transparency of the original icon during drag. |

---

### Customization Examples

#### 1. Dark Mode & High Visibility

The system automatically adapts to `prefers-color-scheme: dark`, but you can force specific colors:

```css
/* Custom "Neon" Theme Example */
:root {
  --nd-label-bg: rgba(0, 255, 65, 0.1);
  --nd-label-border: 1px solid #00ff41;
  --nd-label-text: #00ff41;
  --nd-icon-hover-scale: 1.25;
}

```

#### 2. Layout Overrides

If you want a dense, small-icon grid, you can override the variables directly on the container:

```css
.dense-grid {
  --nd-cols: 12;
  --nd-rows: 10;
  --nd-icon-size: 32px;
  --nd-gap: 4px;
}

```

---

## Technical Classes Reference

While you should primarily use variables, you can target these classes for advanced CSS overrides:

* **`[nd-icons]`**: The main grid container.
* **`[nd-icon]`**: The individual icon wrapper.
* **`[nd-icon] span`**: The label element.
* **`.is-dragging`**: Added to the icon currently being moved.
* **`.nd-icon-ghost`**: The temporary element created during a drag operation (appended to `<body>`).

> **Note**: The `nd-icon-ghost` element is removed from the DOM as soon as the mouse is released.
