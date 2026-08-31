# HTML and CSS Technical Paper

## 1. Introduction

HTML and CSS are the basic technologies used to build web pages.

-   HTML gives a page its structure and meaning.
-   CSS controls how that structure looks and how it is arranged.
-   HTML is mainly about content and semantics.
-   CSS is mainly about presentation and layout.

A simple way to remember it:

``` text
HTML → What is on the page?
CSS  → How should it look and behave?
```

------------------------------------------------------------------------

## 2. Basic HTML Structure

A basic HTML document looks like this:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">
    <title>My Page</title>
</head>
<body>
    <h1>Hello</h1>
    <p>This is my web page.</p>
</body>
</html>
```

Important parts:

-   `<!DOCTYPE html>` tells the browser to use modern HTML.
-   `<html>` is the root element.
-   `<head>` contains page information and resources.
-   `<body>` contains visible page content.

### Semantic HTML

Semantic elements describe their purpose:

``` html
<header>
<nav>
<main>
<section>
<article>
<aside>
<footer>
```

Using semantic HTML makes the document easier to understand and is
useful for accessibility.

------------------------------------------------------------------------

## 3. Common HTML Elements

  Element       Purpose
  ------------- --------------------------
  `h1` - `h6`   Headings
  `p`           Paragraph
  `a`           Link
  `img`         Image
  `ul`          Unordered list
  `ol`          Ordered list
  `li`          List item
  `div`         Generic block container
  `span`        Generic inline container
  `form`        Form
  `input`       Input field
  `button`      Button
  `table`       Tabular data

Example:

``` html
<h1>Courses</h1>

<ul>
    <li>Python</li>
    <li>SQL</li>
    <li>HTML</li>
</ul>

<a href="https://example.com">Visit site</a>
```

------------------------------------------------------------------------

## 4. CSS Syntax

CSS uses a selector followed by declarations.

``` css
selector {
    property: value;
}
```

Example:

``` css
h1 {
    color: red;
    font-size: 32px;
}
```

Here:

-   `h1` is the selector.
-   `color` and `font-size` are properties.
-   `red` and `32px` are values.

CSS can be written inline, internally, or in an external stylesheet. For
normal projects, an external stylesheet is usually easier to maintain.

``` html
<link rel="stylesheet" href="style.css">
```

------------------------------------------------------------------------

## 5. CSS Selectors

Selectors tell CSS which HTML elements should be styled.

### Element selector

``` css
p {
    color: blue;
}
```

Selects all paragraphs.

### Class selector

``` css
.card {
    padding: 20px;
}
```

``` html
<div class="card">Product</div>
```

Classes can be reused.

### ID selector

``` css
#header {
    background-color: black;
}
```

An ID identifies a particular element.

### Descendant selector

``` css
.card p {
    color: gray;
}
```

Selects paragraphs inside `.card`.

### Child selector

``` css
.card > p {
    color: red;
}
```

Selects only direct child paragraphs.

### Attribute selector

``` css
input[type="text"] {
    border: 1px solid gray;
}
```

### Pseudo-class

``` css
button:hover {
    background-color: blue;
}
```

Applies when the button is hovered.

------------------------------------------------------------------------

## 6. CSS Box Model

The box model is one of the most important CSS concepts.

Every element can be understood as a box:

``` text
        MARGIN
┌───────────────────────────────┐
│          BORDER               │
│   ┌───────────────────────┐   │
│   │       PADDING         │   │
│   │   ┌───────────────┐   │   │
│   │   │    CONTENT    │   │   │
│   │   └───────────────┘   │   │
│   └───────────────────────┘   │
└───────────────────────────────┘
```

The four parts are:

1.  Content
2.  Padding
3.  Border
4.  Margin

The most important distinction is:

``` text
padding → space inside the element
margin  → space outside the element
```

Example:

``` css
.box {
    padding: 20px;
    margin: 30px;
    border: 2px solid black;
}
```

### `box-sizing`

A common rule is:

``` css
* {
    box-sizing: border-box;
}
```

With `border-box`, the declared width and height include padding and
border. This makes sizing easier to control.

------------------------------------------------------------------------

## 7. Block, Inline and Inline-Block

### Block elements

Block elements normally start on a new line and use the available width.

Examples:

``` html
<div>
<p>
<h1>
<section>
<header>
<footer>
```

Conceptually:

``` text
┌────────────────────┐
│ First              │
└────────────────────┘
┌────────────────────┐
│ Second             │
└────────────────────┘
```

Width and height normally work on block elements.

### Inline elements

Inline elements stay in the same line when there is enough space.

Examples:

``` html
<span>
<a>
<strong>
<em>
```

Width and height do not normally work on inline elements in the same way
they do on block elements.

### Inline-block

`inline-block` gives a useful combination:

``` css
.box {
    display: inline-block;
    width: 150px;
    height: 100px;
}
```

It can stay beside other inline-level elements while respecting width
and height.

------------------------------------------------------------------------

## 8. Positioning

Important positioning values are `static`, `relative`, `absolute`,
`fixed`, and `sticky`.

### Static

The default:

``` css
.box {
    position: static;
}
```

The element follows normal flow.

### Relative

``` css
.box {
    position: relative;
    top: 20px;
    left: 30px;
}
```

The element moves visually from its normal position, but its original
space remains reserved.

``` text
Normal position
      ↓
