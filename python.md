# Learning Python
## argparse, [api](https://docs.python.org/3/library/argparse.html),[tutorial](https://docs.python.org/3/howto/argparse.html)
* example
```
def run(args):
  pass
if __name__ == '__main__':
  parser = argparse.ArgumentParser(description='Process some integers.')
  # positional arguments
  parser.add_argument('run')
  # optional arguments
  parser.add_argument('--pooling', type = str, default = 'lin', choices = ['max', 'ave', 'lin', 'exp', 'att'])
  parser.add_argument('--dropout', type = float, default = 0.0)
  args = parser.parse_args()
  run(args)
```
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
* Complicated Use Case: Nested decorator which return decorator given arguments. [more](https://www.codementor.io/sheena/advanced-use-python-decorators-class-function-du107nxsv)
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

## Run time
```
import timeit
start_time = timeit.default_timer()
# code you want to evaluate
elapsed = timeit.default_timer() - start_time
```

## function arguments
### func def
* arbitrary number of arguments, single star
```
def f(*args)
  print(args)
  
f(1,2,'x')

```
* arbitrary number of key word arguments, double star
```
def f(**kwargs)
  print(kwargs)
  
f(arg1=hello)
```

### function call
* single star to unpack a tuple as positional arguments
```
def f(x,y,z):
  print(x,y,z)

p = (1,2,3)
f(*p)
```

* double star to unpack a dictionary as like key words input
```
def f(x,y,z):
  print(x,y,z)
  
p = {'y':1, 'x':2, 'z':4}
f(**p)
```

### get arguments of a function
```
# inside function
saved_args = locals()
print(saved_args)
```
## configparser
* read from file
```python
config = configparser.ConfigParser()
config.read(os.path.join(ROOT_DIR, config_file))
```
* quick print config
``` python
print({section: dict(config[section]) for section in config.sections()})
```
## logging
* config logging
```python
# logging.basicConfig does nothing if a handler has been set up already:
logging.basicConfig(filename=log_file, level='INFO', None))
```
* config logging file on the fly \\
    * Both the handler level and logger level must be set
    * see [logging flow](https://docs.python.org/2/howto/logging.html#logging-flow)
```python
import logging

fileh = logging.FileHandler('/tmp/logfile', 'a')
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
fileh.setFormatter(formatter)
fileh.setLevel('INFO')

logger = logging.getLogger()  # root logger
logger.setLevel('INFO')

for hdlr in logger.handlers[:]:  # remove all old handlers
    logger.removeHandler(hdlr)
logger.addHandler(fileh)      # set the new handler
```

