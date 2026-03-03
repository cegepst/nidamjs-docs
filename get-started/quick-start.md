---
icon: rocket
---

# Quick Start

Get your first **NidamJS** desktop environment up and running in just a few minutes.

This guide walks you through:

1. Setting up the desktop container
2. Creating a window
3. Launching your first application

---

## 1. Set Up the Desktop Container

Create a basic HTML file and define the structure of your desktop.

You’ll need:

- A **desktop container**
- A **window target**
- Optional **desktop icons**
- A **taskbar** (recommended for managing windows)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NidamJS Desktop</title>

    <!-- NidamJS default styles -->
    <link rel="stylesheet" href="node_modules/nidamjs/dist/nidam.css" />
  </head>

  <body>
    <!-- Desktop Root -->
    <div nd-desktop>
      <!-- Window Render Target -->
      <div id="target"></div>

      <!-- Desktop Icons Grid -->
      <section nd-icons="8:4">
        <div nd-icon="1:1" nd-id="hello" data-modal="hello">
          <img src="/icons/hello.png" alt="Hello" />
          <span>Hello</span>
        </div>
      </section>

      <!-- Taskbar -->
      <div nd-taskbar>
        <div nd-taskbar-icon data-modal="hello">
          <img src="/icons/hello.png" alt="Hello" />
        </div>
      </div>
    </div>

    <!-- NidamJS module -->
    <script type="module" src="node_modules/nidamjs/dist/nidam.es.js"></script>
  </body>
</html>
```

---

## 2. Create Your First Window

Now define a window component that will be opened when clicking the icon.

```html
<!-- Hello Window -->
<div nd-window nd-window-endpoint="hello">
  <!-- Window Header -->
  <div nd-window-header>
    <span>Hello</span>
    <button nd-window-button="maximize" title="Maximize">[ ]</button>
    <button nd-window-button="close" title="Close">X</button>
  </div>

  <!-- Window Content -->
  <div nd-window-content>
    <p>I am the Hello page content.</p>
  </div>
</div>
```

---

## 3. That’s It 🎉

You now have:

- A working desktop
- A clickable desktop icon
- A taskbar integration
- A fully functional window

You’ve just created your first NidamJS desktop application.

---
