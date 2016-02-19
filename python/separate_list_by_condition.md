#Separate a list based on a condition

The obvious way:
```python
good, bad = [], []
for x in mylist:
	if isgood(x):
		good.append(x)
	else:
		bad.append(x)
```

But we can surely do better.
Based on solution on [Stack Overflow](http://stackoverflow.com/questions/949098/python-split-a-list-based-on-a-condition)

```python
good, bad = [], []
for x in mylist:
	good.append(x) if x in goodvals else bad.append(x)
```

Two other variations:

```python
good, bad = [], []
for x in mylist:
    (bad, good)[x in goodvals].append(x)
```

and 

```python
good, bad = [], []
for x in mylist:
	(good if isgood(x) else bad).append(x)
```