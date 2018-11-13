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
* Complicated Use Case: Nested decorator which return decorator given arguments[more](https://www.codementor.io/sheena/advanced-use-python-decorators-class-function-du107nxsv)
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
## Matplotlib
### Interactive Mode
plt.plot() method will not plot to screen by default, unless call plt.show()
interactively means each time add something to the plot.
* check if interactive mode on
```
matplotlib.is_interactive()
```
* turn on
```
matplotlib.pyplot.ion()
# or
matplotlib.interactive()
```
* turn off
```
matplotlib.pyplot.ioff()
```
