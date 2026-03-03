---
icon: gear
order: 90
---

# Configuration

NidamJS is configured by passing an object to `createNidamApp()`. This object controls global behavior and window manager settings.

## App Options

These options control the overall environment setup.

| Option           | Type     | Default     | Description                                                |
| :--------------- | :------- | :---------- | :--------------------------------------------------------- |
| `modalContainer` | `string` | `"#target"` | Selector for the element where windows are appended.       |
| `registry`       | `Array`  | `[]`        | List of feature modules to auto-initialize in new content. |
| `refreshMap`     | `Object` | `null`      | Configuration for automatic window refreshes.              |
| `refreshTimeout` | `number` | `200`       | Delay in ms before a refresh or destructive closure.       |

## Window Manager Configuration

Configure the core physics and lifecycle of windows under the `windowManager.config` key.

| Option                  | Type     | Default | Description                                              |
| :---------------------- | :------- | :------ | :------------------------------------------------------- |
| `maxWindows`            | `number` | `10`    | Maximum number of concurrent open windows.               |
| `cooldownMs`            | `number` | `500`   | Minimum time between opening the same window.            |
| `zIndexBase`            | `number` | `40`    | The starting z-index for the window stack.               |
| `taskbarHeight`         | `number` | `0`     | Height reserved for the taskbar (affects maximize/snap). |
| `dragThreshold`         | `number` | `10`    | Pixels to move before a drag operation starts.           |
| `layoutStabilizationMs` | `number` | `450`   | How long to auto-recenter windows after initial load.    |

## Notifications

NidamJS uses built-in toast notifications for window errors (for example max windows reached or open failures).

To customize toast appearance, override CSS variables in your app stylesheet:

```css
:root {
  --nd-toast-bg-error: rgba(127, 29, 29, 0.92);
  --nd-toast-bg-success: rgba(6, 78, 59, 0.9);
  --nd-toast-radius: 0.75rem;
}
```

## Example Configuration

```javascript
const app = createNidamApp({
  modalContainer: "#desktop",
  windowManager: {
    config: {
      maxWindows: 5,
      taskbarHeight: 80,
      cooldownMs: 300,
    },
  },
  registry: [
    {
      selector: ".custom-widget",
      init: (el) => new MyWidget(el),
    },
  ],
});
```
