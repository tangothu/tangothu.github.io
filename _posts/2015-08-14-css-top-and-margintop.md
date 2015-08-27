---
layout: post
title: CSS position, top and margin-top
categories:
- Programming
tags:
- CSS
- Client Side
---

###1.How to use position property with top,left,right,bottom

In CSS, top, left, right and bottom should always be used with position property. We define a css element called "sub" as below:

```
.sub {
  position: relative;
  top: 10px;
}
```

There are two common values of property ().
For 
```
position: relative;
```
, the element defined with this attribute will "relatively" shift its position based on where it originally locates. 

For 
```
position: absolute;
```
, the element defined with this attribute will shift its position either:
a. the parent element of sub is also defined with position, then sub will shift based on the parent's location. 
b. sub doesn't have a parent who has position attribute; then sub will use HTML body and shift based on the body's location. 

###2.What is margin-top? What's the difference between top and margin-top?

margin-top is to add distance on the margin of element, it will shift the block element along with its subsequent elements which are also in document flow (will push them further down). top, on the other hand, is used to shift the element without having effects on the surrounding elements. 

Here is the link to display position absolute vs. position relative.
[Link](http://jsfiddle.net/v1Lfc5hg/)
