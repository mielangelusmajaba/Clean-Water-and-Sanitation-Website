# Script Explanation — `index.html` (selected <script> tag

## What this file is
A simple, clear, high-school-level explanation of the JavaScript in the `<script>` tag near the end of `index.html`. It explains what each part does (Back to Top button, Navigation active link detection, Accordion), and gives a near line-by-line description so you can follow the code easily.

---

## Quick summary
- Back-to-top code: shows a "Back to Top" button after you scroll down 300px, and scrolls smoothly to the top when clicked.
- Navigation active link detection: watches scrolling and highlights the correct navigation link for the section currently visible on the page.
- Accordion: makes the FAQ items open and close; only one FAQ item is open at a time; it also updates accessibility attributes.

---

## Full script (original, trimmed) and line-by-line explanation
Below is the code grouped into three parts. After each small code block there's a simple explanation of what each statement does.

### 1) Back to Top Button Functionality

```js
// Back to Top Button Functionality
const backToTopButton = document.querySelector('.back-to-top');

window.addEventListener('scroll', () => {
    if (window.scrollY > 300) {
        backToTopButton.classList.add('show');
    } else {
        backToTopButton.classList.remove('show');
    }
});

backToTopButton.addEventListener('click', (e) => {
    e.preventDefault();
    window.scrollTo({
        top: 0,
        behavior: 'smooth'
    });
});
```

Line-by-line (simple):
- `const backToTopButton = document.querySelector('.back-to-top');`
  - Finds the first element in the page that has the class `back-to-top` and saves it to a variable named `backToTopButton`. This is the button that will appear to let users go back to the top.
- `window.addEventListener('scroll', () => { ... });`
  - Tells the browser: "Whenever the page is scrolled, run the code inside the `{ ... }`." This is how the script checks the scroll position continuously.
- `if (window.scrollY > 300) { ... } else { ... }`
  - `window.scrollY` is the vertical scroll position in pixels. If it's greater than 300 pixels, that means the user scrolled down enough.
- `backToTopButton.classList.add('show');`
  - Adds the class `show` to the button when scrolled past 300px. Usually CSS uses that class to make the button visible (for example, by changing `opacity` or `display`).
- `backToTopButton.classList.remove('show');`
  - Removes the `show` class when the user is near the top of the page, hiding the button again.
- `backToTopButton.addEventListener('click', (e) => { ... });`
  - When the user clicks the back-to-top button, run the code inside the handler.
- `e.preventDefault();`
  - Prevents the link's default behavior (jumping to `#` or reloading) so the script can smoothly scroll instead.
- `window.scrollTo({ top: 0, behavior: 'smooth' });`
  - Scrolls the page to the top (`top: 0`) using a smooth animation (`behavior: 'smooth'`). This is what makes the page glide back up instead of jumping.

Why this is useful: users can jump back to the top without manually scrolling, and the button only appears when it would be helpful.

---

### 2) Navigation Active Link Detection

```js
// Navigation Active Link Detection
const navLinks = document.querySelectorAll('.nav-link');
const homeSection = document.getElementById('home');
const aboutSection = document.getElementById('about');
const gallerySection = document.getElementById('gallery');
const faqSection = document.getElementById('faq');
const contactSection = document.getElementById('contact');

function updateActiveLink() {
    let currentSection = 'home';
    const scrollPos = window.scrollY;

    // Define section boundaries
    const homeTop = homeSection.offsetTop;
    const aboutTop = aboutSection.offsetTop;
    const galleryTop = gallerySection.offsetTop;
    const faqTop = faqSection.offsetTop;
    const contactTop = contactSection.offsetTop;

    // Determine active section based on scroll position
    if (scrollPos < aboutTop - 200) {
        currentSection = 'home';
    } else if (scrollPos < galleryTop - 200) {
        currentSection = 'about';
    } else if (scrollPos < faqTop - 200) {
        currentSection = 'gallery';
    } else if (scrollPos < contactTop - 200) {
        currentSection = 'faq';
    } else {
        currentSection = 'contact';
    }

    // Update active class on all nav links
    navLinks.forEach(link => {
        link.classList.remove('active');
        link.removeAttribute('aria-current');

        if (link.getAttribute('data-section') === currentSection) {
            link.classList.add('active');
            link.setAttribute('aria-current', 'page');
        }
    });
}

// Listen to scroll events
window.addEventListener('scroll', updateActiveLink, { passive: true });

// Call once on page load
updateActiveLink();

// Update on link click
navLinks.forEach(link => {
    link.addEventListener('click', (e) => {
        navLinks.forEach(l => l.classList.remove('active'));
        link.classList.add('active');
    });
});
```

