---
layout: post
title: Function parameter with default value in Python
categories:
- blog
tags:
- Python
- Function
---

## Function parameter with default value in Python

This article is to describe restrictions when defining python function parameter default values. 

There are two types of Python objects, immutable objects and mutable objects.

Mutable objects include: dict(), list(), ...
Immutable objects include: tuple(), string, int, float, ...

Take a look at below code piece:

```
def append_to_list(value, default_list=[])
	default_list.append(value)
	return default_list

>>> my_list = append_to_list(1)
>>> my_list
    [1] 

>>> my_other_list = append_to_list(2)
>>> my_other_list
    [1,2]
```

For my_other_list, we are expecting only 2 in the list, but it looks like the result is "remembering" the result from last run. Why is this happening? 

Note that all python value are passed by its reference (address). When the function is defined, the function parameter is also defined. When using the mutable objects as the parameter, it will allocate a fixed memory address to the mutable object. When the function is called, it is actually using this mutable object's address cache to get the result. 



