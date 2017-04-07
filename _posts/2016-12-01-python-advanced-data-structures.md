---
layout: post
title: Python advanced data structure - collections
categories:
- Programming
tags:
- Python
- Data Structure

---

## Python advanced data structure - collections

In Python, dict, list, set and tuple are commonly used data structures. Other than these, Python also has several useful built-in data structures in its collections module, e.g., OrderedDict, Counter, deque. Let's take a look at their behavior and sample usage.
 
### OrderedDict()

- How to define a OrderedDict : 
OrderedDict( [(key1, value1), (key2, value2)] )

- Difference with dict: dict is un-ordered data structure. When you want to iterate key value pairs based on their sequence in the dict 

- Example

```
from collections import OrderedDict

items = (
    ('d', 4),
    ('a', 1),
    ('b', 2),
    ('c', 3),
)

regular_dict = dict(items)
ordered_dict = OrderedDict(items)

print 'regular_dict:'
for k, v in regular_dict.items():
    print k, v

print 'ordered_dict:'
for k, v in ordered_dict.items():
    print k, v
```

Result:

```
regular_dict:
a 1
c 3
b 2
d 4
ordered_dict:
d 4
a 1
b 2
c 3
```

Note the OrderedDict will iterate the elements with the sequence when the OrderedDict was defined. On the other hand, for dict, since there is no sequence of the key-value paire, the iteration is also not ordered. 

### Counter()

Assume you have a list and you want to get the statistics of the elements in the list, for example, total number of item counts, the item that most frequently shows in the list, etc. Then you may use Counter() in collections to get these stats. 

- Example: get the item counts

```
from collections import Counter
 
li = ["Roundness", "Square", "Triangle", \
"Roundness", "Roundness", "Square"]
a = Counter(li)
 
print a
print "{0} : {1}".format(a.values(),a.keys())
 
print(a.most_common(2)) 
```

Result:

```
Counter({'Roundness': 3, 'Square': 2, 'Triangle': 1})
[3, 1, 2] : ['Roundness', 'Triangle', 'Square']
[('Roundness', 3), ('Square', 2)]
```

### deque()

deque stands for "double-ended queue". It implements the fast append and pop element from the head. 

In list, we can also do 

```
l.insert(0, v)
l.pop(0)
```

But the time complexity of methods call above will be O(n), which is linear increasing to the number of elements in list. For deque, the time complexity of inserting and poping is O(1). 

- Example

```
from collections import deque

q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
q.popleft()
q.popleft()
print (q)
```

Result:


```
deque(['b', 'c', 'x'])
```


