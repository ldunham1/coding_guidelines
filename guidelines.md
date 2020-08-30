# Python Coding Guidelines

## Introduction:
These guidelines are formed and iterated upon during development of various projects. They attempt to favour logic and readability, and are updated as they mature.

They are **Guidelines** not **Rules**.
 
[PEP 8](https://www.python.org/dev/peps/pep-0008/) is used as the basis of these guidelines.

## Contents:
- [Naming](#naming)
 - [Public object naming](#object-naming)
 - [Private object naming](#private-object-naming)
 - [General naming conventions](#general-naming-conventions)
 - [Private object naming](#private-object-naming)
- [Naming Exceptions](#naming-exceptions)
- [Importing](#importing)
 - [Ordering](#import-orders)
 - [Grouping](#import-grouping)
 - [Sections](#import-Sectioning)
- [Line Length](#line-length)
 - [Comprehensions](#comprehensions)
- [Slicing](#index-slicing)


## Naming:
These general naming conventions used to help discern intended usage and aid readability.

#### Object naming
```
# ../package_name/module_name.py


CONSTANT_NAME = ...

variable_name = ...


def function_name(arg):
    ...


class ClassName():
    ...

    def method_name(self, ...):
        ...

```

#### Private object naming
Private objects are simply prefixed with a single underscore.
```
# ../_package_name/_module_name.py


_CONSTANT_NAME = ...

_variable_name = ...


def _function_name(arg):
    ...


class _ClassName():
    ...

    def _method_name(self, ...):
        ...

```

#### General naming conventions

Use descriptive names wherever possible, favouring readability over convenience.

Single letter variable names are only suitable representing an integer.
```
for i in range(10):
    print(i)
```

Use a single underscore for a throwaway variable.
```
for _ in range(10):
    print(True)
```

Reverse notation aids readability and auto-complete.

##### + Yes
```
layer_enabled = ...
layer_disabled = ...
layer_mode_additive = ...
layer_mode_override = ...
```

##### - No
```
enabled_layer = ...
disabled_layer = ...
additive_layer_mode = ...
override_layer_mode = ...
```

### Naming - Exceptions:
Like [PEP 8's Overriding Principle](https://www.python.org/dev/peps/pep-0008/#overriding-principle), the general concensus is for public parts of the API to reflect existing usage. 

##### + Yes
```
class QCustomWidget(QWidget):

    def conformingMethod(self):
        non_public_variable = True

```

##### - No
```
class CustomWidget(QWidget):

    def non_conforming_method(self):
        ...

```


## Importing:

#### Import orders

Use a [natural sort order](https://en.wikipedia.org/wiki/Natural_sort_order) by line instead of package/module name.
```
from functools import partial
from itertools import izip
import json
import os
import sys
```

#### Import grouping
Group imports using braces, separated by line (with natural sorting). 
```
from itertools import (
    imap,
    izip,
    permutations,
)
```

#### Import sectioning
Separate imports by source into natural sorted sections.

1. Standard library imports
2. Third-party imports
3. Local imports.

```
from functools import partial
import os

from numpy import arange
import six

from . import some_module
from package_name import other_module
import other_package
```


## Line length:
In theory, a line can be as long it wants to be provided it is readable without horizontal scrolling.
This usually means lines of around 100-120 characters are at the upper end of acceptability.
One-liners are usually discouraged unless they are readable at a glance.


#### Comprehensions
Comprehensions are encouraged to follow this pattern.
```
my_list_comp = [
    name
    for name in name_list
    if name.startswith('Bob')
]
my_dict_comp = {
    full_name.split('.')[0]: full_name.split('.')[-1] 
    for full_name in full_name_list
    if full_name.startswith('Bob')
}
```


## Index slicing:
If you intend on getting the last item with a slice, then use -1, regardless of the fixed length.
This aids readability by explicitly expressing intent.


##### + Yes
```
full_name = 'Bob.Something'
surname = full_name.split('.')[-1]
```

##### - No
```
full_name = 'Bob.Something'
surname = full_name.split('.')[1]
```