Move visually
      ↓
Original space remains
```

### Absolute

A common pattern is:

``` css
.parent {
    position: relative;
}

.child {
    position: absolute;
    top: 0;
    right: 0;
}
```

The absolutely positioned child is removed from normal flow and is
positioned relative to its containing block. A positioned parent is
commonly used as that reference.

### Fixed

``` css
.button {
    position: fixed;
    right: 20px;
    bottom: 20px;
}
```

The element is positioned relative to the viewport and normally stays
there while scrolling.

### Sticky

``` css
header {
    position: sticky;
    top: 0;
}
```

The element behaves normally until the scroll position reaches the
specified threshold.

------------------------------------------------------------------------

## 9. Common CSS Structural Classes

In projects, class names are often used to describe layout or structure:

``` text
.container
.wrapper
.header
.nav
.main
.sidebar
.content
.grid
.row
.column
.card
.footer
```

For example:

``` html
<div class="container">
    <main class="content"></main>
    <aside class="sidebar"></aside>
</div>
```

``` css
.container {
    max-width: 1200px;
    margin: 0 auto;
}
```

These names are conventions chosen by developers. They are not special
CSS keywords.

------------------------------------------------------------------------

## 10. Common CSS Styling Classes

Classes can also describe reusable visual styles:

``` text
.btn
.btn-primary
.card
.text-center
.hidden
.active
.error
.success
```

Example:

``` css
.btn {
    padding: 10px 16px;
    border: none;
    border-radius: 6px;
}

.btn-primary {
    background-color: blue;
    color: white;
}
```

``` html
<button class="btn btn-primary">Submit</button>
```

Here `.btn` provides common button styling and `.btn-primary` provides
the variation.

------------------------------------------------------------------------

## 11. CSS Specificity

When multiple CSS rules match an element, specificity helps decide which
declaration wins, along with the cascade and source order.

A simple order to remember is:

``` text
Inline style
     ↓
ID
     ↓
Class / attribute / pseudo-class
     ↓
Element
```

Example:

``` css
p {
    color: blue;
}

.text {
    color: green;
}

#message {
    color: red;
}
```

``` html
<p id="message" class="text">Hello</p>
```

The text becomes red because the ID selector has higher specificity.

Specificity is not simply "the rule written last". A more specific
selector can win even when it appears earlier.

I should avoid using `!important` unless there is a real reason because
it can make the cascade harder to manage.

------------------------------------------------------------------------

## 12. Flexbox

Flexbox is mainly a one-dimensional layout system.

It is useful when items are arranged mainly in a row or a column.

``` css
.container {
    display: flex;
}
```

``` text
┌─────────────────────────────┐
│  A      B      C      D     │
└─────────────────────────────┘
```

Important properties:

### `flex-direction`

``` css
flex-direction: row;
flex-direction: column;
```

Controls the main direction.

### `justify-content`

Controls distribution along the main axis.

Common values:

``` css
justify-content: flex-start;
justify-content: center;
justify-content: flex-end;
justify-content: space-between;
justify-content: space-around;
justify-content: space-evenly;
```

### `align-items`

Controls alignment on the cross axis.

``` css
align-items: flex-start;
align-items: center;
align-items: flex-end;
align-items: stretch;
```

### `flex-wrap`

Allows items to move to another line:

``` css
flex-wrap: wrap;
```

### `gap`

Adds space between flex items:

``` css
gap: 20px;
```

A common centering pattern is:

``` css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

------------------------------------------------------------------------

## 13. CSS Grid

Grid is mainly a two-dimensional layout system.

It is useful when a design naturally has rows and columns.

``` css
.container {
    display: grid;
}
```

Example:

``` text
        Column 1   Column 2   Column 3
        ┌────────┬──────────┬────────┐
Row 1   │   A    │    B     │   C    │
        ├────────┼──────────┼────────┤
Row 2   │   D    │    E     │   F    │
        └────────┴──────────┴────────┘
```

### `grid-template-columns`

``` css
grid-template-columns: 1fr 2fr 1fr;
```

The available space is divided into four fractions:

``` text
1fr : 2fr : 1fr
25% : 50% : 25%
```

### `grid-template-rows`

``` css
grid-template-rows: 100px 200px;
```

Creates two rows.

### `repeat()`

Instead of:

``` css
grid-template-columns: 1fr 1fr 1fr;
```

I can write:

``` css
grid-template-columns: repeat(3, 1fr);
```

### Grid item positioning

``` css
.item {
    grid-column: 1 / 3;
    grid-row: 1;
}
```

The item spans from column line 1 to column line 3.

### Flexbox vs Grid

A useful starting rule is:

``` text
Flexbox → mainly one direction
Grid    → rows + columns
```

