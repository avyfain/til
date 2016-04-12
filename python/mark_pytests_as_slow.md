# Skipping slow py.test tests

Sometimes, we have different kinds of tests, which we don't want to always run. For example, slow tests:

```python
import time
import pytest

def test_fast():
    assert 1 + 1 == 2

def test_slowness():
    time.sleep(10)
    assert 2 + 2 == 4
```

```bash
$ py.test
============================= test session starts ================================
platform darwin -- Python 3.4.3, pytest-2.8.2, py-1.4.30, pluggy-0.3.1
rootdir: /Users/avyfaingezicht/testexample, inifile: pytest.ini
collected 2 items

tests/test_stuff.py ..
============================ 15 passed in 10.23 seconds ============================
```

Pytest allows you to create arbitrary markers, and using flags from the command line set up special logic when the flags are present (or not).

By adding the following `conftest.py` file at the root of your project's directory, you can achieve exactly that:

```python
import py.test

def pytest_addoption(parser):
    parser.addoption('--slow',
                     action='store_true',
                     default=False,
                     help='Also run slow tests')

def pytest_runtest_setup(item):
    """Skip tests if they are marked as slow and --slow is not given"""
    if getattr(item.obj, 'slow', None) and not item.config.getvalue('slow'):
        py.test.skip('slow tests not requested')
```

Then its as easy as marking your test:

```python
import time
import pytest

def test_fast():
    assert 1 + 1 == 2

@pytest.mark.slow
def test_slowness():
    time.sleep(10)
    assert 2 + 2 == 4
```

```bash
$ py.test
============================= test session starts ================================
platform darwin -- Python 3.4.3, pytest-2.8.2, py-1.4.30, pluggy-0.3.1
rootdir: /Users/avyfaingezicht/testexample, inifile: pytest.ini
collected 2 items

tests/test_stuff.py .s
============================ 15 passed in 0.23 seconds ==========================
```

On the other hand:

```bash
$ py.test --slow
============================= test session starts ================================
platform darwin -- Python 3.4.3, pytest-2.8.2, py-1.4.30, pluggy-0.3.1
rootdir: /Users/avyfaingezicht/testexample, inifile: pytest.ini
collected 2 items

tests/test_stuff.py ..
============================ 15 passed in 10.21 seconds ============================
```