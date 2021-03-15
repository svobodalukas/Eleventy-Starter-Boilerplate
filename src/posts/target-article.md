---
title: 'No JavaScript: Modal dialog'
description: I'll show you how to code few simple components without any line of JavaScript. Let's start with the very common component - modal window.
date: 2021-02-20T00:00:00Z
---

# No JavaScript: Modal dialog

In the frontend developer life are sometimes special moments when JavaScript can't be used. Or you just don't want to use it for something pretty simple. I'll show you how to code few simple components without any line of JavaScript. Let's start with the very common component: modal window (or popup, alert, flash message as you wish).

The basic markup of the dialog could look like this:

```html
<a href="#" class="btn">Open Modal</a>

<div class="modal">
  <div class="modal__overlay"></div>
  <div class="modal__dialog">
    <a href="#" class="modal__close" title="close dialog">
      <svg width="24" height="24" viewBox="0 0 24 24">
        <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
      </svg>
    </a>
    <div class="modal__content">Hey! This is the modal dialog with some content.</div>
    <div class="modal__footer">
      <a href="#" class="btn">
        Okay, got it
      </a>
    </div>
  </div>
</div>
```

```css
/* Wrapper, hidden by default */
.modal {
  display: none;
  z-index: 900;
}

/* Black page overlay with opacity */
.modal-overlay {
  display: none;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0,0,0, .75);
}

/* Modal dialog itself */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 90vw;
  max-width: 20rem;
  background-color: #fff;
}
```

It's simple styling with a fixed position, centering on the page and one z-index. Technically, there is no JavaScript magic needed except the triggering and closing of the modal dialog.

## :target do the magic

We will use [:target](https://developer.mozilla.org/en-US/docs/Web/CSS/:target) pseudo-class for modal behavior control:

```css
/* show modal when its id is in the page url  */
.modal:target {
  display: block;
}
```

We have to add ids to our html code:

```html
<a href="#my-modal" class="btn">Open Modal</a>
<div class="modal" id="my-modal">
    ...
</div>    
```

How the whole example looks like:
[... codepen ...]

## Other use cases
This pseudo-class is really handy, right? May be you could imagine other cases in your projects like collapsible navigation or simple tabs: 

Navigation:
[... codepen ...]

Simple tabs:
[... codepen ...]

## Further reading
- [CSS Only Modal using target](https://dev.to/mandrewdarts/css-only-modal-using-target-5ac7)