---
title: Media query fundamentals
short-title: Media queries
slug: Learn_web_development/Core/CSS_layout/Media_queries
page-type: learn-module-chapter
sidebar: learnsidebar
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}

The **CSS Media Query** gives you a way to apply CSS only when the browser and device environment matches a rule that you specify, for example "viewport is wider than 480 pixels". Media queries are a key part of [responsive web design](/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design), as they allow you to create different layouts depending on the size of the viewport, but they can also be used to detect other things about the environment your site is running on, for example whether the user is using a touchscreen rather than a mouse.

In this lesson, you will first learn about the syntax used in media queries, and then move on to use them in examples showing how a basic design might be made responsive.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisites:</th>
      <td>
        <a href="/en-US/docs/Learn_web_development/Core/Structuring_content"
          >Structuring content with HTML</a
        >,
        <a href="/en-US/docs/Learn_web_development/Core/Styling_basics">CSS Styling basics</a>,
        <a href="/en-US/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fundamental text and font styling</a>,
        familiarity with <a href="/en-US/docs/Learn_web_development/Core/CSS_layout/Introduction">CSS layout fundamental concepts</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Learning outcomes:</th>
      <td>
        <ul>
          <li>The syntax of media queries.</li>
          <li>The common types of media query.</li>
          <li>Using <code>width</code> and <code>height</code> media queries to create responsive layouts.</li>
          <li>Choosing breakpoints.</li>
          <li>Using media queries to implement a mobile-first design.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Media query basics

The simplest media query syntax looks like this:

```css
@media media-type and (media-feature-rule) {
  /* CSS rules go here */
}
```

It consists of:

- A media type, which tells the browser what kind of media this code is for (print or screen).
- A media expression, which is a rule, or test that must be passed for the contained CSS to be applied.
- A set of CSS rules that will be applied if the test passes and the media type is correct.

### Media types

The possible types of media you can specify are:

- `all`
- `print`
- `screen`

The following media query will only set the body to 12pt if the page is printed. It will not apply when the page is loaded in a browser.

```css
@media print {
  body {
    font-size: 12pt;
  }
}
```

> [!NOTE]
> The media type here is different from the so-called {{glossary("MIME type")}}.
> There were a number of other media types defined in the Level 3 Media Queries specification; these have been deprecated and should be avoided.
> Media types are optional; if you do not indicate a media type in your media query, then the media query will default to being for all media types.

### Media feature rules

After specifying the type, you can then target a media feature with a rule.
The following examples show how to use different media queries.
To change the `width` of your screen, change the size of your browser or rotate your handheld device.

> [!NOTE]
> Alternatively, you can use browser developer tools' responsive sizing features (such as Firefox [Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/)) to simulate different device widths.

#### Width and height

The feature we tend to detect most often in order to create responsive designs (and that has widespread browser support) is viewport width, and we can apply CSS if the viewport is above or below a certain width — or an exact width — using the `min-width`, `max-width`, and `width` media features.

These features are used to create layouts that respond to different screen sizes. For example, to set the body text color to red if the viewport is exactly 600 pixels, you would use the following media query.

```css live-sample___width
@media screen and (width: 600px) {
  body {
    color: red;
  }
}
```

```html live-sample___width
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("width")}}

Try adjusting the browser window width to see if you can find the sweet spot where the above demo is exactly `600px` wide so the text will turn red.

The `width` (and `height`) media features can be used as ranges, and therefore be prefixed with `min-` or `max-` to indicate that the given value is a minimum, or a maximum. For example, to make the color blue if the viewport is 600 pixels or narrower, use `max-width`:

```css live-sample___max-width
@media screen and (max-width: 600px) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___max-width
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("max-width")}}

Try narrowing the window until the above text turns blue.

In practice, using minimum or maximum values is much more useful for responsive design so you will rarely see `width` or `height` used alone.

