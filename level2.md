# HTML&CSS

## Responsive CSS (Media Queries, Mobile/Desktop First Approach)

Responsive design ensures web content adapts to different screen sizes and devices. Media queries are the foundation of responsive CSS.

### Media Query Basics

Media queries allow you to apply CSS rules conditionally based on device characteristics:

```html

<link rel="stylesheet" media="screen and (min-width: 900px)" href="widescreen.css">
<link rel="stylesheet" media="screen and (max-width: 600px)" href="smallscreen.css">
```

or

```css
@media (max-width: 768px) {
  .container {
    padding: 10px;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .container {
    padding: 20px;
  }
}
```

### Common Breakpoints

Standard breakpoints for responsive design:

- Mobile: `320px - 480px`
- Tablet: `481px - 768px`
- Desktop: `769px - 1024px`
- Large Desktop: `1025px+`

### Mobile-First vs Desktop-First Approach

**Mobile-First (Recommended):**
- Start with mobile styles as the base
- Use `min-width` media queries to enhance for larger screens
- Progressive enhancement approach

**Desktop-First:**
- Start with desktop styles as the base
- Use `max-width` media queries to adapt for smaller screens
- Graceful degradation approach

Example for mobile-first approach:
- [responsive mobile-first](level2/example-responsive-mobile-first.html)

### Media Query Features

**Viewport Dimensions:**
- `width`, `height` - exact dimensions
- `min-width`, `max-width` - minimum/maximum width
- `min-height`, `max-height` - minimum/maximum height

**Device Orientation:**
- `orientation: portrait` - device in portrait mode
- `orientation: landscape` - device in landscape mode

**Display Density:**
- `resolution` - device pixel ratio
- `min-resolution`, `max-resolution` - minimum/maximum resolution

**Example:**
```css
@media (min-width: 768px) and (orientation: landscape) {
  .sidebar {
    width: 300px;
  }
}

@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .logo {
    background-image: url('logo@2x.png');
  }
}
```

### Responsive Typography

Use relative units and fluid typography:

```css
/* Fluid typography */
h1 {
  font-size: clamp(1.5rem, 4vw, 3rem);
}

/* Responsive line-height */
p {
  line-height: 1.6;
}

@media (min-width: 768px) {
  p {
    line-height: 1.8;
  }
}
```

### Responsive Images

```css
img {
  max-width: 100%;
  height: auto;
}

/* Different images for different screen sizes */
.hero-image {
  background-image: url('hero-mobile.jpg');
}

@media (min-width: 768px) {
  .hero-image {
    background-image: url('hero-desktop.jpg');
  }
}
```

### Container Queries (Modern Approach)

Container queries allow styling based on container size rather than viewport:

```css
.card {
  container-type: inline-size;
}

@container (min-width: 300px) {
  .card-content {
    display: flex;
  }
}
```
or 
```css
.post {
  container-type: inline-size;
  container-name: sidebar;
}
/*
.post {
  container: sidebar / inline-size;
}
*/

@container sidebar (width > 700px) {
  .card {
    font-size: 2em;
  }
}
```
## Advanced Layouts (Flexbox, Grid)

Modern CSS provides powerful layout systems for creating complex, responsive designs.

### Flexbox (1D Layout)

Flexbox is ideal for one-dimensional layouts (row or column) and component-level styling.

**Flex Container Properties:**

```css
.flex-container {
  display: flex; /* or inline-flex */
  flex-direction: row | row-reverse | column | column-reverse;
  flex-wrap: nowrap | wrap | wrap-reverse;
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;    /* main axis*/
  align-items: stretch | flex-start | flex-end | center | baseline; /* perpendicular main axis*/
  align-content: stretch | flex-start | flex-end | center | space-between | space-around | space-evenly; /* Y axis*/
  gap: 20px; /* Space between items */
}
```

**Flex Item Properties:**

