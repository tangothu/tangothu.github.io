---
layout: post
title: Python data structure usage summary
categories:
- blog
tags:
- Python
- Data Structure
---

## Python data structure usage - list, tuple, dict and set

This article is to write notes to record how to define, insert/remove, access and iterate python data structures.

### list()

- Define - use square brackets [] to define.

```
classmates = ['Macro','Bob','Julia']
```
- Insert

```
position = 1
classmates.insert(position,'Aaron') # or use classmates.append('Stephen') which will append data to the last position of list 
```
- Remove

```
classmates.remove('Aaron') # or use classmates.pop(), which will return the last element 
```
- Access

We can use square bracket with index to access element
```
classmates[0] 
```

- Iterate

We can use enumerate, to loop list. Enumerate method will get the iterator of list, yield its index and related element, and loop the index until the very end of list. It will create an *indexed series* of data.

```
for i, item in enumerate(classmates):
	print i, item
```

Alternatively we can also use

```
for i in range(0, len(classmates)):
	print i, classmates[i]
```

Note, in Python, list() is not *hashable*. What hashable means is an object will have a hashvalue (provided by its own __hash__() method and can be compared to other objects) which never changes during its lifetime. For list(), since it is not immutable, the value of itself might be changed, thus if it has hashvalue it will get changed too during its lifetime. That's why python list() doesn't have __hash__() method and is not hashable.

### tuple()

- Define - use parenthesis () to define

```
month = ('Tom','Feb','Dec')
```

- Insert

Once a tuple is created, it cannot be inserted with an element since it is immutable.

- Remove

Once a tuple is created, it cannot remove an element since it is immutable.

- Access

We can use square bracket with index to access element

```
month[0] 
```

- Iterate
We can use enumerate, to loop tuple.

```
for i, item in enumerate(month):
	print i, item
```

### set()

- Define - use set() with a list in the parenthesis to define it 

```
s = set([3,4,5])
```

- Insert

```
s.add('5') # s will now become set(['5',3,4,5])
```

- Remove

```
s.remove(5) # s will not become set(['5',3,4])
```

- Access

Note we cannot use s[0] to access an element in set because set is unordered collection without index.

- Iterate

```
for i in s
	print i
```

### dict()

Dict, in Python, is a *mapping* type which maps a key to a value. The mapping mechanism is implemented by hash function to the key. 
Note in dict, the key has to be hashable (thus immutable) - this is because each time when requesting a value using a key, hash(key) should produce the same result, or otherwise you will get two values based on the same key - that means the definition of dict is wrong! As a result, the key should be hashable, or in other words, the hash(key) should always be the same. 
If the key can be changed, then the hash functino will also return a different value, which shouldn't happen. Thus, the key has to be immutable (list cannot be the key in dict.)

- Define - use curly brackets {} to define.

```
students = {'kevin':60,'michael':70,'jason':65}
```

- Insert

```
students['mike'] = 80
```

- Remove

Use key word *del*

```
del students['mike']
```

or 

```
students.pop('mike') # this will return 80 and remove 'mike':80 in dict
```

- Access

```
students['mike']
```

or

```
score=students.get('mike')
```

- Iterate

Use items() method

```
for key, value in students.items()
	print key, value
```

Reference:
[Python Dictionary Keys](https://wiki.python.org/moin/DictionaryKeys)




