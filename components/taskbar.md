---
icon: rows
---

# Taskbar

The `Taskbar` component provides a persistent interface for launching applications and managing open windows. Inspired by modern desktop environments like macOS, it offers a sleek, glassmorphic design with flexible positioning.

## Overview

The taskbar acts as the primary navigation hub for the NidamJS desktop. It displays application icons that serve as triggers to open windows. When a window is open, the taskbar provides visual feedback and allows users to bring the window back into focus with a single click.

### Features

- **Flexible Positioning**: Dock the taskbar to the bottom (default), left, or right side of the screen.
- **Visual Indicators**: Active applications are highlighted with a pill-shaped indicator.
- **Glassmorphism**: Built-in support for backdrop blur and semi-transparent backgrounds.
- **Automatic Sync**: Real-time updates based on window lifecycle events (`opened`/`closed`).
- **Responsive Animations**: Smooth transitions for hover states and state changes.

## Usage

### Basic Structure

The taskbar is defined in HTML using the `nd-taskbar` attribute. Individual items use the `tb-icon` attribute and must map to a window endpoint via `data-modal`.

```html
<div nd-taskbar>
  <div tb-icon data-modal="terminal">
    <img src="/icons/terminal.png" alt="Terminal" />
  </div>
  <div tb-icon data-modal="browser">
    <img src="/icons/browser.png" alt="Browser" />
  </div>
  <div tb-icon data-modal="settings">
    <img src="/icons/settings.png" alt="Settings" />
  </div>
</div>
```

### Changing Position

You can change the orientation and position of the taskbar using the `tb-position` attribute.

+++ Bottom (Default)
```html
<div nd-taskbar tb-position="bottom">...</div>
```
+++ Left
```html
<div nd-taskbar tb-position="left">...</div>
```
+++ Right
```html
<div nd-taskbar tb-position="right">...</div>
```
+++

## Structure & Styling

The `TaskbarManager` targets specific attributes to apply logic and styling.

```html
<div nd-taskbar tb-position="bottom">
  <!-- Taskbar Icon -->
  <div tb-icon data-modal="app-id" class="is-open">
    <img src="..." />
    <!-- The "is-open" class adds the active indicator via CSS ::after -->
  </div>
</div>
```

### CSS Variables

Customize the look and feel of the taskbar by overriding these CSS variables in your stylesheet:

```css
:root {
  /* Taskbar Container */
  --tb-bg: rgba(255, 255, 255, 0.1);
  --tb-radius: 24px;
  --tb-offset: 10px; /* Distance from screen edge */
  --tb-pad-x: 1rem;
  --tb-pad-y: 0.5rem;

  /* Icons */
  --tb-icon-size: 54px;
  --tb-icon-radius: 14px;

  /* Active Indicator */
  --tb-indicator-bg: #ffffff;
  --tb-indicator-glow: rgba(255, 255, 255, 0.3);
}
```

## API Reference

### HTML Attributes

| Attribute | Target | Values | Description |
| :--- | :--- | :--- | :--- |
| `nd-taskbar` | Container | - | Identifies the taskbar element. |
| `tb-position` | Container | `bottom`, `left`, `right` | Sets the placement of the taskbar. |
| `tb-icon` | Icon Item | - | Identifies a clickable icon item. |
| `data-modal` | Icon Item | `string` | The window endpoint/ID to trigger on click. |
