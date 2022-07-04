title: Metaclass
authors: milo

# metaclass {:#}

```
:::python
class A(object):
    def __new__(cls, *args, **kwargs):
        print "__new__()"
        return super(A, cls).__new__(cls, *args, **kwargs)
        
    def __init__(self):
        print "__init__()"
        return super(A, self).__init__()
        
A()
# __new__()
# __init__()
# <__main__.A object at 0x109f5db10>
```
先执行__new__
