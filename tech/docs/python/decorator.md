title: Decorator
authors: milo

# Decorator 装饰器 {:#}

装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。<br/>
装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。<br/>
概括的讲，装饰器的作用就是为已经存在的对象 **添加额外的功能** 。

假设已有一个函数：
```
:::python
def foo():
    print "foo"
```


现在需要统计函数耗时：
```
:::python
import time
import functools

def count_cost(func):
    @functools.wraps(func)
    def decorator():
        begin = time.time()
        result = func()
        end = time.time()
        print "count_cost %s" % (end - begin)
        return result
    return decorator
```

那么，
```
:::python
@count_cost
def foo():
    print "foo"
```
等价于
```
:::python
def foo():
    print "foo"
    
foo = count_cost(foo)
```

如果想要支持foo函数可以任意传入参数：
```
:::python
import time
import functools

def count_cost(func):
    @functools.wraps(func)
    def decorator(*args, **kwargs):
        begin = time.time()
        result = func()
        end = time.time()
        print "count_cost %s" % (end - begin)
        return result
    return decorator
    
@count_cost
def foo(param):
    print "foo param: %s" % param
```

如果想要支持装饰器可以任意传入参数（例如：函数执行时间超过n秒的才打印）：
```
:::python
import time
import functools

def count_cost(seconds):
    def outer_decorator(func):
        @functools.wraps(func)
        def decorator(*args, **kwargs):
            begin = time.time()
            result = func()
            end = time.time()
            if end - begin > seconds:
                print "count_cost %s" % (end - begin)
            return result
        return decorator
    return outer_decorator
    
# 函数执行时间超过5秒才打印
@count_cost(5)
def foo(param):
    time.sleep(param)
    print "foo param: %s" % param
```
