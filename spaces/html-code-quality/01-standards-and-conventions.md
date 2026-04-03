# 1. Standards and Conventions
Every HTML file must follow these base syntax rules:

* **DOCTYPE:** Always use `<!DOCTYPE html>` to ensure the browser uses standards mode.
* **Language:** Specify the document language using `<html lang="en">`.
* **Encoding:** Use `<meta charset="UTF-8">` as the very first tag in the `<head>`.
* **Naming:** Use lowercase for all tags and attributes. Use **kebab-case** for classes and IDs.
* **Indentation:** Use 2 spaces for nested elements.
* **Attributes:** Always use double quotes for attribute values.
* **Void Elements:** Do not use trailing slashes (e.g., use `<img>`, not `<img />`).

## Base Structure Example
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document Title</title>
</head>
  <body>
  </body>
</html>