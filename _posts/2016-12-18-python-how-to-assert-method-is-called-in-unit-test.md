---
layout: post
title: Python How to assert a method is called in unit test using Mock/MagicMock
categories:
- Programming
tags:
- Python
- Unit Test

---

## Why do we need unit test? 

Sometimes developers are asked to restructure their code due to design, framework change or code cleanup purpose. One scenario is, the developer will implement same logic but move the code from one class to another, etc., and still keep the same business logic. Consequently, how to test if the code refactor / restructure doesn't have any impact is crucial. If the unit test is already in place, then the developer doesn't need to rerun the whole program again and again to test the code's logic after the refactor. 

## Real problem - how to test if a method is called with its argument?

We have one web app views module which contains a method like this:

```python
@action_config(renderer='offering_definition/edit_package.mako')
def copy(self):
    package = self.context.offering_definition
    copy_util = CopyUtility(self.context.db)
    package_copy = copy_util.copy_package(package)
    redirect = self.request.route_path(
        'package manager id',
        action='edit',
        id=package_copy.definition_id
    )
    return HTTPFound(location=redirect)
```

What this function does is, we got an instance called "package", and want to copy that object. Once copied the web page should be redirected to another page. 

Now, we want an unit test that can verify if copy_package() method is actually called in copy() method, with the argument of package. How can we achieve that? 

## Solution - use Mock/MagicMock 

Python Mock/MagicMock enables us to reproduce expensive objects in our tests by using built-in methods (`__call__`, `__import__`) and variables to “memorize” the status of attributes, and function calls. We can use them to mimic the resources by controlling how they were created, what their return value is. There is one method called _assert_called_with()_ which asserts that the patched function was called with the arguments specified as arguments, to assert_called_with(). Let's take a look how this is implemented. 

For above code, we can write a unit test like this:

```python
def test_copy_processing_post(self):
    package = make_offering_definition(self.db)
    self.request.method = 'POST'
    self.request.POST = POST_EDIT_PARAMS
    self.context.definition_id = package.definition_id
    odh = OfferingDefinitionHandler(self.context, self.request)
    <span style="background-color: #FFFF00">
    with patch('boss.views.offering_definition.lib.CopyUtility.copy_package'
               ) as copy_package_call:
        copy_return = odh.copy()
        copy_package_call.assert_called_with(package)
    </span>
```

## How does this work?

 

