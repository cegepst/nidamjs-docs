---
icon: browser
order: 20
---

# Window Management

Windows are the fundamental building blocks of the NidamJS desktop environment. Unlike traditional components that take a data object, NidamJS windows are defined as **HTML endpoints**.

## Opening a Window

To open a window, use the `open()` method on the `window` module.

```javascript
const wm = app.getModule('window');

// Opens /pages/profile.html via fetch
wm.open('pages/profile.html');
```

### Endpoint Strategies

NidamJS supports two ways to load content:

1.  **Network Fetch**: If `static` mode is disabled (default), NidamJS fetches the endpoint URL. It adds the header `X-Modal-Request: 1`.
2.  **Static Templates**: If `static: true` is set in the Window Manager config, NidamJS looks for a `<template data-route="endpoint">` element in the document.

## Window HTML Structure

For a window to be managed by NidamJS (draggable, closable), it **must** have a specific structure in its HTML response.

```html
<div class="window window-shell animate-appearance">
  <div class="window-bar" data-bar>
    <strong class="window-title">My Application</strong>
    <div class="window-actions">
      <!-- Buttons must have these data attributes -->
      <button class="mini-btn" data-maximize title="Maximize">[]</button>
      <button class="mini-btn" data-close title="Close">x</button>
    </div>
  </div>
  <div class="window-content-scrollable">
    <div class="window-content">
      <!-- Your actual app content here -->
    </div>
  </div>
</div>
```

| Element | Requirement |
| :--- | :--- |
| `.window` | The outermost container. Must be present. |
| `[data-bar]` | The element the user clicks to drag the window. |
| `[data-close]` | Any element that should close the window on click. |
| `[data-maximize]` | Any element that should maximize the window on click. |

## Lifecycle Events

You can listen for window events on the root document.

```javascript
document.addEventListener('window:opened', (e) => {
  const { endpoint, winElement } = e.detail;
  console.log(`Window ${endpoint} is now in the DOM`);
});

document.addEventListener('window:closed', (e) => {
  const { endpoint } = e.detail;
  console.log(`Window ${endpoint} was removed`);
});
```

## Programmatic Controls

Once you have a reference to a window element, you can use the Window Manager methods:

```javascript
const wm = app.getModule('window');
const win = document.querySelector('.window');

wm.focus(win);
wm.toggleMaximize(win);
wm.close(win);
```
