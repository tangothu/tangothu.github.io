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
    with patch('boss.views.offering_definition.lib.CopyUtility.copy_package'
               ) as copy_package_call:
        copy_return = odh.copy()
        copy_package_call.assert_called_with(package)
```

## How does this work?

Let's analyze the code line by line:

```
with patch('boss.views.offering_definition.lib.CopyUtility.copy_package'
    ) as copy_package_call:
```

This will create a new instance of mock.patch object and assign it to a variable called copy_package_call. copy_package_call is a MagicMock object with the name as copy_package. This object create all attributes and methods as you access them and store details of how they have been used. You can assume copy_package_call an object that keeps track of how its attributes (if any) or callable is called, how many times the callable is called, what are the arguments, etc. 


```
    copy_return = odh.copy()
```

When it comes to this line, because copy() includes package_copy = copy_util.copy_package(package) method call, and copy_package() is been defined as a MagicMock(), the CallableMixin’s __call__() method will also be invoked. 

```
def __call__(_mock_self, *args, **kwargs):
    # can't use self in-case a function / method we are mocking uses self
    # in the signature
    _mock_self._mock_check_sig(*args, **kwargs)
    return _mock_self._mock_call(*args, **kwargs)

def _mock_call(_mock_self, *args, **kwargs):
    self = _mock_self
    self.called = True
    self.call_count += 1
    _new_name = self._mock_new_name
    _new_parent = self._mock_new_parent
    _call = _Call((args, kwargs), two=True)
    self.call_args = _call
    self.call_args_list.append(_call)
    self.mock_calls.append(_Call(('', args, kwargs)))
```

This line self.call_args = call keeps track of args / kwargs in a tuple that are used in function call. In this case, it will be the instance of OfferingDefinition. This self.callargs is used later to trace the call arguments.

```
    copy_package_call.assert_called_with(package)
```

If we go to the source code of assert_called_with(), we can see what it's doing:

```
def assert_called_with(_mock_self, *args, **kwargs):
    """assert that the mock was called with the specified arguments.
    Raises an AssertionError if the args and keyword args passed in are
    different to the last call to the mock."""
    self = _mock_self
    if self.call_args is None:
        expected = self._format_mock_call_signature(args, kwargs)
        raise AssertionError('Expected call: %s\nNot called' % (expected,))
    def _error_message(cause):
        msg = self._format_mock_failure_message(args, kwargs)
        if six.PY2 and cause is not None:
            # Tack on some diagnostics for Python without __cause__
            msg = '%s\n%s' % (msg, str(cause))
        return msg
    expected = self._call_matcher((args, kwargs))
    actual = self._call_matcher(self.call_args)
    if expected != actual:
        cause = expected if isinstance(expected, Exception) else None
        six.raise_from(AssertionError(_error_message(cause)), cause)
```

In this call – assert_called_with(package), package is passed into function as args.
This method assert_called_with compares if the expected mock object (copy_package()) and the actual object are invoked with by the same argument (OfferingDefinition). Because self.call_args also keeps track of package, the expected and actual object will be equal, and thus the unit test function will pass. 


## More methods about MagicMock (from Python doc)

**assert_called_with()**

This method is a convenient way of asserting that calls are made in a particular way:

**assert_called_once_with()**

Assert that the mock was called exactly once and with the specified arguments.

**assert_any_call()**

assert the mock has been called with the specified arguments. The assert passes if the mock has ever been called, unlike assert_called_with() and assert_called_once_with() that only pass if the call is the most recent one.

**assert_has_calls()**

assert the mock has been called with the specified calls. The mock_calls list is checked for the calls. If any_order is false (the default) then the calls must be sequential. There can be extra calls before or after the specified calls. If any_order is true then the calls can be in any order, but they must all appear in mock_calls.


