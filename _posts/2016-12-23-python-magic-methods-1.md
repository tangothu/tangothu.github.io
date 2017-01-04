---
layout: post
title: Python magic methods (1)
categories:
- Programming
tags:
- Python
- advanced features

---

## Python magic methods (1) : \_\_new\_\_() and \_\_init\_\_() 

In Python, magic methods are special methods where you can define to add "magic" to classes. They're always surrounded by double underscores (called 'dunder'). We will look into these magic methods in Object construction and initialization and see how they add advantages to python object oriented programming. 


 
### Object construction and initialization

- \_\_new\_\_(cls, [...): the first method to get called in an object's instantiation
- \_\_init\_\_(self, [...): the method get called to create objects of class

From the definition above, we see that \_\_new\_\_() accepts cls as it's first parameter, while \_\_init\_\_() accepts self. \_\_init\_\_() is called after \_\_new\_\_() when object is initialized. 

Let's take a look at example below. 

```
class MyClass(object):

    def __new__(cls):
        print "MyClass.__new__ called"
        return super(MyClass, cls).__new__(cls) # here super is type

    def __init__(self):
        print "MyClass.__init__ called"



if __name__ == '__main__':
    c = MyClass()
```

Result:

```
MyClass.__new__ called
MyClass.__init__ called
```

As we can see, \_\_new\_\_() method took cls as the input to create _self_ object, then when a new MyClass object is instantiated, \_\_init\_\_() is called thereafter. 

Another thing to note: \_\_new\_\_() is called automatically when calling the class name, which is when we do MyClass(). On the other hand, \_\_init\_\_() is called every time an instance of the class is returned by \_\_new\_\_() passing the returned instance to \_\_init\_\_() as 'self'. 

As such, \_\_new\_\_() will return the instance of _cls_, and then, this new instance will be picked up by \_\_init\_\_(). If we ommit calling super for \_\_new\_\_() and not return anything, then \_\_init\_\_() won't be executed. 

### When to use \_\_new\_\_()?

If you want to control the actual creation process, use the \_\_new\_\_() method. If you intend to alter something like the base classes or the attributes, you’ll have to do it in \_\_new\_\_(). 

Below example is to help to create a customized list with a method , "add". By default, python list doesn't have add method, but in this example we can create this customized list by adding add to the attr of the metaclass in \_\_new\_\_() method. 

```
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)

class MyList(list):
    __metaclass__ = ListMetaclass # use ListMetaclass to customize class

if __name__ == '__main__':
    #c = MyClass()
    l = MyList()
    l.add(1)
    print (l)
```

Result:
```
[1]
```

The result shows this MyList() instance has a method add which adds elements to the list. 