There are many other media features that you can test for, although some of the newer features introduced in Levels 4 and 5 of the media queries specification have limited browser support. Each feature is documented on MDN along with browser support information, and you can find a complete list at [Using Media Queries: Syntax](/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries#syntax).

#### Orientation

One well-supported media feature is `orientation`, which allows us to test for portrait or landscape orientation. To change the body text color if the device is in landscape orientation, use the following media query.

```css live-sample___orientation
@media (orientation: landscape) {
  body {
    color: rebeccapurple;
  }
}
```

```html hidden live-sample___orientation
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("orientation")}}

The above example is pretty difficult to test in-page; to see it in action, you are advised the copy the above code into a local HTML file, and open it in its own tab.

A standard desktop view has a landscape orientation, and a design that works well in this orientation may not work as well when viewed on a phone or tablet in portrait mode. Testing for orientation can help you to create a layout which is optimized for devices in portrait mode.

#### Use of pointing devices

As part of the Level 4 specification, the `hover` media feature was introduced. This feature means you can test if the user has the ability to hover over an element, which essentially means they are using some kind of pointing device; touchscreen and keyboard navigation does not hover.

```css live-sample___hover-example
@media screen and (hover: hover) {
  body:hover {
    color: white;
    background: black;
  }
}
```

```html hidden live-sample___hover-example
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("hover-example")}}

The above example changes to white text on black when hovered over, but only on devices where hovering is possible. If we know the user cannot hover, we could display some interactive features by default. For users who can hover, we might choose to make them available when a link is hovered over.

Also in Level 4 is the `pointer` media feature. This takes three possible values, `none`, `fine` and `coarse`. A `fine` pointer is something like a mouse or trackpad. It enables the user to precisely target a small area. A `coarse` pointer is your finger on a touchscreen. The value `none` means the user has no pointing device; perhaps they are navigating with the keyboard only or with voice commands.

Using `pointer` can help you to design better interfaces that respond to the type of interaction a user is having with a screen. For example, you could create larger hit areas if you know that the user is interacting with the device as a touchscreen.

### Using ranged syntax

One common case is to check if the viewport width is between two values:

```css
@media (min-width: 30em) and (max-width: 50em) {
  /* … */
}
```

If you want to improve the readability of this, you can use "range" syntax:

```css
@media (30em <= width <= 50em) {
  /* … */
}
```

So in this case, styles are applied when the viewport width is between `30em` and `50em`.

## More complex media queries

With all of the different possible media queries, you may want to combine them, or create lists of queries — any of which could be matched.

As before, try testing the examples in this section by adjusting your browser width.

### "and" logic in media queries

To combine media features you can use `and` in much the same way as we have used `and` above to combine a media type and feature. For example, we might want to test for a `min-width` and `orientation`. The body text will only be blue if the viewport is at least 600 pixels wide and the device is in landscape mode.

```css live-sample___and
@media screen and (min-width: 600px) and (orientation: landscape) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___and
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("and")}}

### "or" logic in media queries

If you have a set of queries, any of which could match, then you can comma separate these queries. In the below example the text will be blue if the viewport is at least 600 pixels wide OR the device is in landscape orientation. If either of these things are true the query matches.

```css live-sample___or
@media screen and (min-width: 600px), screen and (orientation: landscape) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___or
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("or")}}

### "not" logic in media queries

You can negate an entire media query by using the `not` operator. This reverses the meaning of the entire media query. Therefore in this next example the text will only be blue if the viewport is _not_ at least 600 pixels wide.

```css live-sample___not
@media not (min-width: 600px) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___not
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("not")}}

You can also use `not` to negate specific expressions.

```css
@media (not (width < 600px)) and (not (width > 1000px)) {
  body {
    color: blue;
  }
}
```

This will apply the styles if the viewport width is between 600 and 1000 pixels. This is equivalent to `(600px <= width <= 1000px)`.

## How to choose breakpoints

In the early days of responsive design, many designers would attempt to target very specific screen sizes. Lists of screen sizes of popular phones and tablets were published in order that designs could be created to neatly match those viewports.

There are now far too many devices, with a huge variety of sizes, to make that feasible. This means that instead of targeting specific sizes for all designs, a better approach is to change the design at the size where the content starts to break in some way. Perhaps the line lengths become far too long, or a sidebar gets squashed and hard to read. That's the point at which you want to use a media query to change the design to a better one for the space you have available. This approach means that it doesn't matter what the exact dimensions are of the device being used; every range is catered for. The points at which a media query is introduced are known as **breakpoints**.

[Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) in Firefox DevTools is very useful for working out where these breakpoints should go. You can easily make the viewport smaller and larger to see where the content would be improved by adding a media query and tweaking the design.

![A screenshot of a layout in a mobile view in Firefox DevTools.](rwd-mode.png)

## Mobile-first responsive design

Broadly, you can take two approaches to responsive design. You can start with your desktop or widest view and then add breakpoints to move things around as the viewport becomes smaller, or you can start with the smallest view and add layout as the viewport becomes larger. This second approach is described as **mobile first** responsive design and is quite often the best approach to follow.

The view for the very smallest devices is quite often a simple single column of content, much as it appears in normal flow. This means that you probably don't need to do a lot of layout for small devices — order your source well and you will have a readable layout by default.

## Creating your own mobile-first design

Now it's your turn; in this tutorial section you will build up your own basic mobile-first responsive design. In a production site you are likely to have more things to adjust within your media queries, however the approach will be exactly the same.

### Getting started

Our starting point is an HTML document with some CSS applied to add background colors to the various parts of the layout.

First of all, copy the HTML code from the block below into a text editor, save it as an HTML file on your computer, and open it in your browser:

```html live-sample___walkthrough
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Media Queries: a simple mobile first design, step 1</title>
  <style>
    /* Add styles here */
  </style>
