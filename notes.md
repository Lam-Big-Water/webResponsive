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


# Responsive images
- Images, on the other hand, have an intrinsic size. If an image is wider than the screen, the image overflows and the user has to scroll horizontally to see all of it.

- Constrain your images
```css
img {
  /* to declare that images can never be rendered at a size wider than their containing element */

  /* You can apply the same rule to other kinds of embedded content too, like videos and iframes. */
  max-inline-size: 100%;

  /* Adding a block-size value of auto means the browser preserves your images' aspect ratio as it resizes them. */
  block-size: auto;

  /* property to preserve your site's design - Unfortunately, this often means the browser has to squash or stretch the image to make it fit in the intended space.*/
  aspect-ratio: 2 / 1;

  /* To prevent squashing and stretching, use the object-fit property. */
  /* tells the browser to preserve the image's aspect ratio, leaving empty space around the image if needed.*/
  object-fit: contain;


  /* tells the browser to preserve the image's aspect ratio, cropping the image if needed. */
  object-fit: cover;

  /* This adjusts the focus of the crop, so you can make sure the most important part of the image is still visible. */
  object-position: top center;
}
```

- Hints for sizing
```html
<!-- always include width and height attributes. This prevents your other content from jumping around when the image loads. -->
<img
 src="image.png"
 alt="A description of the image."
 width="300"
 height="200"
>
```

- Hints for loading
```html
<!--  For images below the fold, use a value of lazy. The browser won't load lazy images until the user has scrolled far down enough that the image is about to come into view. If the user never scrolls, the image never loads. -->
<img
 src="image.png"
 alt="A description of the image."
 width="300"
 height="200"
 loading="lazy"
>

<!-- the default value of eager to prevent images from being lazy loaded: -->
<img
 src="hero.jpg"
 alt="A description of the image."
 width="1200"
 height="800"
 loading="eager"
>
```

- Fetch Priority
```html
<!-- tells the browser to fetch the image right away and at high priority, instead of waiting until the browser has finished its layout and would fetch images normally. -->
<img
 src="hero.jpg"
 alt="A description of the image."
 width="1200"
 height="800"
 loading="eager"
 fetchpriority="high"
>
```

- Hints for preloading
```html
<!-- However, some images may be unavailable, such as images added by JavaScript or a CSS background image. -->

<!-- You can use preloading to get the browser to fetch these important images ahead of time. For really important images, you can combine this preloading with the fetchpriority attribute -->
<link rel="preload" href="hero.jpg" as="image" fetchpriority="high">
```
<i style="color: red">Again, use these attributes sparingly to avoid overriding the browser's prioritization heuristics too often. Overusing them can cause performance degradation.
</i>

```html
<!-- Some browsers support preloading responsive images based on srcset, using the imagesrcset and imagesizes attributes. For example: -->
<link rel="preload" imagesrcset="hero_sm.jpg 1x hero_med.jpg 2x hero_lg.jpg 3x" as="image" fetchpriority="high">
```

- Image decoding
```html
<!-- There's also a decoding attribute you can add to img elements. You can tell the browser that the image can be decoded asynchronously, so it can prioritize processing other content. -->
<img
 src="image.png"
 alt="A description of the image."
 width="300"
 height="200"
 loading="lazy"
 decoding="async"
>
<!-- You can use the sync value if the image itself is the most important piece of content to prioritize.
 -->
<img
 src="hero.jpg"
 alt="A description of the image."
 width="1200"
 height="800"
 loading="eager"
 decoding="sync"
>

```
- Responsive images with 'srcset'

- if a user has a small screen and a low-bandwidth network, making them download the same size images as users with larger screens wastes data. - To fix this issue, add multiple versions of the same image at different sizes, and use the srcset attribute to tell the browser these sizes exist and when to use them.

```html
<!-- To save bandwidth, the browser only downloads the larger image if they're needed. -->
<img
 src="small-image.png"
 alt="A description of the image."
 width="300"
 height="200"
 loading="lazy"
 decoding="async"
 srcset="small-image.png 300w,
  medium-image.png 600w,
  large-image.png 1200w"
>
<!-- If you're using the width descriptor, you must also use the sizes attribute to give the browser more information. This tells the browser what size you expect the image to be displayed at under different conditions. Those conditions are specified in a media query. -->
<img
 src="small-image.png"
 alt="A description of the image."
 width="300"
 height="200"
 loading="lazy"
 decoding="async"
 srcset="small-image.png 300w,
  medium-image.png 600w,
  large-image.png 1200w"
 sizes="(min-width: 66em) 33vw,
  (min-width: 44em) 50vw,
  100vw"
>
```

