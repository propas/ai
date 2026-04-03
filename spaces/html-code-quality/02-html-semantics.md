# 2. HTML Semantics
Use elements for their intended meaning to improve SEO and accessibility.

* **Structure:** Use `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, and `<footer>`.
* **Headings:** Use only one `<h1>` per page. Follow a strict descending order (h1 > h2 > h3).
* **Main Content:** The `<main>` tag must only appear once per page.
* **Interactive Elements:** * Use `<a>` for navigation to a different URL.
    * Use `<button>` for on-page actions or form submissions.

## Structure layout
```html
<header>
  <nav aria-label="Main Navigation">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>

<main id="main-content">
  <section id="hero">
    <h1>Building Accessible Futures</h1>
    <p>A deep dive into semantic architecture.</p>
  </section>

  <article id="blog-post">
    <h2>Understanding HTML5 Sections</h2>
    <p>Semantic tags improve accessibility and SEO rankings...</p>
  </article>

  <aside>
    <h3>Related Links</h3>
    <ul>
      <li><a href="/seo-tips">SEO Best Practices</a></li>
    </ul>
  </aside>
</main>

<footer>
  <p>&copy; 2026 Developer Space. All rights reserved.</p>
</footer>