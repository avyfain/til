# Find 1st item matching a predicate in an iterable

As simple as it sounds. I've actually used this one many times before, but today I had to look up how to get a default value if no match is found.

```python
next((x for x in seq if predicate(x)), None)
```

Sample usage:
```python
>>> next(item for item in range(5) if f>2)
3
>>> next(item for item in range(5) if f>10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> next((f for f in range(5) if f>10), 101)
101
```