- Pixel density descriptor
```html
<!-- Use the density descriptor to describe the pixel density of the image in relation to the image in the src attribute. The density descriptor is a number followed by the letter x, as in 1x or 2x. -->
<img
 src="small-image.png"
 alt="A description of the image."
 width="300"
 height="200"
 loading="lazy"
 decoding="async"
 srcset="small-image.png 1x,
  medium-image.png 2x,
  large-image.png 3x"
>
```
<i style="color: red">Note: You can use either width descriptors or density descriptors, but not both together.</i>


- Presentational images
```html
<!-- Images in HTML are content. That's why you include the alt attribute with a description of the image for screen readers and search engines. -->
<img
 src="flourish.png"
 alt=""
 width="400"
 height="50"
>
```

- Background images
```css
element {
  background-image: url(flourish.png);
}

/* The image-set function in CSS works a lot like the srcset attribute in HTML. Provide a list of images with a pixel density descriptor for each one. */
element {
  background-image: image-set(
    small-image.png 1x,
    medium-image.png 2x,
    large-image.png 3x
  );
}
```

## Sum up
- There are many factors to consider when you're adding images to your site, including:
 1. Reserving the right space for each image.
 2. Figuring out how many sizes you need.
 3. Deciding whether the image is content or decorative.


# The picture element
```html
<!-- In the same way that srcset builds upon the src attribute -->
<!-- The difference is that where the srcset attribute gives suggestions to the browser, the picture element gives commands. This gives you control. -->
<picture>
  <img src="image.jpg" alt="A description of the image.">
</picture>
```

- Image formats
```html
<!-- You can specify multiple source elements inside a picture element, each one with its own srcset attribute. The browser then executes the first one that it can. -->
<picture>
  <!-- type told the browser format -->
  <!-- if the browser is capable of rendering -->
  <source srcset="image.avif" type="image/avif">
  <!-- if the browser is capable of rendering -->
  <source srcset="image.webp" type="image/webp">
  <!-- final choose - This means you can start using new image formats without sacrificing backward compatibility.-->
  <img src="image.jpg" alt="A description of the image." 
    width="300" height="200" loading="lazy" decoding="async">
</picture>
```

- Image size
```html
<picture>
  <source srcset="large.png" media="(min-width: 75em)">
  <source srcset="medium.png" media="(min-width: 40em)">
  <img src="small.png" alt="A description of the image." 
    width="300" height="200" loading="lazy" decoding="async">
</picture>
```

```html
<!-- This is different to using the srcset and sizes attributes on the img element. In that case you're providing suggestions to the browser. The source element is more like a command than a suggestion. -->
<picture>
  <source srcset="large.png 1x" media="(min-width: 75em)">
  <source srcset="medium.png 1x, large.png 2x" media="(min-width: 40em)">
  <img src="small.png" alt="A description of the image." width="300" height="200" loading="lazy" decoding="async"
    srcset="small.png 1x, medium.png 2x, large.png 3x">
</picture>
```

- Cropping
```html
<!-- The different images might have different width and height ratios to suit their context better -->
<picture>
  <source srcset="full.jpg" media="(min-width: 75em)" width="1200" height="500">
  <source srcset="regular.jpg" media="(min-width: 50em)" width="800" height="400">
  <img src="cropped.jpg" alt="A description of the image." width="400" height="400" loading="eager" decoding="sync">
</picture>
```

# Icons
- PNGs (and JPGs, WebP, and AVIF) are bitmap images. Bitmap images store information as pixels.

- In an SVG, information is stored as drawing instructions. When the browser reads the SVG file the instructions are converted into pixels. Best of all, these instructions are relative. Regardless of the dimensions you use to describe lines and shapes, everything renders at just the right crispness.

- XML
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE svg>
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="-21 -21 42 42" width="100" height="100">
  <title>Smiling face</title>
  <circle r="20" fill="yellow" stroke="black"/>
  <ellipse rx="2.5" ry="4" cx="-6" cy="-7" fill="black"/>
  <ellipse rx="2.5" ry="4" cx="6" cy="-7" fill="black"/>
  <path stroke="black" d="M -12,5 A 13.5,13.5,0 0,0 12,5 A 13,13,0 0,1 -12,5"/>
