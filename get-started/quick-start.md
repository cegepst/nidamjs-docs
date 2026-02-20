---
icon: rocket
---

# Quick Start

Get your first NidamJS desktop environment running in minutes.

## Basic Usage

### 1. Add the container

Create an HTML file and add a container element where the windows will be rendered. You also need to define a taskbar if you want application management.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NidamJS Desktop</title>
    <!-- Default styles from the package -->
    <link rel="stylesheet" href="node_modules/nidamjs/dist/nidam.css" />
  </head>
  <body>
    <div nd-desktop>
      <!-- Target for windows -->
      <div id="target"></div>

      <!-- Taskbar -->
      <div nd-taskbar tb-position="bottom">
        <button tb-icon data-modal="app-one">App 1</button>
      </div>
    </div>

    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

### 2. Initialize the App

Import `createNidamApp` from the library and initialize it pointing to your target container.

```javascript
import { createNidamApp } from "nidamjs";

const app = createNidamApp({
  modalContainer: "#target",
  windowManager: {
    config: {
      maxWindows: 5,
    }
  }
});

app.initialize();
```

### 3. Open a Window

NidamJS works with **endpoints**. An endpoint can be a URL that returns window HTML, or a reference to a local template.

```javascript
// Get the window manager module
const wm = app.getModule('window');

// Open a window by its endpoint/URL
wm.open('pages/welcome.html');
```

## Static Template Usage

For static sites, you can define windows inside `<template>` tags with a `data-route`.

```html
<template data-route="my-app">
  <div class="window">
    <div class="window-bar" data-bar>
      <strong>My App</strong>
      <button data-close>x</button>
    </div>
    <div class="window-content">Hello World</div>
  </div>
</template>
```

Then open it using the route name:
```javascript
wm.open('my-app');
```
