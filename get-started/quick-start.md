---
icon: rocket
---

# NidamJS Quickstart

Getting started with NidamJS is quick and flexible. Depending on your needs, you can either drop in the script for automatic initialization with default settings or take full manual control to pass a custom configuration object.

This guide covers both approaches.

---

## Method 1: Automatic Initialization (Zero-Config)

For most drop-in use cases, NidamJS can initialize itself automatically when the script loads. It will use its default configuration and automatically bind to elements in the DOM out-of-the-box.

**How to use:**
Simply add the main stylesheet and script to your HTML file. That's it!

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>NidamJS App</title>

    <!-- Include the NidamJS CSS -->
    <link href="/node_modules/nidamjs/dist/nidam.css" rel="stylesheet" />

    <!-- Include the NidamJS ES Module -->
    <script type="module" src="/node_modules/nidamjs/dist/nidam.es.js"></script>
  </head>
  <body>
    <!-- Your NidamJS DOM elements go here -->
    <div nd-toast-stack data-position="bottom-right"></div>
    <div nd-desktop>
      <!-- ... -->
    </div>
  </body>
</html>
```

Under the hood, NidamJS determines if manual control is requested. If it finds no hints of manual initialization, it automatically calls [initNidamApp()](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/bootstrap/AppInitalizer.js#5-20).

---

## Method 2: Custom Configuration (Manual Initialization)

If you need to define custom behaviors (like window thresholds, grid sizing, or custom event registries), you'll want to initialize NidamJS manually.

To do this, NidamJS requires two steps:

1. Signal to the library **not** to auto-initialize by adding the `data-nd-init` attribute to your script tag.
2. Manually import and call [initNidamApp(config)](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/bootstrap/AppInitalizer.js#5-20) inside your script.

### Step 1: Update your HTML

Add the `data-nd-init` attribute to the `script` tag loading your own initialization logic:

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>NidamJS Custom App</title>
    <link href="/node_modules/nidamjs/dist/nidam.css" rel="stylesheet" />

    <!-- Notice the data-nd-init attribute to pause auto-loading! -->
    <script type="module" src="app.js" data-nd-init></script>
  </head>
  <body>
    <!-- ... -->
  </body>
</html>
```

### Step 2: Initialize in JavaScript

Inside your `app.js` file, you can now import [initNidamApp](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/bootstrap/AppInitalizer.js#5-20) (the default export), fetch or define your configuration, and start the application.

```javascript
// app.js

// 1. Import the initializer from the library
import initNidamApp from "nidamjs";

// 2. Define or import your configuration
// You can supply an object directly:
const myConfig = {
  windowManager: {
    zIndexBase: 100,
    resizeDebounceMs: 200,
  },
  refreshTimeout: 5000,
};

// Or import from a JSON file (standard ES module feature):
// import myConfig from "./config.json" with { type: "json" };

// 3. Boot up the app!
const appInstance = initNidamApp(myConfig);

// (Optional) Access internal managers explicitly:
// const winManager = appInstance.getModule("window");
// const delegator = appInstance.getModule("delegator");
```

> [!TIP]
> **Config Formats**: [initNidamApp](file://wsl.localhost/Ubuntu/home/zachari/nidamjs/src/bootstrap/AppInitalizer.js#5-20) accepts either a native Javascript object or a JSON string. If a JSON string is passed, NidamJS will parse it internally, falling back to default settings automatically if it encounters invalid JSON!