</svg>
```

```html
<!-- If you put the SVG inside your HTML, use the aria-hidden attribute to hide it from assistive technology. -->
<button class="menu">
  <svg aria-hidden="true" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 80" width="16" height="16">
    <rect width="100" height="20" />
    <rect y="30" width="100" height="20"/>
    <rect y="60" width="100" height="20"/>
  </svg>
  Menu
</button>
```
<i style="color: red">If you use the same icon multiple times in one page, it would be inefficient to repeat the entire SVG markup each time. There's an element in SVG called use which allows you to “clone” part of an SVG, even from a different SVG element.</i>

```html
<!-- Use the aria-label attribute to provide the accessible name -->
<!-- If you put the SVG inside your HTML, use the aria-label attribute on the link or button to give it an accessible name and use the aria-hidden attribute on the icon. -->
<button aria-label="Menu">
  <svg aria-hidden="true" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 80" width="16" height="16">
    <rect width="100" height="20" />
    <rect y="30" width="100" height="20"/>
    <rect y="60" width="100" height="20"/>
  </svg>
</button>
```

# Theming

- Customize the browser interface
```html
<meta name="theme-color" content="#00D494">
<!-- You can also specify a theme color in a web app manifest file. -->
<link rel="manifest" href="/manifest.json">
```

- Provide a dark mode
```css
@media (prefers-color-scheme: dark) {
  // Styles for a dark theme.
}

```
```html
<!-- You can also use the prefers-color-scheme media feature inside SVG -->
<meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)">
```

- images
```css
/* If you are using SVGs in your HTML, you can apply custom properties there too. */
svg {
  stroke: var(--ink-color);
  fill: var(--page-color);
}
```

```css
/* If you want to tone down the brightness of your photographic images when displayed in dark mode, you can apply a filter in CSS. */
@media (prefers-color-scheme: dark) {
  img {
    filter: brightness(.8) contrast(1.2);
  }
}
```
```html
/* For some images, you might want to swap them out completely in dark mode. */
<picture>
  <source srcset="darkimage.png" media="(prefers-color-scheme: dark)">
  <img src="lightimage.png" alt="A description of the image.">
</picture>
```

- Forms
```css
html {
  color-scheme: light;
}
@media (prefers-color-scheme: dark) {
  html {
    color-scheme: dark;
  }
}
/* So you support dark theme, but all the form inputs are still light themed. What can you do? */
html { color-scheme: light dark; }
```
```html
<!-- You can also use HTML. Add this in the head of your documents: -->
<meta name="supported-color-schemes" content="light dark">
```
```css
/* Use the accent-color property in CSS to style checkboxes, radio buttons, and some other form fields. */
html {
  accent-color: red;
}
```

# Accessibility

- color contrast

- font size

- Keyboard navigation
```css
a:focus,
a:hover {
  outline: 1px dotted;
}
a:focus-visible {
  outline: 3px solid;
}
```
- Headings
```html
<h1>Welcome to my page</h1>
  <h2>About me</h2>
    <h3>My childhood</h3>
  <h2>About this website</h2>
    <h3>How this site was built</h3>
```

- Structure
```html
<header>...</header>
<nav>...</nav>
<main>...</main>
<aside>...</aside>
<footer>...</footer>
```

- Forms
```html
<label for="name">Your name</label>
<input id="name" type="text">
```

- Images
```html
<img src="dog.jpg" alt="A golden retriever sitting on the grass looking happy.">
```
<i style='color: orange'>Using sensible semantic HTML will make your web pages more accessible to assistive technologies like screen readers and also to other audio outputs like voice assistants.
</i>

- ARIA - ARIA stands for Accessible Rich Internet Applications. Its vocabulary allows you to provide semantic information when there isn't a suitable HTML element available.


# Interaction
- `pointer` value of `none` the user might be using a keyboard 

- `pointer` value of `coarse` it means the primary input mechanism isn’t very accurate. A finger on a touchscreen is a coarse pointer.

- `pointer` value of `fine` it means the primary input mechanism is capable of fine-grained control. A mouse or stylus is a fine pointer.

```css
/* In this example, buttons are made larger for coarse pointers: */
button {
  padding: 0.5em 1em;
}
@media (pointer: coarse) {
  button {
    padding: 1em 2em;
  }
}
/*
The any-pointer differs from the pointer media feature in that it reports if any pointing device passes the test.

In this example, if the device has both a fine and a coarse input mechanism, the coarse styles are applied.
 */
