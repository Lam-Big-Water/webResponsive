- center on the screen 
```css
margin: 0 auto;
```
# Introduction
- A meta element for viewport
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
explain(
    `
    width=device-width => 
    This tells the browser to assume the width of the website is the same as the width of the device.


    initial-scale=1 => 
    This tells the browser how much or how little scaling to do.
    `
)
```

# Media queries
- `@media`
```css
@media print {
  body {
    background-color: transparent;
  }
}
```
```html
<link rel="stylesheet" href="global.css">
<link rel="stylesheet" href="print.css" media="print">

explain(`
You can use the `@media` at-rule, or link
`)
```

- Query conditions
```css
@media all and (orientation: landscape) {
   // Styles for landscape mode.
}
@media all and (orientation: portrait) {
   // Styles for portrait mode.
}
```

- Adjust styles based on viewport size
```css
@media (min-width: 400px) {
    /* Style for viewport widers than 400 pixels */
}

@media (max-width: 400px) {
    /* Styles for viewports narrower than 400 pixels */
}

@media (min-width: 50em) and (max-width: 60em) {
    /* Styles for viewports wider than 50em and narrower than 60em */
}
```

# Internationalization

- Logical properties
```css
text-align
margin-inline-start
margin-inline-end
```
```html
<!-- 
  translated into a right-to-left language 
  dir => direction
  rtl => right to left
-->
<body dir="rtl">
</body>
```
```css
writing-mode
```
![Alt text](https://web.dev/static/learn/design/internationalization/image/latin-hebrew-japanese-a-538b56c52363b.webp)


- Identify page language
```html
<!-- But unlike the lang attribute, which can go on any element, hreflang can only be applied to a and link elements. -->
<html lang="en">

<p>I felt some <span lang="de">schadenfreude</span>.</p>

<a href="/path/to/german/version" hreflang="de">German version</a>

<a href="/path/to/german/version" hreflang="de" lang="de">Deutsche Version</a>

<link href="/path/to/german/version" rel="alternate" hreflang="de">
```
```css
/* specifies how words should be hyphenated when text wraps across multiple lines. */
hyphens: auto;
```

# Macro layouts - page
```css
/* automatically */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
  grid-gap: 1em;
}
```

# Micro layouts - component

- grid
```css
h1 {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 1em;
}
h1::before,
h1::after {
  content: "";
  border-top: 0.1em double black;
  align-self: center;
}
```

- flex
```css
.media {
  display: flex;
  align-items: center;
  gap: 1em;
}
.media-illustration {
  flex: 1;
  max-inline-size: 200px;
}
.media-content {
  flex: 3;
}
```

- container queries
```css
main,
aside {
  container-type: inline-size;
}

.media-illustration {
  max-width: 200px;
  margin: auto;
}

@container (min-width: 25em) {
  .media {
    display: flex;
    align-items: center;
    gap: 1em;
  }

  .media-illustration {
    flex: 1;
  }

  .media-content {
    flex: 3;
  }
}
```
![Alt text](https://web.dev/static/learn/design/micro-layouts/image/two-containers-different-5a460bfff7342.png)


# Typography

- If you don't specify any styles for your text, browsers will apply their own default styles - User Agent Stylesheets.

- Scaling Text - That the text will get too small or too big
```css
html {
  font-size: calc(0.75rem + 1.5vw);
}
```

- Clamping text
```css
/* clamp() => it takes three value. The middle value is the same as what you pass to calc(). The opening value = minimum, the closing value = maximum */
html {
  font-size: clamp(1rem, 0.75rem + 1.5vw, 2rem);
}
```

- Line length
```css
/* Using ch units for width will cause new lines to wrap at the 66th character at that font size */
article {
  max-inline-size: 66ch;
}
```

- Line height
```css
/* Use unit-less values for your line-height declarations. This ensures that the line height is relative to the font-size*/
article {
  max-inline-size: 66ch;
  line-height: 1.65;
}
blockquote {
  max-inline-size: 45ch;
  line-height: 2;
}
```

- Web fonts
```css
/* Use @font-face to tell browsers where to find you your web font files - but every web font file you add could potentially degrade the user experience as it increases page load time. */
@font-face {
  font-family: Roboto;
  src: url('/fonts/roboto-regular.woff2') format('woff2');
}
body {
  font-family: Roboto, sans-serif;
}

```

- Font loading
```html
/*
You can request that browsers start downloading a font file as soon as possible. Add a link element to the head of your document that references your web font file. A rel attribute with a value of preload tells the browser to prioritize that file. An as attribute with a value of font tells the browser what kind of file this is. The type attribute allows you to be even more specific.

You need to include the crossorigin attribute even if you are hosting the font files yourself.
*/
<link href="/fonts/roboto-regular.woff2" type="font/woff2" 
  rel="preload" as="font" crossorigin>
```

```css
/* Use a font-display value of swap if you still want the web font to replace the system font whenever the web font finally loads. */
body {
  font-family: Roboto, sans-serif;
  font-display: swap;
}


/* Use a font-display value of fallback if you want to stick with the system font once text has been rendered. */
body {
  font-family: Roboto, sans-serif;
  font-display: fallback;
}
```

- Variable fonts - If you're using lots of different weights, a variable font could give you a big performance gain.
