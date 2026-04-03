# 4. Performance & Speed
* **Resource Loading:** CSS in `<head>`, JavaScript at the end of `<body>` (or use `defer`).
* **Media:** Use `loading="lazy"` for images and iframes below the fold.
* **DOM Size:** Keep the DOM depth shallow to speed up rendering.
* **Non-blocking JS:** Use the defer or async attribute for scripts.
* **Image Sizing:** Always define width and height to prevent layout shifts.

## Performance Implementation
```html
<head>
  <link rel="preconnect" href="[https://fonts.googleapis.com](https://fonts.googleapis.com)">
  <link rel="stylesheet" href="styles.min.css">
</head>
<body>
  <img src="hero-banner.jpg" width="1200" height="600" alt="Team collaborating" fetchpriority="high">

  <img src="feature-graphic.png" width="400" height="300" alt="Graphic description" loading="lazy">

  <script src="main.js" defer></script>
</body>