# Clean Water & Sanitation Static Site – Code Explanation

This document explains every major part of the provided `index.html` and `styles.css` files. It focuses on:

- Overall page structure (sections and layout)
- Key HTML elements and their roles
- CSS variables and the `:root` selector
- Pseudo-elements like `::before`
- Responsive design with `@media` queries
- The mobile navigation toggle implemented only with HTML and CSS (no JavaScript)

---

## 1. HTML Structure Overview (`index.html`)

The HTML document is a static, multi-section webpage about SDG 6 (Clean Water and Sanitation). It is divided into the following logical sections:

1. `<head>` with metadata, fonts, and stylesheet links
2. `<header>` – fixed navigation bar
3. Hero section (main banner)
4. About section
5. Key focus areas (cards)
6. Impact gallery
7. Contact section (info + form)
8. Footer

### 1.1 Document Head

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SDG 6 - Clean Water and Sanitation</title>
    <link rel="stylesheet" href="styles.css">
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700&family=Open+Sans:wght@400;500&display=swap" rel="stylesheet">
</head>
```

- `<!DOCTYPE html>`: Declares HTML5.
- `<html lang="en">`: Sets document language to English.
- `<meta charset="UTF-8">`: Ensures proper character encoding.
- `<meta name="viewport" ...>`: Enables responsive scaling on mobile devices.
- `<title>`: Defines the browser tab title.
- `<link rel="stylesheet" href="styles.css">`: Includes the external CSS file.
- Google Fonts `<link>`: Loads the `Montserrat` and `Open Sans` fonts used in the CSS.

---

## 2. Header and Navigation

```html
<header class="header" role="banner">
    <div class="container">
        <a href="#" class="logo" aria-label="SDG 6 Initiative Home">
            <div class="logo-placeholder">Logo</div>
        </a>
        <nav class="nav" role="navigation" aria-label="Main navigation">
            <input type="checkbox" id="nav-toggle-checkbox" class="nav-toggle-checkbox">
            <label for="nav-toggle-checkbox" class="nav-toggle" aria-label="Toggle navigation">
                <span></span>
                <span></span>
                <span></span>
            </label>
            <ul class="nav-menu" role="list">
                <li><a href="#" class="active" aria-current="page">Home</a></li>
                <li><a href="#about">About Us</a></li>
                <li><a href="#gallery">Gallery</a></li>
                <li><a href="#contact">Contact Us</a></li>
            </ul>
        </nav>
    </div>
</header>
```

### 2.1 Semantics and Accessibility

- `<header class="header" role="banner">`: Defines the top banner area with navigation.
- `.container`: A reusable wrapper class that centers content and sets max-width.
- `<a class="logo">`: Clickable logo linking to the top of the page.
- `aria-label` attributes: Provide descriptive labels for screen readers.
- `role="navigation"` and `aria-label="Main navigation"`: Clearly identify the nav region.
- `aria-current="page"` on the "Home" link: Indicates the current page.

### 2.2 Mobile Navigation Toggle (Checkbox + Label Pattern)

The navigation is made responsive and interactive **without JavaScript**, using a checkbox and label:

```html
<input type="checkbox" id="nav-toggle-checkbox" class="nav-toggle-checkbox">
<label for="nav-toggle-checkbox" class="nav-toggle" aria-label="Toggle navigation">
    <span></span>
    <span></span>
    <span></span>
