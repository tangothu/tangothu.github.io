---
layout: post
title: Python function - pass by value or by reference?
categories:
- blog
tags:
- Python
- Function
---

## Python function - pass by value or by reference?

Let's look at below examples to change int number and list in function variables. 

### Changing an int number in function variable

```
def changeNumber(num):
	num = 2
	print ('Changing num to %s',num)

num = 1
print ('Before change, num is %s',num)
changeNumber(num)
print ('After change, num is %s',num)
```

Result:

```
('Before change, num is %s', 1)
('Changing num to %s', 2)
('After change, num is %s', 1)
```

### Changing an list in function variable

```
def changeList(lst):
	lst.append(50)
	newlst = [1,2,3,4,5,6,7,8,9,10]
	lst = newlst

lst = [1,2,3,4]
print ('Before change, list is ',lst)
changeList(lst)
print ('After change, list is ',lst)
```

Result:

```
('Before change, list is ', [1, 2, 3, 4])
('After change, list is ', [1, 2, 3, 4, 50])
```

### int value is not changed while list is changed.. What's happening?

In Python, all variables can be seen as the reference to an object in memory (like a tag on an object). When changing the num (type: int), because int is a immutable data type in Python, so in changeNumber(num) method an local variable (also named "num") is created locally with in the function, and it is assigned as int value 2. However, the original parameter num, is still pointing to 1, thus num is not changed after calling function changeNumber(num).

When changing the lst (type: list), the parameter lst is passed into the function. In the function it is calling append method, which is to change the content of the list (list is mutable, which means the content of it can be changed). But when we reassign the lst to a new list(newlst), it is the same case as changeNumber again - lst is assigned to a new list but it is a local variable assignment without impacting the external variable. So in this case, the variable itself is not changed externally outside of function, but the content already changed. 

