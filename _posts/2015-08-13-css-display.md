---
layout: post
title: CSS display usage
categories:
- Programming
tags:
- CSS
- Client Side
---

I've been recently writing client side code which uses css style. One important layout property in CSS is called display. Here I will write down my understanding of display property in CSS.

There are multiple values display property can be assigned: inline, block, inline-block (introduced in CSS 2.1). In order to differenciate them, we need to take a look at HTML elements types first. 

In HTML, elements can be classifies as two types: block elements and inline elements. 

block element: Every element will start from a new line. The element's width, height, and margin size can be set. By default, the element's width and height will be the same as its parents' width and height.
Examples: <script src="https://gist.github.com/tangothu/701b69b0d3f603b431a5.js"></script>

inline element: Element that does not start a new line. Will ignore top and bottom margin settings as well as width and height. The element width and height is the word's (or the image's) width and height.
Examples: <script src="https://gist.github.com/tangothu/43dd6bce46674064a5f3.js"></script>

For each element in HTML, it will correspond to either block or inline element. We can overwrite the way how an element is displayed by using the display property. For example:

<script src="https://gist.github.com/tangothu/51d08c16a68bf32af42d.js"></script> -- This will convert the *div* element to a *span* element
<script src="https://gist.github.com/tangothu/b7206058192d0ea3ab88.js"></script> -- This will convert the *span* element to a *div* element

What is display: inline-block then?

inline-block is the element who can be defined with width and height, and also can be aligned in the same line with other element. 

jsfiddle example to test the attributes:
[Link](http://jsfiddle.net/ayzLztau/)