</label>
<ul class="nav-menu" role="list">...</ul>
```

- `input[type="checkbox"]`: Hidden on larger screens; used as a state toggle.
- `id="nav-toggle-checkbox"`: Connects the input to the label via `for`.
- `<label for="nav-toggle-checkbox">`: Clicking this label toggles the checkbox.
- Three `<span>` elements: Visually represent the hamburger icon (three horizontal bars).
- `.nav-menu`: The actual menu that is shown/hidden with CSS based on the checkbox state.

In CSS, the rule:

```css
.nav-toggle-checkbox:checked ~ .nav-menu { ... }
```

is used to show the menu when the checkbox is checked. This approach replaces the need for JavaScript.

(Details on the CSS side are explained in section **4.3 Mobile Navigation Styles**.)

---

## 3. Main Content Sections

### 3.1 Hero Section

```html
<section class="hero" role="banner" aria-labelledby="hero-heading">
    <div class="hero-overlay">
        <div class="container">
            <div class="hero-content">
                <h1 id="hero-heading">Clean Water & Sanitation for All</h1>
                <p class="hero-text">Ensuring availability ... community action.</p>
                <ul class="benefits-list" role="list" aria-label="Key benefits">
                    <li>✓ Sustainable Water Management</li>
                    <li>✓ Improved Sanitation Systems</li>
                    <li>✓ Water Resource Protection</li>
                </ul>
                <a href="#learn-more" class="btn btn-primary">Learn More</a>
                <div class="trust-indicators" role="group" aria-label="Impact statistics">
                    <div class="trust-item">
                        <span class="trust-number" aria-label="Over 1 million">1M+</span>
                        <span class="trust-text">People Impacted</span>
                    </div>
                    <div class="trust-item">
                        <span class="trust-number" aria-label="Over 100">100+</span>
                        <span class="trust-text">Projects Completed</span>
                    </div>
                    <div class="trust-item">
                        <span class="trust-number" aria-label="Over 50">50+</span>
                        <span class="trust-text">Countries Reached</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

- `<section class="hero">`: Main banner area (full-height, background image + gradient overlay via CSS).
- `role="banner"` and `aria-labelledby="hero-heading"`: Accessibility for screen readers.
- `.hero-overlay`: An internal wrapper used to position content and overlay the gradient.
- `.hero-content`: Contains the main hero text.
- `.benefits-list`: A bullet list of key benefits, styled without default list markers.
- `.btn btn-primary`: Primary call-to-action button.
- `.trust-indicators`: A flex container showing impact stats.

### 3.2 About Section

```html
<section id="about" class="section" aria-labelledby="about-heading">
    <div class="container">
        <h2 id="about-heading">About SDG 6</h2>
        <div class="content-grid">
            <div class="content-text flow">
                <p>SDG 6 aims to ensure access to water ...</p>
                <a href="#" class="btn btn-secondary">Our Mission</a>
            </div>
            <div class="content-image">
                <div class="img-placeholder" role="img" aria-label="Water resources visualization">Water Resource Image</div>
            </div>
        </div>
    </div>
</section>
```

- `id="about"`: Allows navigation via `#about` links.
- `.section`: Shared padding and vertical spacing.
- `.content-grid`: Two-column layout (text + image) using CSS Grid; collapses on smaller screens.
- `.flow`: Utility class adding consistent vertical spacing between children.
- `.img-placeholder`: A styled box that stands in for a real image.

### 3.3 Key Focus Areas (Cards)

```html
<section class="section bg-light" aria-labelledby="focus-areas-heading">
    <div class="container">
        <h2 id="focus-areas-heading">Key Focus Areas</h2>
        <div class="cards-grid" role="list">
            <article class="card" role="listitem"> ... </article>
            <article class="card" role="listitem"> ... </article>
            <article class="card" role="listitem"> ... </article>
        </div>
    </div>
</section>
```

- `bg-light`: Applies a light background color.
- `.cards-grid`: Responsive grid for the cards.
- `<article class="card">`: Semantically, each card is an independent piece of content.
- `.card-image`: Placeholder block for a card image.

### 3.4 Gallery Section

```html
<section id="gallery" class="section" aria-labelledby="gallery-heading">
    <div class="container">
        <h2 id="gallery-heading">Our Impact Gallery</h2>
        <div class="gallery-grid" role="list" aria-label="Impact photo gallery">
            <div class="gallery-item" role="img" aria-label="Gallery image 1">Image 1</div>
            <!-- more items -->
        </div>
    </div>
</section>
```

- `.gallery-grid`: CSS Grid creates a flexible image gallery.
- `.gallery-item`: Each item is a box that could contain an image; uses an `aspect-ratio` and hover scale effect.

### 3.5 Contact Section

