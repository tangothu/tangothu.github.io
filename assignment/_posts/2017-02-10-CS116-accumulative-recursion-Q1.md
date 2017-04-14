---
layout: post
title: Python CS116 Winter - Use accumulative recursion to complete the function nat_range
categories:
- assignment
tags:
- Python
- Data Structure
- accumulative recursion
---


### Question 1

- Use accumulative recursion to complete the function nat_range, that consumes the natural
numbers start, end (with start <= end), and a positive integer step, and then produces a list:
· that begins with start
· advances by step for each subsequent element
· but, ends right before reaching end
For example,
· nat_range(0, 5, 1) => [0, 1, 2, 3, 4]
· nat_range(0, 0, 1) => []
· nat_range(3, 7, 2) => [3, 5]

Note: This is a basic version of the built-in Python function range(start, end, step), which
you are not allowed to use here, and also shares the semantics of string / list slicing!
