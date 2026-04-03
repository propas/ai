# 3. Content Quality & Accessibility
* **Images:** Every `<img>` must have an `alt` attribute. Use `alt=""` for decorative images.
* **Labels:** Every form input must have an associated `<label>` via `for` and `id`.
* **Links:** Use descriptive text like "Read the full case study" instead of "Click Here."
* **Buttons:** Use <button type="button">. Never use a <div> as a button.

## Accessible Form
```html
<form action="/submit-contact" method="POST">
  <fieldset>
    <legend>Contact Information</legend>
    
    <label for="full-name">Full Name:</label>
    <input type="text" id="full-name" name="name" autocomplete="name" required>

    <label for="user-email">Email Address:</label>
    <input type="email" id="user-email" name="email" autocomplete="email" required>
    
    <button type="submit">Submit Request</button>
  </fieldset>
</form>