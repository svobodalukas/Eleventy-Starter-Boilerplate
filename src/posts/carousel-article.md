---
title: 'No JavaScript: Carousel'
description: The previous article was about using target pseudo-class to controll components like modal or navigation without javascript. I would like to show you something similar today - simple carousel component created by HTML&CSS only.
date: 2021-02-15T00:00:00Z
---

# No JavaScript: Carousel

The previous article was about using target pseudo-class to controll components like modal or navigation without javascript. I would like to show you something similar today - simple carousel component created by HTML&CSS only.

[... diagram carousel ...]

HTML code is just list of items. There could be images or anything else as a content inside.

```html
<ul class="carousel">
  <li class="carousel__item">Item 1</li>
  <li class="carousel__item">Item 2</li>
  <li class="carousel__item">Item 3</li>
  <li class="carousel__item">Item 4</li>
  <li class="carousel__item">Item 5</li>
  <li class="carousel__item">Item 6</li>
  <li class="carousel__item">Item 7</li>
  <li class="carousel__item">Item 8</li>
</ul>
```

Creating scrollable element with elements inside is not a rocket science:

```css
/* Carousel */
.carousel {
  display: flex;
  overflow-x: auto;
}
```

[... codepen ...]

Our carousel element is now horizontally scrollable. It works fine, but the difference between this simple solution and "real" carousel is the snapping of items during swipe.

## scroll-snap property

As you expect, CSS has a solution - scroll-snap property:

```css
.carousel {
  ...
  /* definition of axis and behavior  */
  scroll-snap-type: x mandatory;
}

.carousel__item {
  /* box’s snap position  */
  scroll-snap-align: start;
}
```

The [scroll-snap-type](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type) define on which axis you would like to snap and how strict it should be. [Scroll-snap-align](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-align) is about definition of snapping point on the inner element.

The result:
[... codepen ...]

## Further reading
- [CSS Scroll Snap](https://ishadeed.com/article/css-scroll-snap/)