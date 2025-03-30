# Using Modified Pico CSS with Google Fonts

This guide explains how to use our modified Pico CSS framework with Google Fonts in your projects.

## Quick Start Template

Here's a complete HTML template that you can use as a starting point:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Project with Pico CSS</title>
    
    <!-- Google Fonts - Source Sans 3 -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Source+Sans+3:ital,wght@0,200..900;1,200..900&display=swap" rel="stylesheet">
    
    <!-- Pico CSS Framework -->
    <link rel="stylesheet" href="css/pico.min.css">
    
    <!-- Apply Source Sans 3 from Google Fonts -->
    <style>
        :root {
            --oz-pico-font-family-sans-serif: 'Source Sans 3', system-ui, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, Helvetica, Arial, "Helvetica Neue", sans-serif, var(--oz-pico-font-family-emoji);
            --oz-pico-font-family: var(--oz-pico-font-family-sans-serif);
        }
    </style>
</head>
<body>
    <main class="container">
        <h1>Hello World</h1>
        <p>Your content goes here...</p>
    </main>
</body>
</html>
```

## Steps to Set Up Your Project

1. **Create a Basic Project Structure**:
   ```
   my-project/
   ├── css/
   │   └── pico.min.css (copy from our modified version)
   ├── index.html
   └── README.md
   ```

2. **Copy the Modified CSS**:
   Copy the `pico.min.css` file from our project's `css` directory to your project's `css` directory.

3. **Use Google Fonts in Your HTML**:
   Add the Google Fonts links to your HTML head, as shown in the template above.

4. **Apply the Font with CSS Variables**:
   Include the `:root` styles to apply Source Sans 3 to the Pico CSS framework.

## Benefits of This Approach

- **Better Loading Performance**: Google Fonts are optimized for fast loading and are often already cached in users' browsers
- **Easier Maintenance**: No need to manage font files in your project
- **Better Separation of Concerns**: HTML loads fonts, CSS handles styling
- **Improved Flexibility**: Easily switch or update fonts without rebuilding CSS

## Customizing the Font

If you want to use a different font:

1. Replace the Google Fonts link with your preferred font
2. Update the CSS variable to reference your new font:

```css
:root {
    --oz-pico-font-family-sans-serif: 'Your Font Name', system-ui, /* ... other fallbacks ... */;
    --oz-pico-font-family: var(--oz-pico-font-family-sans-serif);
}
```

## Using the Original System Fonts

If you prefer to use the default system fonts without Google Fonts:

1. Skip the Google Fonts links in your HTML
2. Don't include the custom style block with font family variables

The modified Pico CSS will automatically use the system fonts defined in the CSS.

## Compatibility

This approach works with all modern browsers and provides graceful fallbacks to system fonts when needed. 