```css
.flex-item {
  flex-grow: 0; /* How much to grow */
  flex-shrink: 1; /* How much to shrink */
  flex-basis: auto; /* Initial size */
  flex: 1 1 200px; /* Shorthand: grow shrink basis */
  order: 0; /* Visual order */
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

**Common Flexbox Patterns:**

Examples for flexbox layouts:
- [flexbox examples](level2/example-flexbox.html)

### CSS Grid (2D Layout)

Grid is perfect for two-dimensional layouts with explicit rows and columns.

**Grid Container Properties:**

```css
.grid-container {
  display: grid; /* or inline-grid */
  grid-template-columns: 1fr 2fr 1fr; /* Column sizes */
  grid-template-rows: auto 1fr auto; /* Row sizes */
  gap: 20px; /* Space between grid items */
  grid-auto-flow: row | column | dense;
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
```

**Grid Item Properties:**

```css
.grid-item {
  grid-column: 1 / 3; /* Start / end column */
  grid-row: 1 / 2; /* Start / end row */
  grid-area: header; /* Named grid area */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

**Grid Sizing Functions:**

```css
.grid-container {
  /* Repeat function */
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  
  /* Minmax function */
  grid-template-columns: minmax(200px, 1fr) 2fr;
  
  /* Auto-fit vs auto-fill */
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}
```

**Common Grid Patterns:**

Examples for grid layouts:
- [grid examples](level2/example-grid.html)

### Combining Flexbox and Grid

Use Grid for overall page layout and Flexbox for component-level styling:

```css
/* Grid for page structure */
.page {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

/* Flexbox for component styling */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card {
  display: flex;
  flex-direction: column;
}

.card-content {
  flex: 1; /* Push footer to bottom */
}
```

## Form Inputs - Elements and Styling

Forms are crucial for user interaction. Understanding form elements and their styling is essential for creating accessible, user-friendly interfaces.

### Form Elements Overview

**Input Types:**

```html
<!-- Text inputs -->
<input type="text" placeholder="Enter your name">
<input type="email" placeholder="Enter your email">
<input type="password" placeholder="Enter password">
<input type="tel" placeholder="Phone number">
<input type="url" placeholder="Website URL">
<input type="search" placeholder="Search...">

<!-- Number inputs -->
<input type="number" min="0" max="100" step="1">
<input type="range" min="0" max="100" value="50">

<!-- Date and time -->
<input type="date">
<input type="time">
<input type="datetime-local">
<input type="month">
<input type="week">

<!-- File upload -->
<input type="file" accept="image/*">
<input type="file" multiple>

<!-- Selection inputs -->
<input type="radio" name="option" value="option1">
<input type="checkbox" name="feature" value="feature1">

<!-- Action inputs -->
<input type="submit" value="Submit">
<input type="button" value="Click me">
<input type="reset" value="Reset">
<input type="hidden" name="token" value="abc123">
```

**Other Form Elements:**

```html
<!-- Text areas -->
<textarea placeholder="Enter your message" rows="4" cols="50"></textarea>

<!-- Select dropdowns -->
<select name="country">
  <option value="">Choose a country</option>
  <option value="us">United States</option>
  <option value="ca">Canada</option>
  <option value="uk">United Kingdom</option>
</select>

<!-- Fieldset and legend for grouping -->
<fieldset>
  <legend>Personal Information</legend>
  <input type="text" name="firstName" placeholder="First Name">
  <input type="text" name="lastName" placeholder="Last Name">
</fieldset>

<!-- Labels for accessibility -->
<label for="email">Email Address:</label>
<input type="email" id="email" name="email">
```

### Form Styling Basics  <-- TODO refactor

**Reset and Base Styles:**

Form styling examples:
- [form examples](level2/example-forms.html)

### Advanced Input Styling

**Custom Checkbox and Radio:**

Custom styling techniques for form elements are demonstrated in the form examples.

### Form Validation Styling 
**Visual Validation States:**

Form validation styling including success/error states, required field indicators, and custom validation messages are demonstrated in the form examples.

### Responsive Form Design

Responsive form layouts including floating labels, form rows, and mobile adaptations are demonstrated in the form examples.

### Accessibility Considerations

Accessibility features including focus indicators, high contrast support, reduced motion support, and screen reader considerations are demonstrated in the form examples.

### Form Layout Patterns

Various form layout patterns including inline forms, two-column layouts, card forms, and step forms are demonstrated in the form examples.

