# Customizing Pico CSS Framework

This document outlines the steps taken to customize the Pico CSS framework for our needs. We'll keep updating this as we make more modifications.

## Initial Setup

1. Clone the Pico CSS repository
2. Install dependencies:
   ```bash
   yarn install
   ```

## Base Configuration Changes

In `scss/_settings.scss`, we've made two important modifications:
1. Changed the CSS variable prefix from `--pico-` to `--oz-pico-`
2. Set the default theme color to "indigo"

```scss
// Prefix for CSS variables
$css-var-prefix: "--oz-pico-" !default; // Must start with "--"

// Theme color
$theme-color: "indigo" !default;
```

## Font Customization

We've customized the framework to use Source Sans 3 as our primary font, with self-hosted font files:

1. Downloaded Source Sans 3 font files in WOFF2 format and placed them in `fonts/source-sans-3/`

2. Created `scss/_fonts.scss` with @font-face declarations for each weight and style:
```scss
/* source-sans-3-regular - latin */
@font-face {
  font-display: swap;
  font-family: 'Source Sans 3';
  font-style: normal;
  font-weight: 400;
  src: url('/fonts/source-sans-3/source-sans-3-v18-latin-regular.woff2') format('woff2');
}
/* Other weights and styles... */
```

> **Important**: Note the use of absolute paths with leading slashes in the font URLs. This is crucial for the minified CSS to work properly, as the build process may not correctly handle relative paths in minified files.

3. Imported the fonts file in `scss/pico.scss`:
```scss
@forward "settings";
@use "fonts";  // Import our fonts first
@use "index";
@use "custom";
```

4. Set the font family in `scss/_custom.scss`:
```scss
:root {
  --oz-pico-font-family-sans-serif:
    "Source Sans 3", system-ui, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, Helvetica, Arial,
    "Helvetica Neue", sans-serif, var(--oz-pico-font-family-emoji);
  --oz-pico-font-family: var(--oz-pico-font-family-sans-serif);
}
```

This self-hosted approach:
- Includes all weights (200-900) and italic styles
- Uses optimized WOFF2 format for modern browsers
- Maintains system fonts as fallbacks
- Works "out of the box" without requiring Google Fonts links in HTML
- Allows offline use and better performance

## Typography Customizations

We've also made adjustments to the general typography:

```scss
:root {
  // Typography
  --oz-pico-font-size: 95%; // Default is 100% - making it slightly smaller
  --oz-pico-line-height: 1.5; // Default is 1.5
}
```

## Button Customizations

We've customized the appearance of secondary buttons:

```scss
// Light theme (default)
:root:not([data-theme="dark"]) {
  --oz-pico-secondary-background: #eff1f4; // Slate 50
  --oz-pico-secondary-hover-background: #dfe3eb; // Slate 100 for hover
  --oz-pico-secondary-inverse: rgb(30 41 59); // Slate 800 for text
  --oz-pico-secondary-border: rgba(255, 255, 255, 0); // invisible
  // More customizations...
}

// Dark theme
:root[data-theme="dark"] {
  --oz-pico-secondary-background: rgb(30 41 59); // Slate 800
  --oz-pico-secondary-hover: rgb(51 65 85); // Slate 700 for hover
  --oz-pico-secondary-inverse: #e9e9e9; // Slate 50 for text
  --oz-pico-secondary-border: rgba(24, 28, 37, 0); // invisible
  // More customizations...
}
```

## Understanding Pico's Sizing System

In Pico, component sizing is controlled through CSS variables (custom properties) defined in the framework. The main spacing variables that affect component sizes are:

- `--oz-pico-spacing`: Base spacing unit (default: 1rem)
- `--oz-pico-form-element-spacing-vertical`: Vertical padding for form elements and buttons (default: 0.75rem)
- `--oz-pico-form-element-spacing-horizontal`: Horizontal padding for form elements and buttons (default: 1rem)

These variables cascade down to affect various components throughout the framework.

## Customizing Base Properties

### 1. Create Custom Override File

Create a new file `scss/_custom.scss` to hold our customizations:

```scss
// Custom overrides for Pico CSS
@use "sass:map";

:root {
  // Base spacing (affects many components)
  --oz-pico-spacing: 0.75rem;  // Default is 1rem
  
  // Form elements and buttons
  --oz-pico-form-element-spacing-vertical: 0.35rem;  // Default is 0.75rem
  --oz-pico-form-element-spacing-horizontal: 0.55rem;  // Default is 1rem
}
```

Note: The CSS variable prefix in `_custom.scss` must match the prefix defined in `_settings.scss`.

### 2. Import Custom Overrides

Modify `scss/pico.scss` to import our custom overrides after the default styles:

```scss
@forward "settings";
@use "fonts";
@use "index";
@use "custom";  // Add our custom overrides last so they take precedence
```

### 3. Rebuild CSS

After making changes, rebuild the CSS:
```bash
yarn build
```

## Testing Changes

The framework includes example HTML files that can be used to preview your changes:

1. Regular example with classes: `examples/v2-html/index.html`
2. Classless example: `examples/v2-html-classless/index.html`

These files have been modified to use your local CSS files instead of the CDN version:
```html
<link rel="stylesheet" href="../../css/pico.min.css">
```

## File Structure

Important files for customization:
- `fonts/source-sans-3/` - Self-hosted font files
- `scss/_fonts.scss` - Font declarations
- `scss/_custom.scss` - Our custom overrides
- `scss/pico.scss` - Main SCSS file
- `scss/themes/default/_styles.scss` - Original default styles (reference only, don't modify)
- `css/pico.min.css` - Compiled CSS (don't edit directly)

## Development Workflow

1. Make changes to `_custom.scss` or other SCSS files
2. Run `yarn build` to compile
3. Refresh example pages in browser to see changes
4. Repeat as needed

---
*This documentation will be updated as we make more customizations to the framework.* 

## Font Loading Update (March 30, 2024)

We've updated our approach to font loading:

### Removed Embedded Fonts

We've removed the embedded Source Sans 3 font from the CSS for the following reasons:
- Better separation of concerns: HTML loads fonts, CSS handles styling
- Improved performance through Google Fonts' optimized delivery
- Easier maintenance without font files in the project
- Better caching as Google Fonts are likely already cached in many browsers

### Changes Made:

1. Removed the `scss/_fonts.scss` file with all @font-face declarations
2. Updated `scss/pico.scss` to remove the font import
3. Modified font family declarations in `scss/_custom.scss` to use system fonts by default
4. Created a new approach using Google Fonts in HTML templates

### How to Use Source Sans 3 in Your Projects

Instead of embedded fonts, add the following to your HTML head:

```html
<!-- Google Fonts - Source Sans 3 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Source+Sans+3:ital,wght@0,200..900;1,200..900&display=swap" rel="stylesheet">

<!-- Add custom styles to specify the font -->
<style>
    :root {
        --oz-pico-font-family-sans-serif: 'Source Sans 3', system-ui, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, Helvetica, Arial, "Helvetica Neue", sans-serif, var(--oz-pico-font-family-emoji);
        --oz-pico-font-family: var(--oz-pico-font-family-sans-serif);
    }
</style>
```

This approach provides better performance and follows best practices for web font loading.

See the `test-with-google-font.html` file for a complete example. 