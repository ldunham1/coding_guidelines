# Python Coding Guidelines for Technical Artists

![Python-Image](https://www.python.org/static/community_logos/python-logo-master-v3-TM-flattened.png)

## Introduction:
These guidelines have been designed for Python by <ins>non-programmers</ins> favouring logic and 
readability to improve likelihood of adoption.

They have been developed over various projects and will continue to do so. 

They are **Guidelines** not **Rules**. The whole point is to improve 
readability and code stability with logical reasoning, not to "check boxes" ala "pep8-ers".
 
[PEP 8](https://www.python.org/dev/peps/pep-0008/) is the basis of these guidelines.


## Contents:
- [General](#general)
  - [Python 2/3](#python-2-or-3)
  - [Which IDE?](#which-ide)
- [Naming](#naming)
  - [Public object naming](#object-naming)
  - [Private object naming](#private-object-naming)
  - [General naming conventions](#general-naming-conventions)
  - [Naming Exceptions](#naming-exceptions)
- [Importing](#importing)
- [Line Length](#line-length)
- [Line Breaks and Continuation](#line-breaks-and-continuation)
- [Slicing](#index-slicing)
- [Separators](#separators)
- [Exceptions](#exceptions)
- [Try/Finally](#try-finally)
- [Logging](#logging)
- [Docstrings](#docstrings)
  - [Doctests](#doctests)
- [Testing](#testing)


## General:

The flow of a module should follow;
1. Module docstring (if any).
3. [Imports](#importing).
4. Content.
5. \_\_all__ declaration (if used).
6. `if __name__ == '__main__': ` (if used).

Where logical and possible, objects should be declared **before** they are used. 
We want to improve the developer's chances of encountering an object before 
it was used - improving readability.
```python
# --------------------------------------------------------------------------
# Yes
def ensure_strings(items):
    return list(map(str, items))


def join_names(names, delim=', '):
    str_names = ensure_strings(names)
    return delim.join(str_names)


class Foo(object):

    def bar(self):
        name_list = [
            'Lee',
            'Mike',
            'Agathe',
        ]
        print(join_names(name_list))


# --------------------------------------------------------------------------
# No
class Foo(object):

    def bar(self):
        name_list = [
            'Lee',
            'Mike',
            'Agathe',
        ]
        print(join_names(name_list))


def join_names(names, delim=', '):
    str_names = ensure_strings(names)
    return delim.join(str_names)


def ensure_strings(items):
    return list(map(str, items))
```

### Python 2 or 3:

Where possible and <ins>when it makes sense</ins>, write Python 2 and 3 compatible code. 
In VFX and Games, projects usually lock to a major version of an Application during production.
Some of these projects may continue development for several years using a single Application version due to 
the potential disruption of updating.
As most Applications are only now just moving to Python 3, it makes very little sense to completely disregard 
Python 2 until productions have had a chance to move on. 

> :warning: Never assume you know the extent in which your code will be used. People will always surprise you.


### Which IDE?:

Unless you are forced otherwise, choose use whichever IDE bests suits you and helps improve 
your code quality. 
That being said, I would **highly** recommend investing in an IDE with a minimum set of features.
- Remote Debugging.
    Connect to another process and real-time debug your Python code.
- Conditional Breakpoints, Variable Watching, Expression Evaluation.
    Various "essential" debugging features when remote debugging.
- Version Control Integration; Git, Perforce (if necessary).
    Often taken for granted how much time can be saved not manually executing git commands or p4 calls.
- Code Completion (type matching), PEP8 Inspections & highlighting.
    A must have for any Python IDE.
- Local History.
    Provides a significant workflow iteration boost being able to immediately check what has changed in the last few minutes from working to non-working code.

I've developed professionally with;
- [Notepad++](https://notepad-plus-plus.org/)
- [Wing IDE](https://wingware.com/)
- [Eclipse](https://www.eclipse.org/ide/) (with [PyDev](https://www.pydev.org/))
- [PyCharm](https://www.jetbrains.com/pycharm/)
- [Visual Studio](https://visualstudio.microsoft.com/)
- [Visual Studio Code](https://code.visualstudio.com/)

and I've dabbled with;
- [Sublime](https://www.sublimetext.com/)
- [Atom](https://github.com/atom)

I personally work well with PyCharm and often end up showing colleagues various features that compliment their own workflow (or even functionality they didnt even consider to exist - Diff Local History, for example).


## Naming:

These general naming conventions used to help discern intended usage and aid readability.

#### Object naming

```python
# ../package_name/module_name.py

CONSTANT_NAME = ...
variable_name = ...


def function_name(arg):
    ...


class ClassName(object):
    ...

    def method_name(self, ...):
        ...
```

#### Private object naming

Private objects are simply prefixed with a single underscore.
```python
# ../_package_name/_module_name.py

_CONSTANT_NAME = ...
_variable_name = ...


def _function_name(arg):
    ...


class _ClassName(object):
    ...

    def _method_name(self, ...):
        ...
```

#### General naming conventions

Use descriptive names wherever possible, favouring readability over convenience. 
This being said - avoid over-descriptive or unnecessary long names, as this is usually 
unreadable when used.

Single letter variable names are only suitable representing an integer and _ for the 
throwaway variable.
```python
for i in range(10):
    for _ in range(i):
        print(True)
```

Use reverse notation to improve readability and auto-complete.

```python
# --------------------------------------------------------------------------
# Yes
layer_enabled = ...
layer_disabled = ...
layer_mode_additive = ...
layer_mode_override = ...


# --------------------------------------------------------------------------
# No
enabled_layer = ...
disabled_layer = ...
additive_layer_mode = ...
override_layer_mode = ...
```

### Naming Exceptions:

Like [PEP 8's Overriding Principle](https://www.python.org/dev/peps/pep-0008/#overriding-principle), 
the general consensus for public objects should reflect existing usage. 

```python
# --------------------------------------------------------------------------
# Yes
class QCustomWidget(QWidget):

    def conformingMethod(self):
        ...


# --------------------------------------------------------------------------
# No
class CustomWidget(QWidget):

    def non_conforming_method(self):
        ...
```


## Importing:

Imports are grouped, sorted and indented logically to improve readability.

Separate imports by source into 3 distinct groups.
  1. Standard library imports
  2. Third-party imports
  3. Local imports.

Sort each group using [natural sort order](https://en.wikipedia.org/wiki/Natural_sort_order) 
<ins>by line</ins>, meaning `from sys import ...` is placed before `import abc`.

Implicit relative imports are placed first in their respective groups.


```python
from itertools import (
    imap,
    permutations,
)
import abc
import sys

from numpy import arange
import six

from . import some_module
from package_name import other_module


class Foo(object):
    ...
```


## Line length:

80-120 characters is usually fine. Just avoid lines that need horizontal scrolling.

Use parentheses for line continuations.

```python
full_name_list = [
    'Bob.Something',
    'Mark.Else',
    'John.Blogger',
]

list_comp = [
    name  # Return variable
    for name in full_name_list  # For loop
    if name.startswith('Bob')  # Condition
]
dict_comp = {
    firstname: surname 
    for full_name in full_name_list
    for firstname, surname in full_name.split('.')
}

# Use aligned indentation to identify groups.
for name, age in zip(list_comp,
                     [10, 20, 30]):
    ...
```


## Line breaks and continuation:

### Comments
Use empty line breaks before a comment and before a closing return. 
In general use them where they help identify context and to avoid a wall of text.

```python
try:
    def main():

        # "Bob" is a special person, treat them well.
        if name == 'Bob':
            something_nice(name)

        # If we're not "Bob" then just ignore them.
        # They won't take offense, they totally understand.
        else:
            print('Not "Bob"')

        return True

# Account for "name" not being available
except NameError:
    pass
```

Use parenthesis for line continuation.
> :warning: Just say NO to backslashes for line continuation. 

```python
# Long strings
silly = (
    'super'
    'califragilistic'
    'expialidocious'
)

# Long strings with format
monty = (
    'Always look on '
    'the {0} '
    'side of {1}.'
).format('bright', 'life')
```

### Conditions
Use newlines with an additional indent level to make long conditions easier to digest.

```python
if ((True and False) and
        (True and None) and
        (False or None == False)):
    print('huh?')
```

### Functions/Methods
For functions or methods with many arguments, use inline line continuation to improve readability.
```python
def make(name,
         size,
         colour,
         alliance=None,
         awards=0):
    pass

make(
    'Example',
    size=(10, 10),
    colour=(255, 0, 0),
    alliance='neutral',
    awards=100,
)
```


## Index slicing:

If you intend on getting the last item with a slice, then use -1, regardless of the fixed length.
This aids readability by explicitly expressing intent.

```python
# --------------------------------------------------------------------------
# Yes
full_name = 'Bob.Something'
surname = full_name.split('.')[-1]

# --------------------------------------------------------------------------
# No
full_name = 'Bob.Something'
surname = full_name.split('.')[1]
```


## Separators:

Separators are used to group sections of code to improve readability at a glance 
and therefore are usually as long as you'd accept a line to be.

> It's useful to have a [macro](https://www.jetbrains.com/help/pycharm/using-macros-in-the-editor.html) to create separators to ensure consistency. 

```python
from __future__ import print_function


# --------------------------------------------------------------------------
def remove_first_names(name_list):
    family_names = [
        name.rsplit('.', 1)[-1]
        for name in name_list
    ]
    return family_names


def remove_family_names(name_list):
    first_names = [
        name.split('.', 1)[0]
        for name in name_list
    ]
    return first_names


# --------------------------------------------------------------------------
def is_a_cat_name(name):
    return name in CAT_NAMES
```


## Exceptions:
Try/except blocks should always provide an adequate exception type.

**Lazy Exception handling is generally discouraged.** 

> Remember, we want to improve readability, not just "check boxes".

```python
# Yes
try:
    1 / 0
except ZeroDivisionError:
    pass

# No
try:
    1 / 0
except:
    pass

# Better but still No
try:
    1 / 0
except Exception:
    pass
```


## Try Finally:
Use `finally` to ensure that regardless of how the scope is exited (return, exception etc) that code will be executed (like at `__exit__`)

```python
import fbx


def doit(fbx_filepath):
    scene = fbx.open(fbx_filepath)
    try:
        scene.super_sketchy_op()
        return 'Success'
    except RuntimeError:
        return 'Failed'
    finally:
        scene.close()
```


## Logging:
There is almost no excuse not to use logging. Get into the habit of it as early as possible and keep going. Clients, other devs and future you will thank you for doing so.

```python
import logging


# Unless known, it wont hurt to ensure there is a root logger already setup.
logging.basicConfig()

# Use the root logger if you dont want to associate your massages 
# to a specific tool/module, although using I've yet to find 
# a valid excuse to use debug on a root logger.
logging.debug('debug')  # Nope.
logging.info('Like a print, but better.')

# Better to used a named log object
LOG = logging.getLogger('ExampleLogger')
LOG.setLevel(logging.INFO)  # Only INFO and above message are printed.

LOG.debug('This is ignored.')
LOG.info('This is good.')
LOG.warning('This is great.')
LOG.error('This is ideal.')
```

Absolutely use logging for exceptions. Do not use `traceback` in order to print exception info.
If skipping exceptions (via `pass`) then consider still logging the exception at debug level for future debugging.

```python
# General usage - output the same information you would get 
# from `traceback.print_exc()`, but to a configurable logger object.
try:
    1 / 0
except ZeroDivisionError:
    LOG.exception('')  # prints exception and traceback

    # or - you want to provide specific info...
    LOG.exception('Something went wrong doing 1/0!')  # prints traceback and custom error message.

    # or - if you're going to ignore it...
    LOG.debug('Exception & traceback in debug?!', exc_info=True)  # prints traceback and custom message (if given) but at debug level.
```


## Docstrings:

Use them and keep them updated. 
Good docstrings save far more time for developers (yourself included) than the time 
spent writing them. It doesn't matter if you use Epytext, reST or Google -  <b>just use them!</b>
> Intellisense will make good use of your docstring typehints!
```python
"""
Example of a module Docstring. 
Notice the triple quotes.
"""


class Foo(object):
    """
    Example Foo class docstring.

    :param int my_arg: Completely useless argument.
    """

    def __init__(self, my_arg):
        pass

    def bar(self):
        """Method with no arguments or return."""
        pass

    def bar_two(self, an_arg):
        """
        Takes given `an_arg` and does nothing with it.
        Will always return 0.
        :param int/None an_arg: Completely useless argument.
        :return: The value 0.
        :rtype: int
        """
        return 0
```


#### Doctests:

Potentially unclear functionality usually benefits from a concise code example or two. 
Doctests also provide a
"better-than-nothing" alternative to automated testing. 
If used with an IDE like PyCharm, DocTests can be run with little to no setup, and make 
usage clearer for other developers.

> :exclamation: Pay attention to the line breaks and indentation for the code 
> block if you want your doctests to be recognised.

```python
"""
Example of a Docstring.
Notice the triple quotes.
I am now going to show a code example which doubles as a DocTest.

    .. code-block:: python

        >>> name = 'mike'
        >>> # Test a return
        >>> name.title()
        'Mike'
        >>> # Test a variable
        >>> upper_name = name.upper()
        >>> upper_name == 'MIKE'
        True

"""

class Foo(object):
    """
    Example Foo class DocTest.

    .. code-block:: python

        >>> foo = Foo()
        >>> isinstance(foo, Foo)
        True

    """
    pass
```


## Testing:

Write tests. 
Start simple, its just important that you write tests - [tutorial link](https://realpython.com/python-testing/)

#### unittest

One of the most common testing frameworks is `unittest` - a big plus it being standardlib.

```python
import unittest


# Some class we want to test
class Foo(object):
    pass


class test_FooClass(unittest.TestCase):

    def test_instance_type_check(self):
        foo = Foo()
        self.assertTrue(isinstance(foo, Foo))
```
Your testing coverage should reflect the importance of the code you're testing. The more crucial 
the code, generally the more coverage you should have to reduce the likelihood of an issue.

Automated testing isn't a magic bullet, but it is a very useful tool. 
