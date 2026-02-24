---
icon: rows
---

# Taskbar

The `Taskbar` component provides a persistent interface for launching applications and managing open windows in the NidamJS desktop environment. It displays application icons that serve as triggers to open windows, with visual feedback when windows are open.

## Overview

The taskbar acts as the primary navigation hub for the desktop. It docks to one edge of the screen and displays application icons. When a window is open, the corresponding taskbar icon shows an active indicator, and clicking the icon will either bring the existing window into focus or open a new one.

### How It Works

The `TaskbarManager` class handles the taskbar functionality:

1. **Event Binding**: Listens for clicks on taskbar icons to trigger window opening
2. **Window State Sync**: Subscribes to `window:opened` and `window:closed` events to update icon states
3. **Visual Feedback**: Toggles the `is-open` class on icons based on window state

When you click a taskbar icon, the manager calls `windowManager.open(endpoint)` with the endpoint specified in the icon's `data-modal` attribute. If a window with that endpoint is already open, it will be brought into focus.

### Features

- **Flexible Positioning**: Dock the taskbar to the bottom (default), left, or right side of the screen
- **Visual Indicators**: Active applications display a pill-shaped indicator below the icon
- **Glassmorphism**: Built-in support for backdrop blur and semi-transparent backgrounds
- **Automatic Sync**: Real-time updates based on window lifecycle events
- **Responsive Animations**: Smooth transitions for hover states and state changes
- **Multiple Styles**: Default floating style and extended Windows-style bar

## Usage

### Basic Structure

The taskbar is defined in HTML using the `nd-taskbar` attribute. Individual items use the `nd-taskbar-icon` attribute and must map to a window endpoint via `data-modal`.

```html
<div nd-taskbar>
  <div nd-taskbar-icon data-modal="terminal">
    <img src="/icons/terminal.png" alt="Terminal" />
  </div>
  <div nd-taskbar-icon data-modal="browser">
    <img src="/icons/browser.png" alt="Browser" />
  </div>
  <div nd-taskbar-icon data-modal="settings">
    <img src="/icons/settings.png" alt="Settings" />
  </div>
</div>
```

### Taskbar Styles

NidamJS provides two built-in taskbar styles:

**Default Style (Floating)**

The default floating style positions the taskbar as a rounded bar that floats above the desktop with a glassmorphic appearance.

```html
<div nd-taskbar>
  <!-- Icons here -->
</div>
```

**Extended Style (Windows-style)**

The extended style creates a full-width bar docked to the bottom of the screen, similar to the Windows taskbar.

```html
<div nd-taskbar="extend">
  <!-- Icons here -->
</div>
```

### Changing Position

You can change the orientation and position of the taskbar using the `nd-taskbar-position` attribute.

**Bottom (Default)**

```html
<div nd-taskbar nd-taskbar-position="bottom">...</div>
```

**Left**

```html
<div nd-taskbar nd-taskbar-position="left">...</div>
```

**Right**

```html
<div nd-taskbar nd-taskbar-position="right">...</div>
```

### Icon Labels

For the extended taskbar style, you can add text labels beneath or beside icons:

```html
<div nd-taskbar="extend">
  <div nd-taskbar-icon data-modal="terminal">
    <img src="/icons/terminal.png" alt="Terminal" />
    <span>Terminal</span>
  </div>
</div>
```

Or use buttons with text:

```html
<div nd-taskbar="extend">
  <button nd-taskbar-icon class="toolbar-btn" data-modal="terminal">
    Terminal
  </button>
</div>
```

## Structure & Styling

### DOM Structure

The `TaskbarManager` targets specific attributes to apply logic and styling.

```html
<!-- Taskbar Container -->
<div nd-taskbar nd-taskbar-position="bottom">
  <!-- Inactive Icon -->
  <div nd-taskbar-icon data-modal="app-id">
    <img src="..." />
  </div>

  <!-- Active Icon (is-open class added by TaskbarManager) -->
  <div nd-taskbar-icon data-modal="app-id" class="is-open">
    <img src="..." />
    <!-- The "is-open" class displays the indicator via CSS ::after -->
  </div>
</div>
```

### CSS Architecture

The taskbar styles are organized in three CSS files:

