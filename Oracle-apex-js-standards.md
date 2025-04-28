# Oracle APEX JavaScript & jQuery Coding Standards Cookbook

This document provides a comprehensive guide to JavaScript and jQuery coding standards and best practices for developers working in Oracle APEX environments. The goal is to maintain consistency, readability, maintainability, and security across all codebases.

---

## 📁 Project Structure

```
/web/
  /js/
    custom.js
    utils.js
  /css/
    styles.css
  /images/
    user-image.png
```

Place custom JS and CSS files in the `web` folder and reference them in APEX via shared components.

---

## General Coding Guidelines

- Always use **`let`** or **`const`** instead of `var`.
- Use **strict mode** at the beginning of each file.
  ```js
  'use strict';
  ```
- Write **modular, reusable functions**.
- Follow **camelCase** for variable and function names.
- Use **descriptive names** for variables and functions.
- End all statements with **semicolons**.
- Avoid global variables; use the APEX `namespace` pattern if needed.

---

## JavaScript Best Practices

### Naming Conventions
- `camelCase` for variables and functions.
- `PascalCase` for constructors/classes.
- Use all uppercase with underscores for constants: `MAX_LIMIT`.

### Example
```js
const MAX_RETRIES = 3;

function fetchDataFromServer(endpointUrl) {
  // logic here
}
```

### Functions
- One function = one purpose.
- Use arrow functions for simple operations.
```js
const calculateTotal = (qty, price) => qty * price;
```

---

## jQuery Practices in Oracle APEX

### Selecting Elements
- Use `#STATIC_ID` selectors over classes when targeting APEX components.
```js
$('#P1_CUSTOM_BUTTON').on('click', function() {
  alert('Button clicked!');
});
```

### Event Binding
- Prefer `on()` for event delegation.
- Use `.off()` before `.on()` to avoid duplicate bindings.
```js
$('#P1_CUSTOM_BUTTON').off('click').on('click', handleClick);
```

### DOM Ready
```js
$(function() {
  // Safe to access DOM elements
});
```

---

## APEX-Specific Guidelines

- Use `apex.item()` for accessing page items.
```js
const val = apex.item('P1_NAME').getValue();
```

- Use `apex.server.process` for AJAX callbacks.
```js
apex.server.process("MY_PROCESS", {
  x01: 'param1',
  pageItems: '#P1_ITEM1,#P1_ITEM2'
}, {
  success: function(pData) {
    console.log(pData);
  },
  error: function(request, status, error) {
    console.error(error);
  }
});
```

---

## 📦 Managing JavaScript & jQuery Versions Locally (Strict Security - No External References)

- **Always download and host** the latest **Long Term Support (LTS)** versions of libraries like **jQuery** **locally** inside your project — **never load from public URLs or CDNs**.
- **Reason**: External resources can be changed, hijacked, or go offline, posing serious **security risks** to your APEX applications.

### Folder Structure Example
```
/web/
  /js/
    jquery-3.7.1.min.js
    custom.js
    utils.js
```

### How to Reference Local JavaScript Files Securely
- Upload libraries into **Shared Components → Static Application Files**, or place them directly inside your server’s `/web/js/` folder.
- Reference them **only** through internal APEX file URLs:
```html
<script src="#APP_IMAGES#js/jquery-3.7.1.min.js"></script>
<script src="#APP_IMAGES#js/custom.js"></script>
```
*(Assuming your `/web/` path is exposed via `#APP_IMAGES#` substitution string.)*

### Strict Security Guidelines
- **No external `<script>` tags** from CDNs like Google, jsDelivr, unpkg, etc.
- **No runtime external downloads** of any scripts.
- Validate file integrity yourself before adding any third-party library.
- Document the library source, version, and checksum (if applicable) for future audits.

### Version Update Policy
- Update libraries **only** after downloading new LTS versions manually from the official source.
- Review change logs and test in a safe environment before promoting to production.
- Use clear versioned filenames, e.g., `jquery-3.7.1.min.js`, to avoid accidental overwrites.

---

## Debugging & Logging

- Use `console.log()` during development.
- Remove all debugging logs before production.
- Use `apex.debug()` for APEX-specific logging.

---

## Avoid

- Inline JavaScript in HTML attributes (e.g., `onclick`).
- Direct DOM manipulation unless necessary.
- Using `eval()`.
- Modifying built-in prototypes.

---

## Security

- Escape dynamic HTML content using `apex.util.escapeHTML()`.
- Use APEX APIs for item values to prevent injection.
- Sanitize all inputs if doing manual DOM updates.

---

## HTML & CSS Coding Standards

### HTML
- Use semantic HTML elements (`<section>`, `<article>`, `<header>`, `<footer>`, etc.).
- Use lowercase for all element names and attributes.
- Use double quotes for attribute values.
- Self-close void elements: `<br />`, `<img />`.
- Indent nested elements with 2 spaces.
- Avoid inline styles.

#### Example
```html
<section class="user-profile">
  <h2>User Info</h2>
  <p id="userEmail">john.doe@example.com</p>
</section>
```

### CSS
- Use external CSS files; avoid inline or embedded styles.
- Use BEM (Block Element Modifier) naming convention:
  - `.block`, `.block__element`, `.block--modifier`
- Group related styles together.
- Keep CSS organized by layout sections or components.
- Use variables and consistent units (`rem`, `%`, `em` over `px` where appropriate).

#### Example
```css
.user-profile {
  padding: 1rem;
  background-color: #f5f5f5;
}

.user-profile__image {
  width: 100px;
  height: auto;
}

.user-profile--highlighted {
  border: 2px solid #007cc2;
}
```

---

## Version Control Tips

- Commit small, well-defined changes.
- Include comments in commit messages.
- Use meaningful branch names: `feature/button-handler`, `bugfix/item-null-check`.

---

## Sample Template for JS File
```js
/**
 * File: custom.js
 * Author: Your Name
 * Description: Custom JS for APEX app [App Name]
 */

'use strict';

(function($) {
  const myApp = {
    init: function() {
      this.bindEvents();
    },

    bindEvents: function() {
      $('#P1_CUSTOM_BUTTON').off('click').on('click', this.onCustomButtonClick);
    },

    onCustomButtonClick: function() {
      const name = apex.item('P1_NAME').getValue();
      alert(`Hello, ${name}`);
    }
  };

  $(document).ready(function() {
    myApp.init();
  });
})(jQuery);
```

---

## References

- [Oracle APEX JavaScript API (Internal Use Only)]
- [jQuery API Documentation (Use downloaded copy)]
- [MDN JavaScript Guide (For offline learning resources)]

---