```html
<section id="contact" class="section bg-dark" aria-labelledby="contact-heading">
    <div class="container">
        <h2 id="contact-heading">Contact Us</h2>
        <div class="contact-content">
            <div class="contact-info"> ... </div>
            <div class="contact-form">
                <form aria-label="Contact form">
                    <input type="text" id="name" name="name" placeholder="Your Name" required aria-label="Your name">
                    <input type="email" id="email" name="email" placeholder="Your Email" required aria-label="Your email address">
                    <textarea id="message" name="message" placeholder="Your Message" required aria-label="Your message"></textarea>
                    <button type="submit" class="btn btn-primary">Send Message</button>
                </form>
            </div>
        </div>
    </div>
</section>
```

- `bg-dark`: Dark background with light text.
- `.contact-content`: Two-column grid (info + form) that stacks on smaller screens.
- `<form>` with `input` and `textarea`: Collects user data (no JS; submission behavior would be default or configured later).
- `required`: Built-in HTML validation on fields.

### 3.6 Footer

```html
<footer class="footer">
    <div class="container">
        <div class="footer-content">
            <div class="footer-section"> ... </div>
            <div class="footer-section"> ... </div>
            <div class="footer-section"> ... </div>
        </div>
        <div class="footer-bottom">
            <p>&copy; 2025 SDG 6 Initiative. All rights reserved.</p>
        </div>
    </div>
</footer>
```

- `.footer`: Full-width footer with background color and text.
- `.footer-content`: Grid layout for footer columns.
- `.footer-bottom`: Single line of copyright text.

---

## 4. CSS Overview (`styles.css`)

### 4.1 `:root` and CSS Custom Properties (Variables)

At the top of `styles.css` is the `:root` selector:

```css
:root {
    --color-primary-dark: #22577A;
    --color-primary: #38A3A5;
    --color-secondary: #57CC99;
    --color-accent-light: #80ED99;
    --color-accent-lighter: #C7F9CC;
    /* ... more variables ... */
}
```

- `:root` targets the root element of the document (the `<html>` element).
- Defining variables in `:root` makes them globally available throughout the stylesheet.
- Variables are written as `--name: value;` and referenced with `var(--name)`.

**Purpose of `:root` in this stylesheet:**

- Centralizes design tokens (colors, font sizes, spacing, breakpoints).
- Makes it easy to change the theme (e.g., adjust colors) from one place.
- Ensures consistent spacing and typography across the site.

Examples of variable usage:

```css
body {
    font-family: var(--font-body);
    font-size: var(--font-size-base);
    color: var(--color-medium-gray);
}

.hero::before {
    background: linear-gradient(rgba(56, 163, 165), rgba(34, 87, 122));
}
```

### 4.2 Base Styles and Typography

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

- Resets default margins and paddings.
- `box-sizing: border-box;` ensures padding and borders are included in width/height calculations.

```css
body {
    font-family: var(--font-body);
    font-size: var(--font-size-base);
    line-height: 1.6;
    color: var(--color-medium-gray);
    overflow-x: hidden;
}
```

- Sets global font, base size (fluid via `clamp`), line height, and text color.
- `overflow-x: hidden;` prevents horizontal scrollbars from slight overflows.

Headings use the `var(--font-heading)` font and fluid font sizes:

```css
h1 { font-size: var(--font-size-h1); }
h2 { font-size: var(--font-size-h2); }
```

### 4.3 Layout: Containers and Header

```css
.container {
    max-width: var(--container-max);
    margin: 0 auto;
    padding-inline: var(--container-padding);
    width: 100%;
}
```

- Centers content and limits width for readability.
- `padding-inline`: Horizontal padding responsive via `var(--container-padding)`.

```css
.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: clamp(64px, 8vh, 80px);
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    z-index: 1000;
}
```

- `position: fixed;`: Keeps header pinned to the top when scrolling.
- `backdrop-filter: blur(10px);`: Blurs content behind the header for a glass effect.
- `z-index: 1000;`: Ensures header appears above other elements.

### 4.4 Navigation Styles (Desktop)