Explanation (step-by-step):
- `const navLinks = document.querySelectorAll('.nav-link');`
  - Finds every navigation link (`<a>` elements with class `nav-link`) and stores them in `navLinks` as a list (NodeList).
- `const homeSection = document.getElementById('home');` and similar lines
  - Get references to each of the main page sections by `id` so the script knows where each section starts on the page.
- `function updateActiveLink() { ... }`
  - This is a named function that checks the page scroll position and decides which nav link should be highlighted.
- `const scrollPos = window.scrollY;` and `const ...Top = ...Section.offsetTop;`
  - `scrollPos` is how far the page is scrolled down. Each `offsetTop` is the vertical position of that section from the top of the page. They are used to decide which section is visible.
- The `if/else` chain comparing `scrollPos` with `aboutTop - 200`, `galleryTop - 200`, etc.
  - `- 200` is a buffer so the highlight changes slightly before the section hits the very top of the viewport (this feels more natural). The code sets `currentSection` to `'home'`, `'about'`, `'gallery'`, `'faq'`, or `'contact'` depending on where the page is.
- `navLinks.forEach(link => { ... })` loop
  - First removes the `active` class and `aria-current` from all nav links.
  - Then finds the link which has a `data-section` attribute matching `currentSection` and adds the `active` class and sets `aria-current='page'` for accessibility (this tells screen readers which link is the current page/section).
- `window.addEventListener('scroll', updateActiveLink, { passive: true });`
  - Adds the `updateActiveLink` function to run when the user scrolls. `passive: true` hints to the browser that the handler won't call `preventDefault()`, improving performance.
- `updateActiveLink();`
  - Runs the function once on page load so the correct link is highlighted immediately (useful if the page opened at an anchor or was reloaded partway down).
- The `navLinks.forEach(...)` click handler
  - When a nav link is clicked, it removes `active` from all links and adds it to the clicked link. This visually updates the nav right away (in addition to scroll-based updates when the page moves).

Why this is useful: it gives users a visual cue in the navigation that shows which section they're reading, improving orientation on the page.

---

### 3) Accordion Functionality (FAQ)

```js
// Accordion Functionality
const accordionHeaders = document.querySelectorAll('.accordion-header');

accordionHeaders.forEach(header => {
    header.addEventListener('click', () => {
        const isExpanded = header.getAttribute('aria-expanded') === 'true';
        const content = document.getElementById(header.getAttribute('aria-controls'));

        // Close all other accordion items
        accordionHeaders.forEach(otherHeader => {
            if (otherHeader !== header) {
                otherHeader.setAttribute('aria-expanded', 'false');
                const otherContent = document.getElementById(otherHeader.getAttribute('aria-controls'));
                otherContent.setAttribute('aria-hidden', 'true');
                otherContent.setAttribute('hidden', '');
            }
        });

        // Toggle current accordion item
        if (isExpanded) {
            header.setAttribute('aria-expanded', 'false');
            content.setAttribute('aria-hidden', 'true');
            content.setAttribute('hidden', '');
        } else {
            header.setAttribute('aria-expanded', 'true');
            content.setAttribute('aria-hidden', 'false');
            content.removeAttribute('hidden');
        }
    });
});
```

Explanation (step-by-step):
- `const accordionHeaders = document.querySelectorAll('.accordion-header');`
  - Finds all the accordion header buttons (the clickable parts that open/close each FAQ item).
- `accordionHeaders.forEach(header => { header.addEventListener('click', () => { ... }); });`
  - For each header, attach a click listener that runs when the header is clicked.
- `const isExpanded = header.getAttribute('aria-expanded') === 'true';`
  - Reads the `aria-expanded` attribute (which is `true` or `false`) to know if this accordion item is already open.
- `const content = document.getElementById(header.getAttribute('aria-controls'));`
  - Finds the content panel associated with the header using `aria-controls`. `aria-controls` holds the id of the content element.
- The inner loop `accordionHeaders.forEach(otherHeader => { ... })`
  - Closes all other accordion items before opening the clicked one. This makes sure only a single FAQ is open at a time.
  - For each other header, it sets `aria-expanded=