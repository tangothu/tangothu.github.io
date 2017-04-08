---
layout: post
title: Python magic methods (1) continued - Use magic methods to simplify code in URL matching in Python Pyramid application
categories:
- blog
tags:
- Python
- advanced features

---

In the last article we reviewed the features of python magic methods, \_\_new\_\_() and \_\_init\_\_(). We will look into a real example today about how to use the magic methods to simplify code in Python, with example of URL matching in Pyramid framework. 

## Python magic methods (1) continued: Use descriptor, \_\_new\_\_ and metaclass to simplify URL Matching

Assume we have code like below. The method get_route_param will get the parameter from current request object in Pyramid by the name. 

```
class TitlesContext(RootContext):
    def get_title(self):
        title = self._db.query(Title).get(self.get_route_param('title_id'))
        if not title:
            raise HTTPNotFound()
        return title

    def get_route_param(self, name):
        """
        Return the route parameter with the given ``name`` or raise a 404 if it
        is False-y.

        :rtype: unicode
        :raise: :exc:`pyramid.httpexceptions.HTTPNotFound`
        """
        value = self._request.matchdict[name].strip()
        if not value:
            raise HTTPNotFound()
        return value
```

In one pyramid application, for each Model-View-Controller, there could be multiple context object to query the database objects, thus the method get_route_param could exist in many places. 

To improve, we can refactor the code by removing the get_route_param method by creating a new class, UrlMatch: 

```
class TitlesContext(RootContext):

    title_id = UrlMatch()
    
    def get_title(self):
        title = self._db.query(Title).get(self.title_id)
        if not title:
            raise HTTPNotFound()
        return title    
```


```

class UrlMatch(object):
    """
    Descriptor that will retrieve variable from the current requests matchdict.

    This should be used on a context class to allow retrieving matchdict variables
    from the context. When the context class uses :class:`UrlMatchMeta` as it's metaclass
    then ``UrlMatch`` will use the class attribute name as the key to extract from
    the matchdict. If the parent class of ``UrlMatch`` does not use `UrlMatchMeta`
    as its metaclass then the ``name`` argument must be given to ``UrlMatch``.

    Example with metaclass::

        class MyContext(object):

            __metaclass__ = UrlMatchMeta

            def __init__(self, request):
                self._request = request

            my_var = UrlMatch()
	 """

    def __init__(self, name=None):
        self.name = name

    def __get__(self, obj, type=None):
        if not obj:
            # Accessed via class, so return the descriptor
            return self

        if not self.name:
            raise ValueError(
                'UrlMatch property does not have a name to extract from matchdict. ' +
                'Use viewutils.UrlMatchMeta as your metaclass or pass a name to UrlMatch().'
            )

        try:
            return obj._request.matchdict[self.name]
        except KeyError:
            # Turn the KeyError into AttributeError because this will be accessed as
            # an attribute
            raise AttributeError("Matchdict does not have key '{}'".format(self.name))


class UrlMatchMeta(abc.ABCMeta):
    """
    Metaclass that provides support for the :class:`UrlMatch` descriptor.

    This metaclass will set the ``name`` attribute of each ``UrlMatch`` class attribute
    to the name of the class attribute.
    """

    def __new__(cls, classname, bases, classDict):
        new_class = super(UrlMatchMeta, cls).__new__(cls, classname, bases, classDict)
        cls._set_url_match_names(new_class)

        return new_class

    @staticmethod
    def _set_url_match_names(new_class):
        for key, attr in vars(new_class).items():
            if isinstance(attr, UrlMatch):
                attr.name = key
```

In this example, we used descriptor, metaclass \_\_new\_\_() to create this new class to simplify the code. 
So how will this work? 

1. \_\_metaclass\_\_ is used to redefine how a class is constructed. In UrlMatchMeta, \_\_new\_\_() is called to add the key in MyContext's attributes (in this case, "title\_id") to the name of UrlMatch() object. Note \_\_new\_\_() is called automatically when calling the class name, even before \_\_init\_\_(). By using metaclass, we have created a UrlMatch object called title\_id in TitlesContext and dynamically added title\_id as its name. 

2. \_\_get\_\_() descriptor is used to retrieve the attribute from request context. Python descriptor says if we call obj.d, it will look for d in obj's dictionary, and if d defines \_\_get\_\_(), it will call d.\_\_get\_\_(obj). In our case, when we call self.title\_id (self is the TitlesContext), it finds \_\_get\_\_() method, so it will actually call title\_id.\_\_get\_\_(TitlesContext). Since title\_id is UrlMatch, it will return obj.\_request.matchdict[self.name], which is TitlesContext.\_request.matchdict["title\_id"]. As such, we complete the same way as to look for title\_id parameter in request (which is the same as get\_route\_param from the old way).
