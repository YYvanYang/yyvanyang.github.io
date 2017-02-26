---
layout: post
title:  "CSS Mastery - Content Layout"
categories: jekyll update
---

## Using Positioning

* Elements are initially positioned as static, meaning that block-level elements stack up vertically.
* We can give elements relative positioning , allowing us to nudge them around relative to their original position without altering the flow of elements around them. Doing so also creates a new positioning context for descendant elements. That last fact is what makes relative positioning really useful. Historically, the ability to nudge elements around was an important ingredient in many old-school layout hacks, but these days we can often get by without them.
* Absolute positioning allows us to give an element an exact position with regard to the nearest positioning context, which is either an ancestor with a positioning other than static, or the html element. In this model, elements are lifted out of the page flow, and put back relative to their positioning context. By default, they end up where they originally should have ended up were they static, but without affecting the surrounding elements. We can then choose to change their position, relative to the positioning context.
* Fixed positioning is basically the same as absolute, but the positioning context is automatically set to the browser viewport.”

>摘录来自: Andy Budd and Emil Björklund. “CSS Mastery”。 iBooks. 

## Creating Triangles in CSS

```css
.comment:after {
  position: absolute;
  content: '';
  display: block;
  width: 0;
  height: 0;
  border: .5em solid #dcf0ff;
  border-bottom-color: transparent;
  border-right-color: transparent;
  position: absolute;
  right: -1em;
  top: .5em;
}
```

![]({{site.baseurl}}/images/A314884_3_En_6_Fig3_HTML.gif)

## Automatic Sizing Using Offsets
>Without any declared size, the absolutely positioned element will fall back to the size needed to contain the contents within it. When we declare offsets from opposing sides of the positioning context, the element will stretch to accommodate the size it needs to fulfill these rules.

```html
<header class="photo-header">
  <img src="images/big_spaceship.jpg" alt="An artist's mockup of the "Dragon" spaceship">
  <div class="photo-header-plate">
    <h1>SpaceX unveil the Crew Dragon</h1>
    <p>Photo from SpaceX on <a href="https://www.flickr.com/photos/spacexphotos/16787988882/">Flickr</a></p>
  </div>
</header>
```

“Assuming we don’t want the semitransparent “plate” holding the heading to take up a specific width, we can instead position it from the right, bottom, and left sides and let it figure out its measurements as well as the top edge position by itself:”

摘录来自: Andy Budd and Emil Björklund. “CSS Mastery”。 iBooks. 

```css
.photo-header {
  position: relative;
}
.photo-header-plate {
  position: absolute;
  right: 4em;
  bottom: 4em;
  left: 4em;
  background-color: #fff;
  background-color: rgba(255,255,255,0.7);
  padding: 2em;
}
```
![]({{site.baseurl}}/images/automatic-sizing-using-offset.png)

Regardless of the dimensions of the image, the plate will now sit at the bottom of the image at 4 ems from the bottom and sides. This gives us something that works nicely at different screen sizes—the top edge of the plate will adjust to the content height if there are line breaks.

![]({{site.baseurl}}/images/spacex.jpg)

