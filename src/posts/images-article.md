---
title: 'Deep dive into images on the web'
description: I'll show you how to code few simple components without any line of JavaScript. Let's start with the very common component - modal window.
date: 2021-05-24T00:00:00Z
---

# Deep dive into images on the web

Page speed is an important topic during web development and images are mostly the biggest data consumers. Let’s test it on the one page and check, how we can save 0.5MB of data load by few improvements on the one image. We will be talking about image formats, responsivity and one CSS tip will be there for sure!

This is the basic markup of the page with one bigger image. It's a copy of the real page, on which we tried to improve the page speed loading. Bootstrap is used for the basic layout:

```html
<div class="container">
  <h1>City of Zlín</h1>
  <p class="mb-2">Zlín is a city in southeastern Moravia in the Czech Republic. It has about 75,000 inhabitants. It is the seat of the Zlín Region and it lies on the Dřevnice river. The development of the modern city is closely connected to the Bata Shoes company and its social scheme, developed after the World War I. From 1949 to 1990, the city was renamed Gottwaldov. Source: <a href="https://en.wikipedia.org/wiki/Zl%C3%ADn" target="_blank"></a></p>
  
  <img
    src="img/zlin-1200.jpg"
    srcset="img/zlin-1200.jpg,
            img/zlin-2400.jpg 2x"
    alt="Zlín"
    width="1200"
    height="540"
    class="mb-2">
  
  <p class="mb-2">...</p>  
</div>
```

The content of the image is a city landscape, jpeg format is then fine in this case. But for all resolutions is there just only one image 1200 pixels wide (doubled for retinas). The file size is 167 KB for 1200px and 550KB for 2400px wide. The typical user with some mobile device will download **more than 0,5MB just only for one image**. 

Current file size for user with the mobile sized like iPhone 6,7,8,X (375px viewport width) is:
**552 KB** (zlin-2400.jpg file). Let's try to improve user experience in few steps:

## The image checklist

1. Image optimization
2. Responsive image set
3. WebP format
4. AVIF format
5. Different aspect ratio for mobile users

## 1. Image optimization

We don't have any information about image origin, so it will be better to run optimization tasks to check, if the file size could be smaller.