```css
.header .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.nav-menu {
    display: flex;
    list-style: none;
    gap: clamp(16px, 2vw, 24px);
    align-items: center;
}

.nav-menu a {
    text-decoration: none;
    color: var(--color-navy-blue);
    font-weight: 500;
    font-size: clamp(14px, 1.2vw, 16px);
}

.nav-menu a:hover,
.nav-menu a.active {
    color: var(--color-teal);
}
```

- Flexbox arranges logo and nav links.
- `.nav-menu`: Horizontal list of links (desktop view).
- Hover and active state: Change link color for interaction feedback.

```css
.nav-toggle-checkbox {
    display: none;
}

.nav-toggle {
    display: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: var(--space-xs);
    z-index: 1001;
}

.nav-toggle span {
    display: block;
    width: 25px;
    height: 2px;
    background-color: var(--color-navy-blue);
    margin: 5px 0;
}
```

- `.nav-toggle-checkbox`: Hidden visually; only used for state.
- `.nav-toggle`: The clickable hamburger icon (hidden by default; displayed in mobile via `@media`).
- `.nav-toggle span`: Each bar of the hamburger icon.

### 4.5 Hero Section and `::before` Pseudo-Element

```css
.hero {
    min-height: clamp(500px, 100vh, 1200px);
    background: url('placeholder-hero.jpg') center/cover no-repeat;
    margin-top: clamp(64px, 8vh, 80px);
    position: relative;
    display: flex;
    align-items: center;
    padding-block: var(--space-xxl);
    isolation: isolate;
}

.hero::before {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(rgba(56, 163, 165), rgba(34, 87, 122));
    z-index: -1;
}
```

#### What `::before` Does Here

- `::before` is a **pseudo-element** that creates a new element **before** the content of `.hero`.
- `content: "";` is required to make it appear.
- It is positioned absolutely to cover the entire hero (`inset: 0;` is a shorthand for top/right/bottom/left = 0).
- The background gradient overlays the hero background image.
- `z-index: -1;` places it behind the hero content but above the actual background image of `.hero`.

**Purpose:**

- Adds a tinted gradient overlay on top of the hero image to improve text contrast and visual style without needing extra markup in the HTML.

### 4.6 Buttons, Lists, and Cards

Buttons:

```css
.btn {
    display: inline-block;
    padding: 14px 32px;
    border-radius: 4px;
    font-weight: 600;
    text-transform: uppercase;
    transition: transform 0.3s ease, background-color 0.3s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

.btn-primary {
    background-color: var(--color-secondary);
    color: var(--color-white);
}

.btn-secondary {
    background-color: transparent;
    border: 2px solid var(--color-secondary);
    color: var(--color-secondary);
}
```

- `.btn`: Shared base styles for all buttons.
- `.btn-primary`: Solid background.
- `.btn-secondary`: Outline style.

Benefit list:

```css
.benefits-list {
    list-style: none;
    margin: var(--space-lg) 0;
    display: flex;
    flex-direction: column;
    gap: var(--space-sm);
}
```

- Removes bullets and uses flexbox column layout with gaps.

Cards:

```css
.cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(280px, 100%), 1fr));
    gap: var(--grid-gap);
}

.card {
    background: var(--color-white);
    padding: var(--space-lg);
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    display: flex;
    flex-direction: column;
}
```

- `repeat(auto-fit, minmax(...))`: Responsive number of columns based on available space.
- `.card`: Box with background, padding, shadow, and hover transform.

### 4.7 Gallery and Contact Layout

```css
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(min(300px, 100%), 1fr));
    gap: var(--grid-gap);
}

.gallery-item {
    background: var(--color-light-gray);
    aspect-ratio: 4/3;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.3s ease;
}

.gallery-item:hover {
    transform: scale(1.05);
}
```

- Responsive gallery grid with hover zoom effect.

Contact grid:

```css
.contact-content {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(350px, 100%), 1fr));
    gap: var(--grid-gap-lg);
}

.contact-form input,
.contact-form textarea {
    padding: var(--space-sm);
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 4px;
    background: rgba(255,255,255,0.1);
    color: var(--color-white);
}
```

