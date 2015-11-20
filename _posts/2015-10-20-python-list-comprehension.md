---
layout: post
title: List Comprehension in Python
categories:
- Programming
tags:
- Python
- Data Structure
---

## List Comprehension in Python - Summary

This article is to write tips on what is list comprehension in Python, how to write it, and how to use it.

### What is Python list comprehension? 


List comprehension provides a convenient way to define lists in Python. Traditionally how we create lists in Python is: 

```
nums = [0,1,2,3,4]
```

With list comprehension, we can create lists in this new way, as:

```
nums = [x for x in range(5)]
```

### How to write list comprehension


In list comprehension, there are four components. Take below code piece as an example:

```
nums = [x for x in range(5) if x > 2]
```

There are four parts in this list comprehension, including:

input parameters: range(5)
variable in input parameters: x
condition: if x > 2
list comprehension expression: []

Once you have above four components, you can write list comprehension. 

### Creating generator by using list comprehension grammar


After a list comprehension is created, it will be taken by memory in Python runtime. However, if the list itself is a very large data set, it will occupy a lot of memory as well. Instead of creating list comprehension, we can create generator as an alternative. 

```
nums_generator = (x for x in range(5) if x > 2)
```

Note to create generator using list comprehension grammar, the only difference is we use parentheses () instead of square brackets [].

The benefits of using generator is, only the generator itself will be loaded into memory, which will let the code run faster if memory is limited. In order to retrieve the elements in generator, we only need to use for loop to access its element:

```
for item in nums_generator:
	print item
```