- `root.css`: Defines all CSS custom properties (variables)
- `default.css`: Implements the default floating taskbar style
- `extend.css`: Implements the extended Windows-style taskbar

### CSS Variables

Customize the look and feel of the taskbar by overriding CSS custom properties in your stylesheet. Variables are scoped under the `[nd-taskbar]` selector.

#### Taskbar Container

| Variable                       | Default                                 | Description                               |
| ------------------------------ | --------------------------------------- | ----------------------------------------- |
| `--nd-taskbar-position`        | `absolute`                              | Position type (absolute, fixed, relative) |
| `--nd-taskbar-align`           | `center`                                | Vertical alignment of items               |
| `--nd-taskbar-justify`         | `space-evenly`                          | Horizontal distribution of items          |
| `--nd-taskbar-gap`             | `1.6rem`                                | Gap between icon items                    |
| `--nd-taskbar-pad-y`           | `0.6rem`                                | Vertical padding                          |
| `--nd-taskbar-pad-x`           | `1rem`                                  | Horizontal padding                        |
| `--nd-taskbar-radius`          | `24px`                                  | Border radius                             |
| `--nd-taskbar-bg`              | `rgba(255, 255, 255, 0.1)`              | Background color with transparency        |
| `--nd-taskbar-backdrop-filter` | `blur(12px)`                            | Backdrop blur effect                      |
| `--nd-taskbar-border`          | `1px solid rgba(255, 255, 255, 0.2)`    | Border style                              |
| `--nd-taskbar-box-shadow`      | `0 8px 32px rgba(0, 0, 0, 0.3)`         | Box shadow                                |
| `--nd-taskbar-z-index`         | `1000`                                  | Stacking order                            |
| `--nd-taskbar-transition`      | `all 0.3s cubic-bezier(0.4, 0, 0.2, 1)` | Transition timing                         |

#### Positioning (Bottom)

| Variable                 | Default            | Description               |
| ------------------------ | ------------------ | ------------------------- |
| `--nd-taskbar-bottom`    | `10px`             | Distance from bottom edge |
| `--nd-taskbar-left`      | `50%`              | Horizontal position       |
| `--nd-taskbar-transform` | `translateX(-50%)` | Transform for centering   |
| `--nd-taskbar-width`     | `fit-content`      | Container width           |
| `--nd-taskbar-min-width` | `300px`            | Minimum width             |

#### Positioning (Side - Left/Right)

| Variable                  | Default       | Description              |
| ------------------------- | ------------- | ------------------------ |
| `--nd-taskbar-top`        | `auto`        | Distance from top edge   |
| `--nd-taskbar-right`      | `auto`        | Distance from right edge |
| `--nd-taskbar-height`     | `fit-content` | Container height         |
| `--nd-taskbar-min-height` | `200px`       | Minimum height           |
| `--nd-taskbar-offset`     | `10px`        | General offset           |

#### Flex Layout

| Variable                      | Default | Description                     |
| ----------------------------- | ------- | ------------------------------- |
| `--nd-taskbar-flex-direction` | `row`   | Direction of items (row/column) |

#### Icon Item

| Variable                       | Default                                     | Description                    |
| ------------------------------ | ------------------------------------------- | ------------------------------ |
| `--nd-taskbar-icon-display`    | `flex`                                      | Display type                   |
| `--nd-taskbar-icon-align`      | `center`                                    | Vertical alignment             |
| `--nd-taskbar-icon-justify`    | `center`                                    | Horizontal alignment           |
| `--nd-taskbar-icon-width`      | `54px`                                      | Icon container width           |
| `--nd-taskbar-icon-height`     | `54px`                                      | Icon container height          |
| `--nd-taskbar-icon-size`       | `54px`                                      | Legacy alias for width         |
| `--nd-taskbar-icon-pad`        | `0.5rem`                                    | Internal padding               |
| `--nd-taskbar-icon-radius`     | `14px`                                      | Border radius                  |
| `--nd-taskbar-icon-cursor`     | `pointer`                                   | Cursor type                    |
| `--nd-taskbar-icon-transition` | `background 0.2s ease, transform 0.2s ease` | Transition timing              |
| `--nd-taskbar-icon-position`   | `relative`                                  | Position context               |
| `--nd-taskbar-icon-object-fit` | `contain`                                   | Image fit mode                 |
| `--nd-taskbar-icon-bg`         | `transparent`                               | Background color               |
| `--nd-taskbar-icon-color`      | `#f1f5f9`                                   | Text color                     |
| `--nd-taskbar-icon-min-width`  | `auto`                                      | Minimum width                  |
| `--nd-taskbar-icon-font-size`  | `0.85rem`                                   | Font size for text labels      |
| `--nd-taskbar-icon-transform`  | `none`                                      | Transform applied              |
| `--nd-taskbar-icon-border`     | `1px solid transparent`                     | Border style                   |
| `--nd-taskbar-icon-max-width`  | `4rem`                                      | Maximum width (side positions) |

