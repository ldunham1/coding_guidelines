# Python Coding Guidelines

## Introduction:
These Guidelines have been designed for Python, by non-programmers.
They have been developed over various projects and will continue to do so. 

They are not perfect. They do however heavily favour logic and readability to improve likelihood of adoption.

They are **Guidelines** not **Rules**. The whole point is to improve readability and code stability with logical reasoning, not to "check boxes" ala "pep8-ers".
 
[PEP 8](https://www.python.org/dev/peps/pep-0008/) is used as the base of these guidelines.

## Contents:
- [General](#general)
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
- [Docstrings](#docstrings)
  - [Doctests](#doctests)



## General:

The flow of a module should follow;
1. Module docstring (if any).
2. [Imports](#importing).
3. Content.
4. \__all__ declaration (if used).
5. `if __name__ == '__main__': ` (if used).

Where logical and possible, objects should be declared **before** they are used. We want to improve the developer's chances of encountering an object before it was used - improving readability.
```python
# ---------------------------------------------------
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


# ---------------------------------------------------
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

Use descriptive names wherever possible, favouring readability over convenience. This being said - avoid over-descriptive or unnecessary long names, as this is usually unreadable when used.

Single letter variable names are only suitable representing an integer and _ for the throwaway variable.
```python
for i in range(10):
    for _ in range(i):
        print(True)
```

Use reverse notation to improve readability and auto-complete.

```python
# ---------------------------------------------------
# Yes
layer_enabled = ...
layer_disabled = ...
layer_mode_additive = ...
layer_mode_override = ...

# ---------------------------------------------------
# No
enabled_layer = ...
disabled_layer = ...
additive_layer_mode = ...
override_layer_mode = ...
```

### Naming Exceptions:

Like [PEP 8's Overriding Principle](https://www.python.org/dev/peps/pep-0008/#overriding-principle), the general concensus for public objects should reflect existing usage. 

```python
# ---------------------------------------------------
# Yes
class QCustomWidget(QWidget):

    def conformingMethod(self):
        ...

# ---------------------------------------------------
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

Sort each group using [natural sort order](https://en.wikipedia.org/wiki/Natural_sort_order) <ins>by line</ins> by line (meaning from is placed before import).

Implicit relative imports are placed first in their respective groups.


```python
from itertools import (
    imap,
    izip,
    permutations,
)
import abc
import sys

from numpy import arange
import six

from . import some_module
from package_name import other_module
```


## Line length:

80-120 characters is usually fine. Just avoid lines that need vertical scrolling.

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
    for firstname, surname  in full_name.split('.')
}

# Use aligned indentation to identify groups.
for name, age in zip(list_comp,
                     [10, 20, 30]):
    ...

```


## Line breaks and continuation:

Use empty line breaks at the end of a scope and before a comment.

```python
try:

    # "Bob" is a special person, treat them well.
    if name == 'Bob':
    
        # Do something here.
        something_nice(name)

    else:
        print('Not "Bob"')

# Account for "name" not being available
except NameError:
    pass

```

Use parenthesis for line continuation and stay away from backslashes for line continuation. Its helps nobody.

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


## Index slicing:

If you intend on getting the last item with a slice, then use -1, regardless of the fixed length.
This aids readability by explicitly expressing intent.

```python
# ---------------------------------------------------
# Yes
full_name = 'Bob.Something'
surname = full_name.split('.')[-1]

# ---------------------------------------------------
# No
full_name = 'Bob.Something'
surname = full_name.split('.')[1]
```


## Separators:

Separators are used to group sections of code to improve readability at a glance and therefore are usually as long as you'd accept a line to be.

```python
import pprint


# ---------------------------------------------------
def remove_first_names(name_list):
    family_names = [
        name.split('.')[-1]
        for name in name_list
    ]
    return family_names


def remove_family_names(name_list):
    first_names = [
        name.split('.')[0]
        for name in name_list
    ]
    return first_names


# ---------------------------------------------------
def is_a_cat_name(name):
    return name in CAT_NAMES


```


## Exceptions:
Try/except blocks should always provide an adequate exception type.

**Lazy Exception handling is discouraged if not necessary.** 

Remember, we want to improve readability not "check boxes".

```python
# ---------------------------------------------------
# Yes
try:
    1 / 0

except ZeroDivisionError:
    pass

# ---------------------------------------------------
# No
try:
    1 / 0

except:
    pass

# ---------------------------------------------------
# Still No
try:
    1 / 0

except Exception:
    pass
```


## Docstrings:

Use them and keep them updated. 
Good docstrings save far more time for developers (yourself included) than time save by not writing them.

```python
"""
Example of a Docstring. 
Notice the triple quotes.
"""

```


#### Doctests:

Potentially unclear functionality usually benefits from a concise code example or two. Doctests also provide a
"better-than-nothing" alternative to automated testing. 
If used with an IDE like PyCharm, DocTests can be run with little to no setup, and make usage clearer for other developers.

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

```