- Contact columns adapt to width.
- Inputs styled to match the dark theme of the section.

### 4.8 Footer Styles

```css
.footer {
    background-color: var(--color-primary-dark);
    color: var(--color-white);
    padding: var(--space-xl) 0 var(--space-md);
}

.footer-content {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(200px, 100%), 1fr));
    gap: var(--grid-gap-lg);
}

.footer-bottom {
    text-align: center;
    padding-top: var(--space-lg);
    border-top: 1px solid rgba(255,255,255,0.1);
}
```

- Grid-based layout for footer columns.
- Subtle border and spacing in the bottom section.

---

## 5. Responsive Design with `@media` Queries

The stylesheet uses `@media` queries to implement **responsive design**—the layout adapts to different screen sizes and user preferences.

### 5.1 What is an `@media` Query?

A `@media` query applies certain CSS rules **only when specific conditions are met**, such as:

- Maximum or minimum viewport width.
- User preference for reduced motion.
- Orientation, resolution, etc.

Basic pattern:

```css
@media (max-width: 768px) {
    /* Rules that only apply when viewport width is 768px or less */
}
```

In this project, `@media` queries are used to:

- Convert the desktop navigation into a mobile-friendly menu.
- Adjust padding and layout for smaller screens.
- Improve touch targets on mobile.
- Respect reduced motion settings.

### 5.2 Tablet and Below (`max-width: 768px`)

```css
@media (max-width: 768px) {
    .nav-toggle {
        display: block;
    }

    .nav-menu {
        position: fixed;
        top: clamp(64px, 8vh, 80px);
        left: 0;
        right: 0;
        background: var(--color-white);
        padding: var(--space-lg) var(--space-md);
        flex-direction: column;
        align-items: stretch;
        gap: var(--space-md);
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        transform: translateY(-100%);
        opacity: 0;
        pointer-events: none;
        transition: transform 0.3s ease, opacity 0.3s ease;
    }

    .nav-menu.active {
        transform: translateY(0);
        opacity: 1;
        pointer-events: all;
    }

    .nav-toggle-checkbox:checked ~ .nav-menu {
        transform: translateY(0);
        opacity: 1;
        pointer-events: all;
    }

    .hero-content {
        text-align: center;
        max-width: 100%;
    }

    .btn {
        padding: 16px 40px;
        min-width: 200px;
    }
}
```

**Key points:**

- `.nav-toggle { display: block; }`: Shows the hamburger icon on tablets and smaller devices.
- `.nav-menu`: Becomes a full-width fixed dropdown panel under the header, with vertical layout.
- Initial state: `transform: translateY(-100%); opacity: 0; pointer-events: none;` hides the menu.
- Interactive state:
  - `.nav-toggle-checkbox:checked ~ .nav-menu { ... }` shows the menu when the checkbox is checked.
  - This is the **CSS-only mobile navigation toggle** mechanism.
- Buttons become larger with increased padding and a `min-width` to improve touch usability.

### 5.3 Mobile (`max-width: 480px`)

```css
@media (max-width: 480px) {
    .trust-indicators {
        flex-direction: column;
        align-items: center;
        gap: var(--space-lg);
    }

    .trust-item {
        min-width: 150px;
    }

    .contact-form input,
    .contact-form textarea {
        padding: var(--space-md);
        font-size: 16px; /* Prevents zoom on iOS */
    }

    .footer-content {
        text-align: center;
    }

    .footer-section ul {
        align-items: center;
    }
}
```

- Trust indicators stack vertically for better readability.
- Form inputs get more padding and a guaranteed `font-size: 16px` to avoid automatic zoom on mobile browsers (especially iOS).
- Footer content centers to better fit narrow screens.

### 5.4 Large Desktop (`min-width: 1440px`)

```css
@media (min-width: 1440px) {
    .hero-content,
    .content-text {
        max-width: 65ch; /* Optimal reading width */
    }

    .cards-grid {
        grid-template-columns: repeat(3, 1fr);
    }

    .gallery-grid {
        grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    }
}
```

