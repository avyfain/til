# Create Context Managers with `yield`

Context managers are one of python's most interesting pieces of syntactic sugar.
Perlis' might have said in his Epigrams that "Syntactic sugar causes cancer of the semicolon," but there is no doubt that having the `with` keyword, and making its use the standard way to do certain things in python, has made the language better.

From simple file opening, to building timers, to set-up and tear-down for tests, context managers are great.

Today I learned that you can use the `@contextmanager` decorator, and `yield` from a function instead of defining `__enter__` and  `__exit__`:

```python
from contextlib import contextmanager

@contextmanager
def open_file(path, mode):
    the_file = open(path, mode)
    yield the_file
    the_file.close()

files = []

for x in range(100000):
    with open_file('foo.txt', 'w') as infile:
        files.append(infile)
```

[Source](http://jeffknupp.com/blog/2016/03/07/python-with-context-managers/)