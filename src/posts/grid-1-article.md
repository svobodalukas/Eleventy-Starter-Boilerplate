---
title: 'Start with the CSS Grid'
description: Is Flexbox your daily business, but CSS Grid is still waiting behind your coding door? Let's try one real use case where the CSS Grid is the best choice.
date: 2021-01-08T00:00:00Z
---

# Let's start with the CSS Grid

Is Flexbox your daily business, but CSS Grid is still waiting behind your coding door? Let's try one real use case where the CSS Grid is the best choice.

You probably know that CSS Grid allows you to work in two-dimensional layout. It means arranging elements in multiple rows and columns. Compared to that well known Flexbox works only in rows or columns.

This is one example from one of my projects:
[... img design ...]

You can imagine it as a stage for article teasers with the image on the background. From the accessibility point is important to stick with the order of elements in HTML.

Is it possible to do this without CSS Grid? Easy question if you don't know the CSS Grid yet. So what do you think?

Flexbox will be probably your first choice (some wrapping divs needed) or old-school absolute positioning could be the way. Maybe some magic with float could be possible too. But it will not be a nice code at all. A lot of calc functions and div elements needed only for styling, right?

Let's Grid it.

This is our simplified HTML code:

```html
<div class="teasers">
  <div class="teaser teaser--main">1</div>
  <div class="teaser">2</div>
  <div class="teaser">3</div>
  <div class="teaser">4</div>
  <div class="teaser">5</div>
</div>
```

The mobile version is quite easy to do. Simple divs below each other. Let's skip it and focus on the bigger resolution. We'll start with the definition of the grid layout:

```css
.teasers {
  display: grid; /* no magic happens, just starts grid format context */
  grid-template-columns: 1fr 2fr 1fr;  /* 3 columns, the second one is twice as big as others */
  grid-template-rows: 1fr 1fr 1fr; /* 3 rows with the same width */
  gap: 1rem; /* grid inner spacing */
}
```

Oh wait, what's the *fr*?  **fr** means *fraction* and it's similar to flexbox values for *flex-grow* and *flex-shrink*. [Definition says](https://www.w3.org/TR/css3-grid-layout/#fr-unit): represents a fraction of the leftover space in the grid container. Read more about it in great [article on CSS tricks](https://css-tricks.com/introduction-fr-css-unit/).

Now it's a time to focuse on the main teaser and define its position.

```css
.teaser--main {
  grid-column: 2;
  grid-row: 1 / 4;
}
```

The column definition is clear - the main teaser will be placed in the second column. Definition of the row could be read as *start the teaser on the top and end it on the fourth grid line*.

[... image with placing main teser and grid lines ...]

Now we have to place the rest of teasers:

```css
.grid--1 .item--3 {
  grid-row: start / third;
  grid-column: 3;
}

.grid--1 .item--4 {
  grid-row: second / end;
}
```

## Result

[... codepen ...]

Let me sum it up. The most important part was the definition of the grid layout by *grid-template-columns/rows* properties. It's similar to table definition in Google Docs or MS Word. You have to define a number of columns and rows that you want. Rows and columns could have different heights and widths defined by various units like rem, px, %, or fr.

Then we define the placement of grid children by *grid-column/row*. We can select the exact column/row or define its size by starts and end track position.

There are more ways how to write grid template definitions or place the grid child elements. As always in the CSS. I'll show you in one of the next articles.
