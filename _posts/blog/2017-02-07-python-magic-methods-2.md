---
layout: post
title: Python magic methods (2)
categories:
- blog
tags:
- Python
- advanced features

---

## Python magic methods (2) : \_\_call\_\_()

In Python, we can call a method using object.method(). Is it possible if we use object() to call a method or do something? \_\_call\_\_() method can help us to achieve this - by defining \_\_call\_\_(), the class's instance can be called as a function.

Example:

```
class Person(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('Name is %s.' % self.name)

if __name__ == '__main__':
    p = Person('mike')
    print p()
```

Result:

```
Name is mike.
```

### When to use \_\_call\_\_()?

If we need the objects to be callable, or the object wraps, abstracts the concept of a function, then use \_\_call\_\_(). 

Example:

```
class  Factorial:    
    def  __init__( self ):    
        self .cache = {}    
          
    def  __call__( self , n):    
        if  n  not  in  self .cache:    
            if  n ==  0:    
                self .cache[n] =  1    
            else:    
                self .cache[n] = n *  self .__call__(n-1)    
        return  self .cache[n]    
    
fact = Factorial()    
    
for  i  in  xrange(10):                                                                 
    print ( "{}! = {}" .format(i, fact(i)))  
```

Result:

```
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5040
8! = 40320
9! = 362880
```


