---
icon: gear
---

# Configuration

NidamJS is fully configurable through a structured configuration object.

You can override:

- Global behavior
- Window system logic
- Performance timing
- Snap behavior
- Drag sensitivity
- Layout constraints
- Logging system

---

## Enabling Custom Config

To use a custom configuration, the script **must be loaded manually**.

```html
<script type="module" src="example.js" data-nd-init></script>
```

```js
//example.js

import config from './nidam.config.json' with { type: 'json' };
import initNidamApp from "@moonitoring/nidamjs";

initNidamApp(config);

/*
You can pass a simple js object as config instead of using a config file
initNidamApp({});
*/
```

---

## Global Config Options

### `root`

**Type:** `Document | HTMLElement`
**Default:** `document`

Defines where event delegation is attached.

#### Example

```js
root: document.querySelector("#desktop");
```

Use this if you want multiple isolated desktops.

---

### `modalContainer`

**Type:** `string`
**Default:** `"#target"`

CSS selector where windows are injected.

```js
modalContainer: "#desktop-layer";
```

---

### `pendingModalDatasetKey`

**Type:** `string`
**Default:** `"pendingModal"`

Dataset key used internally to mark elements that are waiting to open.

Only override if integrating with another system.

---

### `registry`

**Type:** `Array<any>`
**Default:** `[]`

Content modules registry.

Used for advanced content injection patterns.

---

## Full WindowManager Options

---

### `zIndexBase`

**Default:** `40`

Starting z-index for windows.

Increase if integrating with high z-index UI systems.

---

### `layoutStabilizationMs`

**Default:** `450`

Wait time before layout calculations after content render.

Increase if using heavy async rendering frameworks.

---

### `cascadeOffset`

**Default:** `30`

Offset (px) between newly opened windows.

```text
Window 1: 100,100
Window 2: 130,130
Window 3: 160,160
```

---

### `cooldownMs`

**Default:** `500`

Minimum time before reopening the same endpoint.

Prevents spam-clicking duplicate windows.

---

### `maxWindows`

**Default:** `10`

Maximum number of concurrent windows.

Set to `Infinity` for unlimited.

---

### `snapGap`

**Default:** `6`

Gap (px) between snapped/tiled windows.

---

### `taskbarHeight`

**Default:** `64`

Height excluded from usable viewport.

Used for snap calculations.

Set to `0` if no taskbar.

---

### `snapThreshold`

**Default:** `30`

Distance from viewport edge required to trigger snap.

Increase for easier snapping.

---

### `dragThreshold`

**Default:** `10`

Minimum cursor travel (px) before drag starts.

Prevents accidental drags.

---

### `resizeDebounceMs`

**Default:** `6`

Debounce delay for window resize calculations.

Higher value = less CPU usage.

---

### `animationDurationMs`

**Default:** `400`

Duration used for state transitions.

Should match your CSS animation durations.

---

### `defaultWidth`

**Default:** `800`

Fallback width when no preset is defined.

---

### `defaultHeight`

**Default:** `600`

Fallback height when no preset is defined.

---

### `minMargin`

**Default:** `10`

Minimum distance between window and viewport edge.

Prevents windows from being partially off-screen.

---

### `edgeDetectionRatio`

**Default:** `0.4`

Defines how much of viewport edges detect side snaps.

Example:

`0.4` → 40% of vertical edge area detects left/right snap.

---

### `scrollRestoreTimeoutMs`

**Default:** `2000`

Maximum wait time for scroll restoration after async content load.

Increase if large content loads slowly.

---

## Example: Custom Tuned Desktop

```js
initNidamApp({
  modalContainer: "#desktop",
  windowManager: {
    zIndexBase: 100,
    maxWindows: 20,
    snapThreshold: 50,
    cascadeOffset: 40,
    taskbarHeight: 48,
    animationDurationMs: 300,
    edgeDetectionRatio: 0.5,
  },
});
```

---

## Performance Tuning Guide

For heavy apps:

Increase:

- `layoutStabilizationMs`
- `resizeDebounceMs`
- `scrollRestoreTimeoutMs`

For snappier UX:

Decrease:

- `dragThreshold`
- `cooldownMs`
- `snapThreshold`

---

## Safe vs Unsafe Changes

#### Safe to Modify

- Snap values
- Drag sensitivity
- Animation timing
- Cascade spacing
- Max windows
- Logging

#### Be Careful With

- `zIndexBase`
- `root`
- `modalContainer`
- `edgeDetectionRatio`

These affect system-level behavior.

---

## Complete Default WindowManager Config

```js
windowManager: {
  zIndexBase: 40,
  layoutStabilizationMs: 450,
  cascadeOffset: 30,
  cooldownMs: 500,
  maxWindows: 10,
  snapGap: 6,
  taskbarHeight: 64,
  snapThreshold: 30,
  dragThreshold: 10,
  resizeDebounceMs: 6,
  animationDurationMs: 400,
  defaultWidth: 800,
  defaultHeight: 600,
  minMargin: 10,
  edgeDetectionRatio: 0.4,
  scrollRestoreTimeoutMs: 2000
}
```