Both are flexible and can sometimes solve the same problem, but this
rule helps me choose quickly.

------------------------------------------------------------------------

## 14. Responsive Design and Media Queries

Responsive design means the layout adapts to different screen sizes.

For example:

``` text
Desktop:
A B C

Mobile:
A
B
C
```

Media queries allow CSS to change depending on the viewport or other
device features.

Example:

``` css
.container {
    display: grid;
    grid-template-columns: 3fr 1fr;
}

@media (max-width: 700px) {
    .container {
        grid-template-columns: 1fr;
    }
}
```

The base CSS is still used. The media query overrides only the
properties that need to change.

``` text
Base CSS
   ↓
Default rules
   +
Media query
   ↓
Only required changes
   =
Final style
```

### Mobile-first approach

Another common approach is to start with the small layout and add
larger-screen rules:

``` css
.container {
    display: block;
}

@media (min-width: 768px) {
    .container {
        display: grid;
        grid-template-columns: 1fr 1fr;
    }
}
```

The important goal is that the page remains usable at different viewport
sizes.

------------------------------------------------------------------------

## 15. CSS Units

### `px`

Useful for a specific size:

``` css
width: 200px;
```

### `%`

Usually relates to the containing block:

``` css
width: 50%;
```

### `rem`

Relative to the root element's font size:

``` css
font-size: 1.5rem;
```

### `em`

Relative to the relevant font size:

``` css
padding: 1em;
```

### Viewport units

``` css
width: 50vw;
height: 50vh;
```

-   `vw` = viewport width
-   `vh` = viewport height

### `fr`

Mainly used with Grid:

``` css
grid-template-columns: 1fr 2fr;
```

It divides available grid space into fractions.

------------------------------------------------------------------------

## 16. Common Header Meta Tags

Useful tags inside `<head>` include:

### Character encoding

``` html
<meta charset="UTF-8">
```

### Viewport

``` html
<meta name="viewport"
      content="width=device-width, initial-scale=1.0">
```

This is important for responsive pages.

### Description

``` html
<meta name="description"
      content="Learning HTML and CSS">
```

### Title

``` html
<title>My Website</title>
```

A common starting `<head>` is:

``` html
<head>
    <meta charset="UTF-8">

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">

    <meta name="description"
          content="My HTML and CSS project">

    <title>My Website</title>

    <link rel="stylesheet" href="style.css">
</head>
```

------------------------------------------------------------------------

## 17. Normal Flow

Before using Flexbox, Grid or positioning, HTML elements normally follow
normal flow.

For example:

``` html
<div>A</div>
<div>B</div>
<div>C</div>
```

Block elements normally appear one below another:

``` text
A
↓
B
↓
C
```

I should first understand the normal flow and then decide whether I
actually need another layout system.

------------------------------------------------------------------------

## 18. Useful CSS Reset

Browsers have default styles. One simple reset I often use is:

``` css
* {
    box-sizing: border-box;
}

html,
body {
    margin: 0;
    padding: 0;
}
```

This removes the default page margin and makes box sizing easier to
control.

------------------------------------------------------------------------

## 19. How I Choose a Layout

When I get a layout problem, I can think in this order:

``` text
Look at the target
       ↓
Is normal flow enough?
       ↓
      No
       ↓
Is it mainly one row/column?
      ↙       ↘
    Yes        No
     ↓          ↓
 Flexbox      Grid
                 ↓
Need an item independently
positioned over another?
                 ↓
            Positioning
```

Examples:

### Navigation bar

``` text
Logo   Home   About   Contact
```

Flexbox is usually a good choice.

### Dashboard

``` text
┌─────┬─────┬─────┐
│  A  │  B  │  C  │
├─────┼─────┼─────┤
│  D  │  E  │  F  │
└─────┴─────┴─────┘
```

Grid is usually a good choice.

### Badge on a card

``` text
┌────────────────┐
│ Card       NEW │
│                │
│     content    │
└────────────────┘
```

Relative + absolute positioning can be useful.

------------------------------------------------------------------------

## 20. Important CSS Properties Quick List

### Size

``` css
width
height
max-width
min-width
max-height
min-height
```

### Spacing

``` css
margin
padding
gap
```

### Border

``` css
border
border-radius
```

### Text

``` css
color
font-size
font-family
font-weight
text-align
line-height
```

### Background

``` css
background-color
background-image
background-size
background-position
```

### Layout

``` css
display
position
top
right
bottom
left
overflow
```

### Flexbox

``` css
flex-direction
justify-content
align-items
flex-wrap
flex
gap
```

### Grid

``` css
grid-template-columns
grid-template-rows
grid-column
grid-row
gap
```

------------------------------------------------------------------------

# References

1.  MDN Web Docs --- HTML and CSS\
    https://developer.mozilla.org/

2.  Shay Howe --- Learn to Code HTML & CSS\
    https://learn.shayhowe.com/

3.  MountBlue Python-Django Path --- HTML/CSS\
    https://github.com/mountblue/python-django-path/tree/master/html-css
