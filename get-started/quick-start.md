---
icon: rocket
---

# Quickstart

Getting started with NidamJS is quick and flexible. Depending on your needs, you can either drop in the script for automatic initialization with default settings or take full manual control to pass a [[configuration|custom configuration]] object. 

**Required:** Add the NidamJS `<script>`, `<link>`, and `<div nd-desktop>` to your page.

Everything else inside `<div nd-desktop>` is optional — add only the components you need (icons, taskbar, toasts).

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>NidamJS App</title>
    <link href="/node_modules/nidamjs/dist/nidam.css" rel="stylesheet" />
    <script type="module" src="/node_modules/nidamjs/dist/nidam.es.js"></script>
  </head>
  <body>    
    <div nd-desktop>
        
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
      
      
      <!-- Toasts notification -->
      <div nd-toast-stack data-position="bottom-right"></div>
    </div>
  </body>
</html>
```
