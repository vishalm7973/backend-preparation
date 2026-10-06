# HTML Interview Questions and Answers

## 1. What is HTML?

HTML (HyperText Markup Language) is used to **structure content** on web pages.

    HTML → Structure
    CSS  → Styling
    JS   → Behavior

---

## 2. What is Semantic HTML?

Semantic HTML uses elements that clearly describe their purpose.

Examples:

    <header>
    <nav>
    <main>
    <section>
    <article>
    <footer>

Benefits:
- Better accessibility
- Better SEO
- Cleaner code

---

## 3. `<div>` vs `<section>`

    <div>     → Generic container
    <section> → Groups related content

Use `<section>` when the content represents a meaningful section of the page.

---

## 4. What is `<!DOCTYPE html>`?

It tells the browser to use **modern HTML standards mode** instead of quirks mode.

    <!DOCTYPE html>

**Quirks mode** is an old browser compatibility mode where browsers behave like older browsers to support outdated websites.

### Easy Way to Remember

    Standards Mode → Modern HTML/CSS behavior
    Quirks Mode   → Old/legacy browser behavior

---

## 5. Why use the viewport meta tag?

It helps pages work correctly on mobile devices.

    <meta
      name="viewport"
      content="width=device-width, initial-scale=1"
    >

---

## 6. What is the `alt` attribute?

`alt` provides alternative text for images.

    <img src="profile.jpg" alt="User profile">

It helps:
- Screen readers
- Accessibility
- When the image cannot load

For decorative images:

    alt=""

---

## 7. Why use `<label>` with form inputs?

A label gives an input an accessible name and makes it easier to click.

    <label for="email">Email</label>
    <input id="email" type="email">

---

## 8. GET vs POST

    GET  → Data usually goes in URL
    POST → Data usually goes in request body

GET is commonly used for fetching/searching.

POST is commonly used for creating or changing data.

HTTPS is what protects data during transmission, not GET/POST itself.

---

## 9. `async` vs `defer`

For external classic scripts:

    async → Downloads and executes as soon as ready
    defer → Executes after HTML parsing

`async` does not guarantee execution order.

`defer` maintains the order of scripts.

Example:

    <script src="a.js" defer></script>
    <script src="b.js" defer></script>

`a.js` runs before `b.js`.

---

## 10. Block vs Inline

Common default behavior:

    Block  → Starts on a new line
    Inline → Takes only required space

Examples:

    Block  → div, p, h1
    Inline → span, a, strong

CSS can change an element's display behavior.

---

## 11. What is the difference between `id` and `class`?

    id    → Identifies one specific element
    class → Can be used by multiple elements

Example:

    <div id="header" class="container"></div>

---

## 12. What are HTML attributes?

Attributes provide additional information about an element.

    <input type="text" placeholder="Enter name">

Here:

    type
    placeholder

are attributes.

---

## 13. What is the difference between `<strong>` and `<b>`?

    <strong> → Indicates importance
    <b>      → Mainly visual bold text

Prefer `<strong>` when the content is semantically important.

---

## 14. What is the difference between `<em>` and `<i>`?

    <em> → Emphasis
    <i>  → Alternate voice/style

Use semantic elements when their meaning matches the content.

---

## 15. What is the difference between `<a>` and `<button>`?

    <a>      → Navigation / link
    <button> → Performs an action

Example:

    <a href="/about">About</a>

    <button onClick={handleSave}>Save</button>

---

## 16. What are void elements?

Elements that do not have a closing tag.

Examples:

    <img>
    <input>
    <br>
    <hr>
    <meta>
    <link>

---

## 17. What is HTML5?

HTML5 is the modern HTML standard that introduced/improved features such as:

- Semantic elements
- Audio/video
- Better forms
- Canvas
- Improved browser APIs

---

## 18. What is accessibility in HTML?

Making websites usable by people with different abilities.

Important practices:

    Semantic HTML
    alt text
    labels for inputs
    Keyboard navigation
    Proper heading structure

---

## 19. What is SEO-friendly HTML?

Use meaningful HTML structure so search engines can understand the page.

Examples:

    <title>
    <meta name="description">
    <h1>
    <main>
    <article>
    <nav>

---

## 20. What is the difference between `localStorage`, `sessionStorage`, and cookies?

    localStorage
    → Persists until manually cleared

    sessionStorage
    → Usually lasts for the current browser tab/session

    Cookies
    → Can be sent automatically with HTTP requests
    → Can have expiration and security attributes