#### Icon Hover State

| Variable                            | Default                | Description            |
| ----------------------------------- | ---------------------- | ---------------------- |
| `--nd-taskbar-icon-hover`           | `rgb(71 85 105 / 0.6)` | Hover background color |
| `--nd-taskbar-icon-hover-transform` | `none`                 | Hover transform        |
| `--nd-taskbar-icon-hover-border`    | `transparent`          | Hover border           |

#### Icon Open State (Extended Style)

| Variable                    | Default       | Description                    |
| --------------------------- | ------------- | ------------------------------ |
| `--nd-taskbar-icon-open-bg` | `transparent` | Background when window is open |

#### Active Indicator (Pill)

| Variable                            | Default                            | Description                |
| ----------------------------------- | ---------------------------------- | -------------------------- |
| `--nd-taskbar-indicator-bg`         | `#fff`                             | Indicator background color |
| `--nd-taskbar-indicator-width`      | `12px`                             | Indicator width            |
| `--nd-taskbar-indicator-height`     | `3px`                              | Indicator height           |
| `--nd-taskbar-indicator-bottom`     | `-6px`                             | Distance from icon bottom  |
| `--nd-taskbar-indicator-left`       | `50%`                              | Horizontal position        |
| `--nd-taskbar-indicator-transform`  | `translateX(-50%)`                 | Centering transform        |
| `--nd-taskbar-indicator-radius`     | `10px`                             | Border radius              |
| `--nd-taskbar-indicator-shadow`     | `0 0 8px rgba(255, 255, 255, 0.3)` | Glow effect                |
| `--nd-taskbar-indicator-opacity`    | `0.8`                              | Opacity level              |
| `--nd-taskbar-indicator-transition` | `all 0.3s ease`                    | Transition timing          |

#### Indicator Hover State

| Variable                               | Default | Description           |
| -------------------------------------- | ------- | --------------------- |
| `--nd-taskbar-indicator-hover-width`   | `18px`  | Width on icon hover   |
| `--nd-taskbar-indicator-hover-height`  | `auto`  | Height on icon hover  |
| `--nd-taskbar-indicator-hover-opacity` | `1`     | Opacity on icon hover |

## Customization Examples

### Changing Taskbar Appearance

```css
:root {
  /* Make the taskbar darker and more opaque */
  --nd-taskbar-bg: rgba(15, 23, 42, 0.85);
  --nd-taskbar-border: 1px solid rgba(100, 116, 139, 0.3);

  /* Adjust positioning */
  --nd-taskbar-bottom: 20px;
  --nd-taskbar-radius: 16px;

  /* Customize the blur effect */
  --nd-taskbar-backdrop-filter: blur(20px);
}
```

### Customizing Icons

```css
:root {
  /* Larger icons */
  --nd-taskbar-icon-width: 64px;
  --nd-taskbar-icon-height: 64px;
  --nd-taskbar-icon-radius: 16px;

  /* Custom hover effect */
  --nd-taskbar-icon-hover: rgba(59, 130, 246, 0.5);
  --nd-taskbar-icon-hover-transform: scale(1.1);
}
```

### Customizing the Active Indicator

```css
:root {
  /* Make the indicator more prominent */
  --nd-taskbar-indicator-bg: #3b82f6;
  --nd-taskbar-indicator-width: 24px;
  --nd-taskbar-indicator-height: 4px;
  --nd-taskbar-indicator-shadow: 0 0 12px rgba(59, 130, 246, 0.6);
}
```

