# Learning Python

## list documentation on objects (functions, variables, etc)
```
# help on sorted function
help(sorted)

# interactive help if not provide args
help()
```

## create dir
``` python
if not os.path.isdir(my_dir):
    os.makedirs(my_dir)
```

## listdir
``` python
import os

for filename in os.listdir(directory):
    if filename.endswith(".asm") or filename.endswith(".py"): 
```

## argparse, [api](https://docs.python.org/3/library/argparse.html),[tutorial](https://docs.python.org/3/howto/argparse.html)
* example
``` python
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
``` python
def decorator(func):
  def decorated():
    do some thing
    return
  return decorated

foo = decorator(foo)
```
* with decorator, syntext is simplified:
``` python
@decorator
def foo:
  pass
```
* Complicated Use Case: Nested decorator which return decorator given arguments. [more](https://www.codementor.io/sheena/advanced-use-python-decorators-class-function-du107nxsv)
``` python
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
``` python
matplotlib.is_interactive()
```
* turn on
``` python
matplotlib.pyplot.ion()
# or
matplotlib.interactive()
``` 
* turn off
``` python
matplotlib.pyplot.ioff()
```

## Run time
``` python
import timeit
start_time = timeit.default_timer()
# code you want to evaluate
elapsed = timeit.default_timer() - start_time
```

## function arguments
### func def
* arbitrary number of arguments, single star
``` python
def f(*args)
  print(args)
  
f(1,2,'x')

```
* arbitrary number of key word arguments, double star
``` python
def f(**kwargs)
  print(kwargs)
  
f(arg1=hello)
```

### function call
* single star to unpack a tuple as positional arguments
``` python
def f(x,y,z):
  print(x,y,z)

p = (1,2,3)
f(*p)
```

* double star to unpack a dictionary as like key words input
``` python
def f(x,y,z):
  print(x,y,z)
  
p = {'y':1, 'x':2, 'z':4}
f(**p)
```

### get arguments of a function
``` python
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
* config logging file on the fly
    * Both the handler level and logger level must be set
    * see [logging flow](https://docs.python.org/2/howto/logging.html#logging-flow)
```python
import logging

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# logging to file
fileh = logging.FileHandler('/tmp/logfile', 'a')
fileh.setFormatter(formatter)
fileh.setLevel('INFO')

# loggint to stdout
streamh = logging.StreamHandler(sys.stdout)
streamh.setLevel('INFO')
streamh.setFormatter(formatter)

logger = logging.getLogger()  # root logger
logger.setLevel('INFO')

for hdlr in logger.handlers[:]:  # remove all old handlers
    logger.removeHandler(hdlr)

logger.addHandler(fileh)
logger.addHander(streamh)
```

## logging argparse arguments
``` python
# return a argparse.Namespace object, which is an object that can get attribute through dot operation (e.g., b.a)
args = parser.parse_args()

# convert Namespace to string
logging.info(str(args))

# convert Namespace to dict, vars(obj) return obj's attribute and its value as a dict object
logging.info(vars(args))

# Best: pretty print
import pprint
logging.info(pprint.pformat(vars(args)))

# if args is a dict or object
import pprint
logging.info(pprint.pformat(vars(args)) if not isinstance(args, dict) else pprint.pformat(args))
```

## ignore warnings
```
import warnings
# can specify warning category
warnings.filterwarnings('ignore',category=FutureWarning)
```

## Read Audio Wave
``` python
import soundfile as sf

x, fs = sf.read('aaa.wav')

# scale to [-1, 1] if x is float
x = x / max(abs(x))
```

## [`yield`](https://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do)
* `yield` is used for `return` in a generator
* A generator is a iterable, but can only iterate onece, it generate values on the fly
* Not store all values in the memory, on the fly way
```
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator:
...    print(i)
0
1
4
```

## [import](https://docs.python.org/3/reference/import.html)
### modules and packages
* `packages` are special `modules`, like directories. 
* `modules` are python file with `.py` extention
* all packages are modules, not all modules are packages.
  * if a module has `__path__` attribute, its a package
* `package` needs to have `__init__.py` file in the directory.
  
### `__init__.py`
* when a package is imported, `__init__.py` file is implicitly executed, and the objects it defines are bound to names in the package’s namespace.
* when a subpackage is imported, parent package's `__init__.py` will also be excuted.
*  if a package’s __init__.py code defines a list named __all__, it is taken to be the list of module names that should be imported when from package import * is encountered
```
# sound_effect/__init__.py
__all__ = ["echo", "surround", "reverse"]

# then this command will import the three moduels in __all__
from sound_effect import *
```
### import process
* search for a module, then bind it to a name in local scope
### searching
* First look up `sys.modules`, a dictionary with recent imported moduels.
* Next, search `sys.meta_path`, a list of meta path finder objects
* Then `sys.path` , a list of locations may contain system path or zip file
### Finders and Loaders
* Finders return a `module spec`, exposed as `__spec__` attribte of the module
* Loaders excute the moduel
  * loader should excute module's code in modules global name space (module.__dict__)

### Add to search path
```
# insert a path to the front of the path list
sys.path.insert(0, '/to/your/path')
```

### Relative import
* Relative imports use leading dots
  * A single leading dot indicates a relative import, starting with the current package.
* absolute import use 
```
import <>
from <> import <>
```
* relative import can only use the second form
```
form .<> import <>
```

## [python -m <module-name>](https://docs.python.org/2/using/cmdline.html#cmdoption-m)
* <module-name>, without `.py`
* will search `sys.path` for the module
* if a package name is provided, <pkg>.__main__ is excuted.


