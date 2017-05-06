---
layout: post
title: Python coroutine - make a generator to coroutine and send data
categories:
- blog
tags:
- Python
- coroutine
- generator

---

This artical will talk about generator in Python coroutines. 
All the demo code below is from http://www.dabeaz.com/coroutines/ . 

In Python, generator is used to produce a sequence of results. Everytime 
you call the generator, it will iterate the next value of the results, 
instead of returning the complete list of results and return them together. 
We use key word _yield_, to make a regular function as generator. 

Let's look at the sample code below: 

```
# grep.py
#
# A very simple coroutine

def grep(pattern):
    print "Looking for %s" % pattern
    while True:
        line = (yield)
        if pattern in line:
            print line,

# Example use
if __name__ == '__main__':
    g = grep("python")
    g.next()
    g.send("Yeah, but no, but yeah, but no")
    g.send("A series of tubes")
    g.send("python generators rock!")
    g.close()
```

In the \_\_main\_\_ function, we first create a generator using 

```
g = grep("python")
```

So now g is a generator. We also see that there line of code's grammar a bit weird:

```
line = (yield)
```

When yield is placed on the right hand side of equation, or say, when we 'assign' 
the yield to a variable, it means this generator becomes a coroutine. Note coroutine is 
a special case of generator but more powerful. Will provide how to make use of coroutine
in next articles. 

To make the coroutine work, it must be "primed" by first calling .next() 
(or send(None)), where this is the activation step. See code below:

```
g.next()
```

Then, we can send values to the function by calling the method send() 

```
g.send("Yeah, but no, but yeah, but no")
g.send("A series of tubes")
g.send("python generators rock!")
```

From above, everytime when we make a call g.send(), we send the string data
and the string value will be assigned to line. In this way, we are able 
to send data to generator. Traditionally, we will just iterate a generator 
and get the value. By using send(), we can now send the data to generator now. 

Note that, there is a infinite while loop in the generator/coroutine definition:

```
while True:
    ...
    ...
```

To stop the coroutine from running, we need to explicitly call close() method:

```
g.close()
```

