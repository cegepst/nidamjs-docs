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

Taskbar Container

| Variable | Default | Description |
|----------|---------|-------------|
| --nd-taskbar-position | absolute | Position type |
| --nd-taskbar-align | center | Align items |
| --nd-taskbar-justify | space-evenly | Justify content |
| --nd-taskbar-gap | 1.6rem | Gap between items |
| --nd-taskbar-pad-y | 0.5rem | Vertical padding |
| --nd-taskbar-pad-xrem | Horizontal` | `1 padding |
| --nd-taskbar-radius | 24px | Border radius |
| --nd-taskbar-bg | rgba(255, 255, 255, 0.1) | Background |
| --nd-taskbar-backdrop-filter | blur(12px) | Backdrop blur |
| --nd-taskbar-border | 1px solid rgba(255, 255, 255, 0.2) | Border |
| --nd-taskbar-box-shadow | 0 8px 32px rgba(0, 0, 0, 0.3) | Box shadow |
| --nd-taskbar-z-index | 1000 | Z-index |
| --nd-taskbar-transition | all 0.3s cubic-bezier(0.4, 0, 0.2, 1) | Transition |
| --nd-taskbar-bottom | 10px | Bottom offset |
| --nd-taskbar-left | 50% | Left position |
| --nd-taskbar-transform | translateX(-50%) | Transform |
| --nd-taskbar-width | fit-content | Width |
| --nd-taskbar-min-width | 300px | Min width |
| --nd-taskbar-height | fit-content | Height |
| --nd-taskbar-min-height | 200px | Min height (side positions) |
| --nd-taskbar-offset | 10px | General offset |
| --nd-taskbar-top | 50% | Top position |
| --nd-taskbar-right | 30px | Right position |

Icons
| Variable | Default | Description |
|----------|---------|-------------|
| --nd-taskbar-icon-size | 54px | Icon size (width & height) |
| --nd-taskbar-icon-width | 54px | Icon width |
| --nd-taskbar-icon-height | 54px | Icon height |
| --nd-taskbar-icon-pad | 0.5rem | Icon padding |
| --nd-taskbar-icon-radius | 14px | Icon border radius |
| --nd-taskbar-icon-max-width | 4rem | Max width (side positions) |
| --nd-taskbar-icon-bg | transparent | Icon background |
| --nd-taskbar-icon-color | inherit | Icon text color |
| --nd-taskbar-icon-hover | rgb(71 85 105 / 0.6) | Hover background |
| --nd-taskbar-icon-cursor | pointer | Cursor |
| --nd-taskbar-icon-display | flex | Display |
| --nd-taskbar-icon-align | center | Align items |
| --nd-taskbar-icon-justify | center | Justify content |
| --nd-taskbar-icon-object-fit | contain | Object fit |
| --nd-taskbar-icon-transition | background 0.2s ease, transform 0.2s ease | Transition |

Indicator (is-open line)
| Variable | Default | Description |
|----------|---------|-------------|
| --nd-taskbar-indicator-bg | #fff | Indicator background |
| --nd-taskbar-indicator-width | 12px | Indicator width |
| --nd-taskbar-indicator-height | 3px | Indicator height |
| --nd-taskbar-indicator-bottom | -6px | Bottom position |
| --nd-taskbar-indicator-left | 50% | Left position |
| --nd-taskbar-indicator-transform | translateX(-50%) | Transform |
| --nd-taskbar-indicator-radius | 10px | Border radius |
| --nd-taskbar-indicator-shadow | 0 0 8px rgba(255, 255, 255, 0.3) | Box shadow |
| --nd-taskbar-indicator-opacity | 0.8 | Opacity |
| --nd-taskbar-indicator-transition | all 0.3s ease | Transition |
| --nd-taskbar-indicator-hover-width | 18px | Hover width |
| --nd-taskbar-indicator-hover-opacity | 1 | Hover opacity |

Extended (Windows-style) Additional Vars
| Variable | Default | Description |
|----------|---------|-------------|
| --nd-taskbar-icon-border | 1px solid transparent | Icon border |
| --nd-taskbar-icon-font-size | 0.85rem | Icon font size |
| --nd-taskbar-icon-min-width | 50px | Min width |
| --nd-taskbar-icon-open-bg | rgb(71 85 105 / 0.4) | Open state background |
| --nd-taskbar-icon-hover-transform | none | Hover transform |
| --nd-taskbar-icon-hover-border | transparent | Hover border |

## API Reference

### HTML Attributes

| Attribute | Target | Values | Description |
| :--- | :--- | :--- | :--- |
| `nd-taskbar` | Container | - | Identifies the taskbar element. |
| `nd-taskbar-position` | Container | `bottom`, `left`, `right` | Sets the placement of the taskbar. |
| `nd-taskbar-icon` | Icon Item | - | Identifies a clickable icon item. |
| `data-modal` | Icon Item | `string` | The window endpoint/ID to trigger on click. |