The selection of the right tool depends on your tech stack. Online tools like [Kraken.io](https://kraken.io/web-interface) are the simpliest way to start. But probably you would like to make optimization over all of your images. You could try smaller tools like [Imagemin](https://github.com/imagemin/imagemin) or bigger solutions to cover image management completely ([Thumbor](http://thumbor.org/), [ImageBoss](https://imageboss.me/)).

I tried Imagemin via gulp task and the result for the mobile was:
**518 KB (- 34 KB)** (zlin-2400.jpg)

## 2. Responsive image set

Optimization helped a little. Now we have to think, if there is a smarter way how serve more images depending on the resolution. Yes, we can!

HTML5 attributes `srcset` and `sizes` will help us to control [https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images](responsivity behavior of images). **Srcset** defines possible image sources for the user agent to use and **sizes** contains Media Conditions to describe properties of the viewport.

How to define correct values for scrset and sizes? Let's assume this:

- Image has the same width as the container
- Our layout is based on the [Bootstrap 4 container class](https://getbootstrap.com/docs/4.6/layout/overview/) and has 15px padding on sides
- The typical screen on mobile is somewhere around 360 - 375px wide (check your audience to be sure)
- We need doubled size to avoid blurry images on retina screens.

This is the final html code:

```html
<img
  src="img-dist/zlin-1110.jpg"
  srcset="
  img-dist/zlin-690.jpg 690w,
  img-dist/zlin-1110.jpg 1110w,
  img-dist/zlin-2220.jpg 2220w
  "
  sizes="
  (max-width: 575px) calc(100vw - 30px),
  (max-width: 767px) 510px,
  (max-width: 991px) 690px,
  (max-width: 1199px) 930px,
  1110px"
  alt="Zlín" class="m-2"> 
```

The `src` attribute contains the default image size good for desktop. It works as a fallback for very old browsers without responsive images support.

The `srcset` definition is mostly up to you and has no strict rules how select right resolutions. I mostly pick the doubled size for 375px viewport as a smallest image first. In our case is image size 345 * 2 = 690px (375px viewport - 30px spacing * 2 for retina).

Then I'll define the largest desktop size `1110px` for the standard desktop and `2220px` for retina screens as the second step.

Do we need some other sizes? As always it depends. I always try to cover possible resolutions with some reasonable steps. We could try to add something between 690px and 1110px you think, right? But are there any devices which could benefit from this sizes? Probably not. All tablets are using retinas, 1110px or 2220px will be more suitable for them.

We could also add some image source in the middle between 1110px and 2220px if it will be benefit for our audience.

For the mobile user is the image file size way better: **59 KB (- 459 KB)** (compared to original 2400px wide image).

[... celebrate anim gif ...]

## 3. WebP format

This improvement looks really great. Could we improve it? Yes, again!

WebP is the image format developed by Google more than 10 years ago with first support in browsers from 2018. Now it's widely supported and widely used.

We have to change our markup if we would like to use WebP format:

```html
<picture>
  <source
    type="image/webp"
    srcset="
    img-dist/zlin-690.webp 690w,
    img-dist/zlin-1110.webp 1110w,
    img-dist/zlin-2220.webp 2220w
    "
    sizes="
    (max-width: 575px) calc(100vw - 30px),
    (max-width: 767px) 510px,
    (max-width: 991px) 690px,
    (max-width: 1199px) 930px,
    1110px">
  <img
    src="img-dist/zlin-1110.jpg"
    srcset="
    img-dist/zlin-690.jpg 690w,
    img-dist/zlin-1110.jpg 1110w,
    img-dist/zlin-2220.jpg 2220w
    "
    sizes="
    (max-width: 575px) calc(100vw - 30px),
    (max-width: 767px) 510px,
    (max-width: 991px) 690px,
    (max-width: 1199px) 930px,
    1110px"
    alt="Zlín" class="mb-2">
</picture>
```

Conversion from jpeg or png to webp could be done automatically in your dev stack or by some online tool. I tried [gulp-webp](https://www.npmjs.com/package/gulp-webp) and the result was: **54 KB (- 5 KB)** (compared to resized 690px wide jped image).

## 4. AVIF format

WebP size improvement is not bad, but not a big dealbraker. The new kid on the image format block is AVIF (AV1 image format), supported currently in Chrome and Firefox. The average save there is a 50% by using an AVIF image compared to a JPEG and 20% compared to WebP images.

For conversion I tried the (squoosh.app)[https://squoosh.app/] and the converted image has **38 KB (- 16 KB)** (compared to webp image). Our code now looks like:

```html
<picture>
  <source
    type="image/avif"
    srcset="
    img-dist/zlin-690.avif 690w,
    img-dist/zlin-1110.avif 1110w,
    img-dist/zlin-2220.avif 2220w
    "
    sizes="
    (max-width: 575px) calc(100vw - 30px),
    (max-width: 767px) 510px,
    (max-width: 991px) 690px,
    (max-width: 1199px) 930px,
    1110px">
  <source
    type="image/webp"
    srcset="
    img-dist/zlin-690.webp 690w,
    img-dist/zlin-1110.webp 1110w,
    img-dist/zlin-2220.webp 2220w
    "
    sizes="
    (max-width: 575px) calc(100vw - 30px),
    (max-width: 767px) 510px,
    (max-width: 991px) 690px,
    (max-width: 1199px) 930px,
    1110px">
  <img
    src="img-dist/zlin-1110.jpg"
    srcset="
    img-dist/zlin-690.jpg 690w,
    img-dist/zlin-1110.jpg 1110w,
    img-dist/zlin-2220.jpg 2220w
    "
    sizes="
    (max-width: 575px) calc(100vw - 30px),
    (max-width: 767px) 510px,
    (max-width: 991px) 690px,
    (max-width: 1199px) 930px,
    1110px"
    alt="Zlín" class="mb-2">
</picture>
```

## 5. Different aspect ratio for mobile users
