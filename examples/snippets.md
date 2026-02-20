---
icon: code
order: 20
---

# Code Snippets

Common patterns for building with NidamJS.

## Trigger Window from Button

The most common way to open a window is via a button in your HTML.

```html
<button data-modal="api/stats.html" class="btn">
  View Stats
</button>
```

## Programmatic Open with Data

Since windows are fetched as HTML, you usually handle data on the server. However, you can use `window:opened` to inject data after loading.

```javascript
const wm = app.getModule('window');

wm.open('templates/user-profile').then((win) => {
  // Inject data into the window body
  win.querySelector('.username').textContent = 'John Doe';
});
```

## Close Topmost Window (ESC Key)

NidamJS handles the ESC key by default, but you can trigger it manually.

```javascript
import { Lifecycle } from "nidamjs";

document.addEventListener('keydown', (e) => {
  if (e.key === 'q' && e.ctrlKey) {
    const wm = app.getModule('window');
    // Internal logic to find topmost window
    const topWin = [...wm._windows.values()].pop(); 
    if (topWin) wm.close(topWin);
  }
});
```

## Integrating Charts (Chart.js)

Initialize third-party libraries inside a window after it opens.

```javascript
document.addEventListener('window:opened', (e) => {
  if (e.detail.endpoint === 'dashboard') {
    const ctx = e.detail.winElement.querySelector('#myChart');
    new Chart(ctx, {
      type: 'bar',
      data: { ... }
    });
  }
});
```

## Custom App Registry

Use the `registry` to automatically bind logic to specific elements whenever a new window is opened.

```javascript
const app = createNidamApp({
  registry: [
    {
      selector: '[data-custom-widget]',
      init: (el, delegator, modules) => {
        el.addEventListener('click', () => alert('Widget clicked!'));
        return { type: 'custom-widget' };
      }
    }
  ]
});
```
