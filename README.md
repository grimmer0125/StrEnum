# StrEnum

[![Build Status](https://github.com/irgeek/StrEnum/workflows/Python%20package/badge.svg)](https://github.com/irgeek/StrEnum/actions)


StrEnum is a Python `enum.Enum` that inherits from `str` to complement
`enum.IntEnum` in the standard library.  Supports python 3.6+.

## Installation

You can use [pip](https://pip.pypa.io/en/stable/) to install.

```bash
pip install StrEnum
```

## Usage

```python
from enum import auto
from strenum import StrEnum


class HttpMethod(StrEnum):
    GET = auto()
    HEAD = auto()
    POST = auto()
    PUT = auto()
    DELETE = auto()
    CONNECT = auto()
    OPTIONS = auto()
    TRACE = auto()
    PATCH = auto()


assert HttpMethod.GET == "GET"

## You can use StrEnum values just like strings:

import urllib.request

req = urllib.request.Request('https://www.python.org/', method=HttpMethod.HEAD)
with urllib.request.urlopen(req) as response:
   html = response.read()

assert len(html) == 0 # HEAD requests do not (usually) include a body
```

There are classes whose `auto()` value folds each member name to upper or lower
case:

```python
from enum import auto
from strenum import LowercaseStrEnum, UppercaseStrEnum

class Tag(LowercaseStrEnum):
    Head = auto()
    Body = auto()
    Div = auto()

assert Tag.Head == "head"
assert Tag.Body == "body"
assert Tag.Div == "div"

class HttpMethod(UppercaseStrEnum):
    Get = auto()
    Head = auto()
    Post = auto()

assert HttpMethod.Get == "GET"
assert HttpMethod.Head == "HEAD"
assert HttpMethod.Post == "POST"
```

As well as classes whose `auto()` value converts each member name to camelCase,
PascalCase, kebab-case and snake_case:

```python
from enum import auto
from strenum import CamelCaseStrEnum, PascalCaseStrEnum
from strenum import KebabCaseStrEnum, SnakeCaseStrEnum

class CamelTestEnum(CamelCaseStrEnum):
    OneTwoThree = auto()


class PascalTestEnum(PascalCaseStrEnum):
    OneTwoThree = auto()


class KebabTestEnum(KebabCaseStrEnum):
    OneTwoThree = auto()


class SnakeTestEnum(SnakeCaseStrEnum):
    OneTwoThree = auto()

assert CamelTestEnum.OneTwoThree == "oneTwoThree"
assert PascalTestEnum.OneTwoThree == "OneTwoThree"
assert KebabTestEnum.OneTwoThree == "one-two-three"
assert SnakeTestEnum.OneTwoThree == "one_two_three"
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to
discuss what you would like to change.

Please ensure tests pass before submitting a PR. This repository uses
[Black](https://black.readthedocs.io/en/stable/) and
[Pylint](https://www.pylint.org/) for consistency. Both are run automatically
as part of the test suite.

## Running the tests

Tests can be run using `make`:

```
make test
```

This will create a virutal environment, install the module and its test
dependencies and run the tests. Alternatively you can do the same thing
manually:

```
python3 -m venv .venv
.venv/bin/pip install .[test]
.venv/bin/pytest
```

## License
[MIT](https://choosealicense.com/licenses/mit/)

Contents
--------

* [API Reference](api_ref.md)

**N.B. Starting with Python 3.10, `enum.StrEnum` is available in the standard
library.  This implementation is _not_ a drop-in replacement for the standard
library implementation. Sepcifically, the Python devs have decided to case fold
name to lowercase by default when `auto()` is used which I think violates the
principle of least surprise.**