</head>
<div class="wrapper">
  <header>
    <nav>
      <ul>
        <li><a href="">About</a></li>
        <li><a href="">Contact</a></li>
        <li><a href="">Meet the team</a></li>
        <li><a href="">Blog</a></li>
      </ul>
    </nav>
  </header>
  <main>
    <article>
      <div class="content">
        <h1>Veggies!</h1>
        <p>
          Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh
          onion daikon amaranth tatsoi tomatillo melon azuki bean garlic.
        </p>

        <p>
          Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot
          courgette tatsoi pea sprouts fava bean collard greens dandelion okra
          wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
        </p>

        <p>
          Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
          kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus
          winter purslane kale. Celery potato scallion desert raisin horseradish
          spinach carrot soko. Lotus root water spinach fennel kombu maize
          bamboo shoot green bean swiss chard seakale pumpkin onion chickpea
          gram corn pea. Brussels sprout coriander water chestnut gourd swiss
          chard wakame kohlrabi beetroot carrot watercress. Corn amaranth
          salsify bunya nuts nori azuki bean chickweed potato bell pepper
          artichoke.
        </p>

        <p>
          Nori grape silver beet broccoli kombu beet greens fava bean potato
          quandong celery. Bunya nuts black-eyed pea prairie turnip leek lentil
          turnip greens parsnip. Sea lettuce lettuce water chestnut eggplant
          winter purslane fennel azuki bean earthnut pea sierra leone bologi
          leek soko chicory celtuce parsley jícama salsify.
        </p>
      </div>
      <aside class="related">
        <p>
          All these veggies are brought to you by the
          <a href="https://veggieipsum.com/">Veggie Ipsum generator</a>.
        </p>
      </aside>
    </article>
    <aside class="sidebar">
      <h2>External vegetable-based links</h2>
      <ul>
        <li>
          <a
            href="https://www.thekitchn.com/how-to-cook-broccoli-5-ways-167323">
            How to cook broccoli
          </a>
        </li>
        <li>
          <a href="https://www.bbcgoodfood.com/glossary/swiss-chard">
            Swiss Chard
          </a>
        </li>
        <li>
          <a
            href="https://www.bbcgoodfood.com/recipes/collection/christmas-parsnip">
            Christmas Parsnip Recipes
          </a>
        </li>
      </ul>
    </aside>
  </main>

  <footer>
    <p>&copy; 2024</p>
  </footer>
</div>
```

The source of the document is ordered in a way that makes the content readable. This is an important first step and one which ensures that if the content were to be read out by a screen reader, it would be understandable.

The initial styles for our example are as follows; copy these into your HTML file inside the `<style></style>` tags, replacing the `/* Add styles here */` comment.

```css live-sample___walkthrough
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  margin: 2em auto;
  font:
    1em/1.3 Arial,
    Helvetica,
    sans-serif;
}

a:link,
a:visited {
  color: #333;
}

nav ul,
aside ul {
  list-style: none;
  padding: 0;
}

nav a:link,
nav a:visited {
  background-color: rgb(207 232 220 / 20%);
  border: 2px solid rgb(79 185 227);
  text-decoration: none;
  display: block;
  padding: 10px;
  color: #333;
  font-weight: bold;
}

nav a:hover {
  background-color: rgb(207 232 220 / 70%);
}

.related {
  background-color: rgb(79 185 227 / 30%);
  border: 1px solid rgb(79 185 227);
  padding: 10px;
}

.sidebar {
  background-color: rgb(207 232 220 / 50%);
  padding: 10px;
}

