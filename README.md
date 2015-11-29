# tabulator-py

[![Travis](https://img.shields.io/travis/okfn/tabulator-py.svg)](https://travis-ci.org/okfn/tabulator-py)
[![Coveralls](http://img.shields.io/coveralls/okfn/tabulator-py.svg?branch=master)](https://coveralls.io/r/okfn/tabulator-py?branch=master)

A utility library that provides a consistent interface for reading tabular data

## Usage

This section is intended to be used by end-users of the library.

### Getting Started

#### Installation

To get started (under development):

```
$ pip install tabulator
```

#### Simple interface

Fast access to the table with `topen` (stands for `table open`) function:

```python
from tabulator import topen, processors

with topen('path.csv') as table:
    table.add_processor(processors.Headers())
    for row in table.readrow(with_headers=True):
        print(row)
```

#### Advanced interface

To get full control over the process you can use `Table` class:

```python
from tabulator import Table, processors, loaders, parsers

table = Table(
        loader=loaders.File('path.csv', encoding='utf-8'),
        parser=parsers.CSV(delimiter=',', quotechar='|'))
table.add_processor(processors.Headers(row=1))
table.open()
headers = table.headers
contents = table.read(with_headers=True, limit=10)
print(headers, contents)
table.close()
```

#### Expert interface

All parts of Tabulator can be tweaked overriding main classes:

```python
from tabulator import Table, Iterator

class UserIterator(Iterator):

    ...

class UserTable(Table):

    ITERATOR_CLASS = UserIterator

    ...

table = UserTable(...)
...
```

### Design Overview

Loader API - [docstrings](https://github.com/okfn/tabulator-py/blob/master/tabulator/loaders/api.py)
Parser API - [docstrings](https://github.com/okfn/tabulator-py/blob/master/tabulator/parsers/api.py)
Processor API - [docstrings](https://github.com/okfn/tabulator-py/blob/master/tabulator/processors/api.py)

## Development

This section is intended to be used by tech users collaborating
on this project.

### Getting Started

To activate virtual environment, install
dependencies, add pre-commit hook to review and test code
and get `run` command as unified developer interface:

```
$ source activate.sh
```

### Reviewing

The project follow the next style guides:
- [Open Knowledge Coding Standards and Style Guide](https://github.com/okfn/coding-standards)
- [PEP 8 - Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)

To check the project against Python style guide:

```
$ run review
```

### Testing

To run tests with coverage check:

```
$ run test
```

Coverage data will be in the `.coverage` file.
