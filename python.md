# Learning Python
## [Decorator](https://www.python-course.eu/python3_decorators.php)
* decorator is a callable object that takes `Class` or `function` as input, modifiy it, and return another `class` or `function`
```
def decorator(func):
  def decorated():
    do some thing
    return
  return decorated

foo = decorator(foo)
```
* with decorator, syntext is simplified:
```
@decorator
def foo:
  pass
```
* Complicated Use Case: Nested decorator which return decorator given arguments
```
def outer_decorator(*outer_args,**outer_kwargs):                            
    def decorator(fn):                                            
        def decorated(*args,**kwargs):                            
            do_something(*outer_args,**outer_kwargs)                      
            return fn(*args,**kwargs)                         
        return decorated                                          
    return decorator       
    
@outer_decorator(1,2,3)
def foo(a,b,c):
    print a
    print b
    print c
foo()
```