@media (any-pointer: fine) {
  button {
    padding: 0.5em 1em;
  }
}
@media (any-pointer: coarse) {
  button {
    padding: 1em 2em;
  }
}
```

- Hover
```css
/* In this example, some supplementary icon is available on hover but only if the primary input device is capable of hovering over an element. */
button .extra {
  visibility: visible;
}
@media (hover: hover) {
  button .extra {
    visibility: hidden;
  }
  button:hover .extra {
    visibility: visible;
  }
}

/* But anyone using a keyboard while a mouse is attached won’t get the benefit of the :hover styles. So it’s a good idea to combine :hover and :focus styles to cover both interactions. */
button .extra {
  visibility: visible;
}
@media (hover: hover) {
  button .extra {
    visibility: hidden;
  }
  button:is(:hover, :focus) .extra {
    visibility: visible;
  }
}

/* Just as any-pointer reports on any of the input mechanisms, any-hover will be true if any of the available input mechanisms are capable of hovering over elements. */
button .extra {
  visibility: hidden;
}
button:hover .extra,
button:focus .extra {
  visibility: visible;
}
@media (any-hover: none) {
  button .extra {
    visibility: visible;
  }
}
```

- Input types
```html
  <label for="email">Email</label>
  <input type="email" id="email">
  <label for="number">Number</label>
  <input type="number" id="number">
  <label for="tel">Tel</label>
  <input type="tel" id="tel">
  <label for="url">URL</label>
  <input type="url" id="url">
```

- Input modes
```html
<!-- The inputmode attribute gives you fine-grained control over virtual keyboards. -->
<label for="age">Age</label>
<input type="number" id="age" inputmode="numeric">
<label for="price">Price</label>
<input type="number" id="price" inputmode="decimal">
```

- Autocomplete
```html
<!--  The autocomplete attribute provides you with a host of options for improving contact forms, log-in forms, and checkout forms.
 -->
<label for="name">Name</label>
<input type="text" id="name" autocomplete="name">
<label for="country">Country</label>
<input type="text" id="country" autocomplete="country">
<label for="email">Email</label>
<input type="email" id="email" autocomplete="email">
```

# User interface patterns

- Navigation
1.  If you have a relatively small number of items, you can style the navigation to look good on small screens.
![Alt text](https://web.dev/static/learn/design/ui-patterns/image/the-same-website-five-na-5068e5d65921e_1920.png)

2. One possible solution is to keep links on one line but truncate the list at the edge of the screen. Users can swipe horizontally to reveal the links that aren't immediately visible. This is the overflow pattern.
<img src='./pattern.png'>

3. As a last resort you could choose to have your navigation hidden by default and provide a toggle mechanism for users to show and hide the contents. This is called progressive disclosure.
![Alt text](https://web.dev/static/learn/design/ui-patterns/image/the-same-website-five-na-382970acfbe0f_1920.png)

- Carousels
```css
/* For narrow screens, display items in a row using flexbox. The row of items will extend beyond the edge of the screen. Use overflow-x: auto to allow horizontal swiping. */

/* The logical version of overflow-x is overflow-inline which would better match with the value for scroll-snap-type. This example uses the physical version for better cross-browser support. */

/* The scroll-snap properties ensure that the items can be swiped in a way that feels smooth. Thanks to scroll-snap-type: inline mandatory, the items snap into place. */
@media (max-width: 50em) {
  .cards {
    display: flex;
    flex-direction: row;
    overflow-x: auto;
    scroll-snap-type: inline mandatory;
    scroll-behavior: smooth;
  }
  .cards .card {
    flex-shrink: 0;
    flex-basis: 15em;
    scroll-snap-align: start;
  }
}
```
<img src='./pattern2.png'/>

```css
/* When the screen is large enough—wider than 50em in this case—switch over to grid and display the items in rows and columns, without hiding anything. */
@media (min-width: 50em) {
  .cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
  }
}
```
<i style='color: orange'>Again, the scroll-snap properties ensure that the interaction feels smooth. Also, notice that the images in the carousel have loading="lazy" applied to them. In this case, the images aren't below the fold, they're beyond the edge, but the same principle applies: if the user never swipes as far as that item, the image won't be downloaded, saving on bandwidth.</i>
- more
https://codepen.io/argyleink/pen/bGgyOGP

- Data tables
- You can apply the overflow pattern to tables. In this example, the table is wrapped in a div with a class of table-container.
```css
.table-container {
  max-inline-size: 100%;
  overflow-x: auto;
  scroll-snap-type: inline mandatory;
  scroll-behavior: smooth;
}
.table-container th, 
.table-container td {
  scroll-snap-align: start;
  padding: var(--metric-box-spacing);
}
```