- For very large screens, line length is capped at around 65 characters (`65ch`) for readability.
- `cards-grid` is forced to three equal columns.
- `gallery-grid` uses slightly larger minimum card widths.

### 5.5 Reduced Motion Preference

```css
@media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

- `prefers-reduced-motion` is a user accessibility setting.
- When enabled, this query effectively **disables animations and transitions** by making them nearly instantaneous.
- This reduces motion for users who are sensitive to animations.

---

## 6. Utility Classes

The stylesheet includes some utility classes for layout and visibility.

### 6.1 Container Variants

```css
.container-narrow { max-width: 900px; }
.container-wide { max-width: 100%; }
```

- Allow tighter or full-width layouts where needed (not heavily used in this HTML but available).

### 6.2 Flow Spacing Utilities

```css
.flow > * + * {
    margin-top: var(--space-md);
}

.flow-lg > * + * {
    margin-top: var(--space-lg);
}
```

- `.flow`: Any element with this class will give each direct child (except the first) a top margin.
- This provides consistent vertical spacing without repeating margins on individual elements.

### 6.3 Responsive Display Utilities

```css
.mobile-only { display: none; }

@media (max-width: 768px) {
    .mobile-only { display: block; }
    .desktop-only { display: none; }
}
```

- `.mobile-only`: Hidden by default, shown on screens ≤768px.
- `.desktop-only`: Visible by default (no base rule here), but hidden on small screens.
- Useful for swapping content tailored for mobile vs desktop.

### 6.4 Print Styles

```css
@media print {
    .header,
    .nav-toggle,
    .btn,
    .footer {
        display: none;
    }

    .hero {
        min-height: auto;
        margin-top: 0;
    }

    .section {
        page-break-inside: avoid;
    }
}
```

- When printing, certain elements (like header, nav toggle, buttons, footer) are hidden.
- Hero section height and margin are reset for better print layout.
- `page-break-inside: avoid;` tries to prevent sections from splitting across pages.

---

## 7. Summary of the Mobile Navigation Implementation (CSS-Only)

This project previously used a JavaScript `<script>` tag for navigation behavior, but it has now been implemented entirely with HTML and CSS.

**Key elements:**

- Hidden checkbox (`.nav-toggle-checkbox`) to store open/closed state.
- `label.nav-toggle` with three `<span>` bars to act as the hamburger button.
- `.nav-menu` that is visually transformed into a dropdown panel on mobile.

**Key CSS:**

1. Hide the checkbox but keep it functional:

   ```css
   .nav-toggle-checkbox {
       display: none;
   }
   ```

2. Show the hamburger icon only on smaller screens:

   ```css
   @media (max-width: 768px) {
       .nav-toggle {
           display: block;
       }
   }
   ```

3. Transform the menu into a hidden dropdown panel:

   ```css
   @media (max-width: 768px) {
       .nav-menu {
           position: fixed;
           top: clamp(64px, 8vh, 80px);
           left: 0;
           right: 0;
           transform: translateY(-100%);
           opacity: 0;
           pointer-events: none;
       }

       .nav-toggle-checkbox:checked ~ .nav-menu {
           transform: translateY(0);
           opacity: 1;
           pointer-events: all;
       }
   }
   ```

**Result:**

- Clicking the hamburger icon toggles the hidden checkbox.
- The `:checked` state triggers CSS rules via the sibling selector (`~`) to show or hide the menu.
- No JavaScript is needed; the behavior is entirely declarative.

---

## 8. How Everything Fits Together

- **HTML** defines the semantic structure: header, hero, sections, footer, and form.
- **CSS variables in `:root`** provide a consistent design system (colors, typography, spacing, breakpoints).
- **Pseudo-elements like `::before`** add decorative layers (e.g., the hero gradient overlay) without extra HTML.
- **`@media` queries** adapt layout and behavior to different screen sizes and user preferences.
- **Checkbox + label navigation pattern** enables a fully functional mobile menu with no JavaScript.

Together, these patterns create a fully responsive, accessible, and static website using only HTML and CSS.