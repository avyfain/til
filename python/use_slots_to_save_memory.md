# Use `__slots__` To Save Memory
By default, python adds a dictionary `__dict__` to every instance of a class. However, when you use __slots__, any object created for that class won't have a __dict__ attribute. Instead, all attribute access is done directly via pointers.

Example, taken from [Raymond Hettinger's class toolkit talk](https://speakerdeck.com/pyconslides/pythons-class-development-toolkit-by-raymond-hettinger):
```python
class Circle(object):
    'An advanced cirlce analytic toolikt'
    
    # flyweight design pattern supresses the instance dictionary
    
    __slots__ = ['diameter']
    version = '0.x'
    
    def __init__(self, radius):
        self.radius = radius
        
    @property
    def radius(self):
        self.diameter / 2.0
        
    @radius.setter
    def radius(self, radius):
        self.diameter = radius * 2.0
```

Now, when you create 1M circles, you're only storing their diameter!