article {
  margin-bottom: 1em;
}
```

If you view the layout in Responsive Design Mode in DevTools, or narrow your browser window down to a mobile-like width, you'll see that it works pretty well as a straightforward mobile view of the site.

{{EmbedLiveSample("walkthrough", "", "600px")}}

### Creating a two-column layout for medium widths

Drag the window wider until you can see that the line lengths are becoming quite long; at this point you've got space for the navigation to display in a horizontal line. This is where we'll add our first media query. We'll use `em` units, as this will mean that if the user has increased their text size, the breakpoint will happen at a similar line-length but wider viewport, than someone with a smaller text size.

Add the following to the bottom of your CSS:

```css
@media screen and (min-width: 40em) {
  article {
    display: grid;
    grid-template-columns: 3fr 1fr;
    column-gap: 20px;
  }

  nav ul {
    display: flex;
  }

  nav li {
    flex: 1;
  }
}
```

This CSS gives us a two-column layout inside the `<article>`, of the article content and related information in the `<aside>` element. We have also used flexbox to put the navigation into a row.

### Adding a third column for wider screens

Let's continue to expand the width until we feel there is enough room for the sidebar to also form a new column. Inside a media query we'll make the `<main>` element into a two column grid. We then need to remove the {{cssxref("margin-bottom")}} on the article so that the two sidebars align with each other, and we'll add a {{cssxref("border")}} to the top of the footer. Typically these small tweaks are the kind of thing you will do to make the design look good at each breakpoint.

Add the following to the bottom of your CSS:

```css
@media screen and (min-width: 70em) {
  main {
    display: grid;
    grid-template-columns: 3fr 1fr;
    column-gap: 20px;
  }

  article {
    margin-bottom: 0;
  }

  footer {
    border-top: 1px solid #ccc;
    margin-top: 2em;
  }
}
```

That's the example finished. If you look at the result at different widths you can see how the design responds and works as a single column, two columns, or three columns, depending on the available width. This is a basic example of a mobile-first responsive design.

### viewport meta

If you look at the HTML source in the above example, you'll see the following element included in the head of the document:

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

This is the [`viewport`](/en-US/docs/Web/HTML/Reference/Elements/meta/name/viewport) meta tag — it exists as a way to control how mobile browsers render content, making sure they respect your media queries. The one above tells mobile browsers "don't render the content with a 980-pixel viewport — render it using the real device width instead, and set a default initial scale level for better consistency." The media queries will then kick in as expected.

For more information on why this is needed, see [The viewport meta tag](/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design#the_viewport_meta_tag) section in the previous article.

## Do you really need a media query?

Flexbox and CSS Grid give you ways to create flexible and even responsive components without the need for a media query: It's always worth considering whether you really need one. For example, you might want a set of cards that are at least 200 pixels wide, and fit as many of these 200 pixels across the main content column as possible, regardless of how wide it is.

This can be achieved with CSS Grid, using no media queries at all:

```html live-sample___grid
<ul class="grid">
  <li>
    <h2>Card 1</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 2</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 3</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 4</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 5</h2>
    <p>…</p>
  </li>
</ul>
```

```css live-sample___grid
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}

.grid li {
  border: 1px solid #666;
  padding: 10px;
}
```

{{EmbedLiveSample("grid", "", "350px")}}

Try making your browser window wider and narrower to see the number of column tracks change.

The nice thing about this method is that grid is not looking at the viewport width, but the width it has available for this component. It might seem strange to wrap up a section about media queries with a suggestion that you might not need one at all! However, in practice you will find that good use of modern layout methods, enhanced with media queries, will give the best results.

## Test your skills

You've reached the end of this article, but can you remember the most important information? You can find a test to verify that you've retained this information before you move on — see [Test your skills: Responsive web design and media queries](/en-US/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design).

## Summary

In this lesson you have learned about media queries, and also discovered how to use them in practice to create a mobile first responsive design.

You could use the starting point that we have created to test out more media queries. For example, perhaps you could change the size of the navigation if you detect that the visitor has a coarse pointer, using the `pointer` media feature.

You could also experiment with adding different components and seeing whether the addition of a media query, or using a layout method like flexbox or grid is the most appropriate way to make the components responsive. Very often there is no right or wrong way — you should experiment and see which works best for your design and content.

OK, we're nearly at the end of this module. Let's finish off by giving you a challenge to test your wider understanding of CSS layout.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}
