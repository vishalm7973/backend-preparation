# CSS Interview Questions and Answers

## 1. What is CSS?

CSS (Cascading Style Sheets) is used to **style and layout HTML elements**.

    HTML → Structure
    CSS  → Style/Layout
    JS   → Behavior

---

## 2. What is the CSS Box Model?

Every element is made of:

    Content
      ↓
    Padding
      ↓
    Border
      ↓
    Margin

### `box-sizing`

    content-box → width applies to content
    border-box  → width includes content + padding + border

Usually:

    box-sizing: border-box;

---

## 3. What is CSS Specificity?

Specificity decides **which CSS rule wins** when multiple rules target the same element.

Basic order:

    Inline styles
        ↓
    ID
        ↓
    Class / attribute / pseudo-class
        ↓
    Element

If specificity is the same, the **later rule wins**.

---

## 4. `display: none` vs `visibility: hidden` vs `opacity: 0`

    display: none
    → Element removed from layout

    visibility: hidden
    → Element hidden but space remains

    opacity: 0
    → Element becomes transparent
    → It can still receive interactions

---

## 5. What does `position` mean?

    static   → Normal position
    relative → Can be moved from its normal position
    absolute → Removed from normal flow
    fixed    → Positioned relative to viewport
    sticky   → Sticks while scrolling after reaching a threshold

Important:

    absolute
    → Usually positioned relative to the nearest positioned ancestor.

---

## 6. Flexbox vs Grid

    Flexbox → One-dimensional layout
              (row OR column)

    Grid     → Two-dimensional layout
              (rows AND columns)

Use Flexbox for arranging items in a row/column.

Use Grid for larger page layouts.

---

## 7. What is `z-index`?

`z-index` controls which element appears **in front of another**.

    .modal {
      z-index: 1000;
    }

A large `z-index` does not always win because **stacking contexts** can affect it.

---

## 8. `em` vs `rem`

    rem → Relative to root font size
    em  → Relative to the element's font size

Example:

    html {
      font-size: 16px;
    }

    2rem = 32px

---

## 9. Pseudo-class vs Pseudo-element

### Pseudo-class

Represents a **state**.

    :hover
    :focus
    :active
    :first-child

### Pseudo-element

Targets a **part of an element** or generated content.

    ::before
    ::after
    ::first-line

---

## 10. How do you make a website responsive?

Use:

- Flexible layouts
- Flexible units
- Flexbox/Grid
- Media queries

Example:

    @media (max-width: 600px) {
      .container {
        flex-direction: column;
      }
    }

---

## 11. What is the difference between Margin and Padding?

    Margin  → Space outside the element
    Padding → Space inside the element

---

## 12. How do you center an element?

Using Flexbox:

    .container {
      display: flex;
      justify-content: center;
      align-items: center;
    }

    justify-content → Main axis
    align-items     → Cross axis

---

## 13. What is `overflow`?

Controls what happens when content is larger than its container.

    overflow: visible;
    overflow: hidden;
    overflow: scroll;
    overflow: auto;

---

## 14. What is `float`?

`float` moves an element to the left or right and allows surrounding content to wrap around it.

It was commonly used for layouts before Flexbox and Grid.

---

## 15. What is `display: inline-block`?

It behaves like an inline element but allows setting:

    width
    height
    padding
    margin

---

## 16. `width: 100%` vs `width: 100vw`

    100%  → Based on the parent's width
    100vw → Based on viewport width

`100vw` can sometimes cause horizontal scrolling because it includes the viewport width including scrollbar space.

---

## 17. What are CSS Media Queries?

Media queries apply CSS based on conditions such as screen width.

    @media (max-width: 768px) {
      .menu {
        display: none;
      }
    }

Commonly used for **responsive design**.

---

## 18. What is `!important`?

It gives a declaration very high priority in the cascade.

    color: red !important;

Avoid using it unnecessarily because it makes CSS harder to maintain and override.

---

## 19. What is the difference between `relative` and `absolute`?

    relative
    → Element remains in normal flow

    absolute
    → Element is removed from normal flow

Example:

    .parent {
      position: relative;
    }

    .child {
      position: absolute;
      top: 0;
      right: 0;
    }

---

## 20. What is CSS inheritance?

Some CSS properties are automatically inherited from the parent.

Example:

    body {
      color: blue;
    }

Child text will usually inherit the blue color unless another rule overrides it.