### Creating a Dark Theme Taskbar

```css
:root {
  --nd-taskbar-bg: rgba(30, 41, 59, 0.9);
  --nd-taskbar-backdrop-filter: blur(16px);
  --nd-taskbar-border: 1px solid rgba(51, 65, 85, 0.5);
  --nd-taskbar-icon-hover: rgba(51, 65, 85, 0.7);
  --nd-taskbar-indicator-bg: #38bdf8;
  --nd-taskbar-indicator-shadow: 0 0 10px rgba(56, 189, 248, 0.5);
}
```

### Side Taskbar Configuration

```css
:root {
  /* Left position */
  --nd-taskbar-position: fixed;
  --nd-taskbar-left: 10px;
  --nd-taskbar-top: 50%;
  --nd-taskbar-transform: translateY(-50%);
  --nd-taskbar-flex-direction: column;
  --nd-taskbar-min-height: 400px;

  /* Adjust icons for vertical layout */
  --nd-taskbar-icon-max-width: 60px;
}
```

## Extended Style Variables

When using `nd-taskbar="extend"`, additional variables customize the Windows-style appearance:

```css
[nd-taskbar="extend"] {
  --nd-taskbar-width: 100%;
  --nd-taskbar-min-width: 100%;
  --nd-taskbar-bottom: 0;
  --nd-taskbar-left: 0;
  --nd-taskbar-transform: none;
  --nd-taskbar-radius: 0;
  --nd-taskbar-bg: rgb(30 41 59 / 0.9);
}

[nd-taskbar="extend"] [nd-taskbar-icon] {
  --nd-taskbar-icon-width: auto;
  --nd-taskbar-icon-min-width: 50px;
  --nd-taskbar-icon-height: 42px;
  --nd-taskbar-icon-radius: 4px;
}
```

## API Reference

### HTML Attributes

| Attribute             | Target    | Values                    | Description                                 |
| :-------------------- | :-------- | :------------------------ | :------------------------------------------ |
| `nd-taskbar`          | Container | -                         | Identifies the taskbar element.             |
| `nd-taskbar`          | Container | `"extend"`                | Enables the extended Windows-style taskbar. |
| `nd-taskbar-position` | Container | `bottom`, `left`, `right` | Sets the placement of the taskbar.          |
| `nd-taskbar-icon`     | Icon Item | -                         | Identifies a clickable icon item.           |
| `data-modal`          | Icon Item | `string`                  | The window endpoint/ID to trigger on click. |

### CSS Selectors

| Selector                                    | Description                        |
| :------------------------------------------ | :--------------------------------- |
| `[nd-taskbar]`                              | Targets the taskbar container      |
| `[nd-taskbar="extend"]`                     | Targets the extended style taskbar |
| `[nd-taskbar][nd-taskbar-position="left"]`  | Targets left-positioned taskbar    |
| `[nd-taskbar][nd-taskbar-position="right"]` | Targets right-positioned taskbar   |
| `[nd-taskbar] [nd-taskbar-icon]`            | Targets all taskbar icons          |
| `[nd-taskbar] [nd-taskbar-icon].is-open`    | Targets icons with open windows    |
| `[nd-taskbar] [nd-taskbar-icon]:hover`      | Targets icons on hover             |

### JavaScript Events

The TaskbarManager listens for these window events:

| Event Name      | Detail                 | Description                                                                                     |
| :-------------- | :--------------------- | :---------------------------------------------------------------------------------------------- |
| `window:opened` | `{ endpoint: string }` | Fired when a window opens. Updates the corresponding taskbar icon to show the active indicator. |
| `window:closed` | `{ endpoint: string }` | Fired when a window closes. Removes the active indicator from the corresponding taskbar icon.   |

### Programmatic Control

To manually control taskbar icons from JavaScript:

```javascript
// Get all taskbar icons for a specific endpoint
const icons = document.querySelectorAll(
  '[nd-taskbar-icon][data-modal="terminal"]',
);

// Manually add the open state
icons.forEach((icon) => icon.classList.add("is-open"));

// Remove the open state
icons.forEach((icon) => icon.classList.remove("is-open"));
```
