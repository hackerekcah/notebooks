# Learning Python

## 1. Help on objects (functions, variables, etc)
```
# help on sorted function
help(sorted)

# interactive help if not provide args
help()
```

## 2. Create dir
``` python
if not os.path.isdir(my_dir):
    os.makedirs(my_dir)
```

## 3. List directories and files
### `listdir`
``` python
import os
# os.listdir will return a list of string, each string is the name of the directories or file. without full path
for filename in os.listdir(directory):
    if filename.endswith(".asm") or filename.endswith(".py"): 
```
### [`glob.glob()`](https://docs.python.org/3/library/glob.html)
* Return a list of all the pathnames matching a specified pattern
* If recursive is true, the pattern “**” will match any files and zero or more directories, subdirectories and symbolic links to directories.
```
# current folder
1.gif
2.txt
card.gif
# sub_folder
3.txt

>>> import glob
>>> glob.glob('./[0-9].*')
['./1.gif', './2.txt']
>>> glob.glob('*.gif')
['1.gif', 'card.gif']
>>> glob.glob('?.gif')
['1.gif']
>>> glob.glob('**/*.txt', recursive=True)
['2.txt', 'sub/3.txt']
>>> glob.glob('./**/', recursive=True)
['./', './sub/']
```

## 4. argparse, [api](https://docs.python.org/3/library/argparse.html),[tutorial](https://docs.python.org/3/howto/argparse.html)
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
* The `Namespace` object
```
# parse_args() return a `arg_parse.Namespace` object

>>> parser.add_argument('--foo')
>>> args = parser.parse_args(['--foo', 'BAR'])
>>> vars(args)
{'foo': 'BAR'}

# use case
def parse_arguments():
   # ...
   # return as a dictionary
   return vars(args)
train(**parse_arguments())
```
### 4.1 Special care for `True` or `False` flag
```
parse.add_argument('--myflag', default=False, action='store_true')

# by default, this will have args.myflag=False
python main.py

# this will have args.myflag = True
python main.py --myfalg


def str2bool(v):
    if isinstance(v, bool):
       return v
    if v.lower() in ('yes', 'true', 't', 'y', '1'):
        return True
    elif v.lower() in ('no', 'false', 'f', 'n', '0'):
        return False
    else:
        raise argparse.ArgumentTypeError('Boolean value expected.')
        
parse.add_argument('--myflag', default=False, type=str2bool)
```
### 4.2 All arguments are associated with `action`, by default is `store`
 * `store` simply add an attribute to the object returned by parse_args().
```
# same as parser.add_argument('--foo', action=store)
parser.add_argument('--foo')
```

### 4.3 [`add_argument`](https://docs.python.org/3/library/argparse.html#the-add-argument-method)
```
help - A brief description of what the argument does.
metavar - A name for the argument in usage messages. (only for display)
dest - The name of the attribute to be added to the object returned by parse_args().
default -The value produced if the argument is absent from the command line
        optional values if not provided, default to `None`
required - For optional args only, if `True`, then must provide
```
#### 4.3.1 [`nargs`](https://docs.python.org/3/library/argparse.html#nargs)
* `nargs=N`
```
parser.add_argument('--foo', nargs=2)
# Pass `--foo a b`, will reutrn a list.
Namespace(foo=['a', 'b'])

parser.add_argument('--foo', nargs=1)
# Pass `--foo a`, will reutrn a list even if only one provided
Namespace(foo=['a'])
```
* `nargs=?`, zero or one argument
* `nargs=*`, zero or more argument, return in a list
* `nargs=+`, at least one argument, return in a list

## 5. [Decorator](https://www.python-course.eu/python3_decorators.php)
* decorate classes, e.g., register architectures
``` python
# models.__init__.py
ARCH_REGISTRY = dict()


def register(cls):
    ARCH_REGISTRY[cls.__name__] = cls
    return cls


from models import conv_trans, group_conv_trans


__all__ = ["conv_trans", "group_conv_trans", "register"]
```
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
## 6. Matplotlib
### 6.1 Interactive Mode
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

## 7. Run time
``` python
import timeit
start_time = timeit.default_timer()
# code you want to evaluate
elapsed = timeit.default_timer() - start_time
```

## 8. function arguments
### 8.1 func def
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

### 8.2 function call
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

### 8.3 get arguments of a function
``` python
# inside function
saved_args = locals()
print(saved_args)
```
## 9. configparser
* read from file
```python
config = configparser.ConfigParser()
config.read(os.path.join(ROOT_DIR, config_file))
```
* quick print config
``` python
print({section: dict(config[section]) for section in config.sections()})
```
## 10. logging
* module logger
``` python
import sys
logging.basicConfig(
    format="%(asctime)s | %(levelname)s | %(name)s | %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
    level=logging.INFO,
    stream=sys.stdout,
)
logger = logging.getLogger("env2vec.manifest")
```
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
logger.addHandler(streamh)
```

## 11. logging argparse arguments
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

## 12. ignore warnings
```
import warnings
# can specify warning category
warnings.filterwarnings('ignore',category=FutureWarning)
```

## 13. Read Audio Wave
``` python
import soundfile as sf

x, fs = sf.read('aaa.wav')

# scale to [-1, 1] if x is float
x = x / max(abs(x))
```

## 14. [`yield`](https://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do)
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

## 15. [import](https://docs.python.org/3/reference/import.html)
### 15.1 modules and packages
* `packages` are special `modules`, like directories. 
* `modules` are python file with `.py` extention
* all packages are modules, not all modules are packages.
  * if a module has `__path__` attribute, its a package
* `package` needs to have `__init__.py` file in the directory.
  
### 15.2 `__init__.py`
* when a package is imported, `__init__.py` file is implicitly executed, and the objects it defines are bound to names in the package’s namespace.
* when a subpackage is imported, parent package's `__init__.py` will also be excuted.
*  if a package’s __init__.py code defines a list named __all__, it is taken to be the list of module names that should be imported when from package import * is encountered
```
# sound_effect/__init__.py
__all__ = ["echo", "surround", "reverse"]

# then this command will import the three moduels in __all__
from sound_effect import *
```
### 15.2.1 `__main__`
* module `__main__.py` under `somepackage` will be run if run a package `python -m somepackage`
* When a module is run, its `__name__` attribute will be replaced by `__main__`
### 15.3 import process
* search for a module, then bind it to a name in local scope
### 15.4 searching
* First look up `sys.modules`, a dictionary with recent imported moduels.
* Next, search `sys.meta_path`, a list of meta path finder objects
* Then `sys.path` , a list of locations may contain system path or zip file
### 15.5 Finders and Loaders
* Finders return a `module spec`, exposed as `__spec__` attribte of the module
* Loaders excute the moduel
  * loader should excute module's code in modules global name space (module.__dict__)

### 15.6 Add to search path
```
# insert a path to the front of the path list
sys.path.insert(0, '/to/your/path')
```

### 15.7 Relative import
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

### 15.8 List imported modules in local scope
``` python
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'sys']
```

### 15.9 `PYTHONPATH` environment variable
* `PYTHONPATH` will be added to `sys.path`
* Pycharm will automatically add content root and source root to PYTHONPATH
* export a python path
```
export PYTHONPATH='/some/path'
```

## 16. [python -m `<module-name>`](https://docs.python.org/2/using/cmdline.html#cmdoption-m)
* `<module-name>`, without `.py`
* will search `sys.path` for the module
* if a package name is provided, `<pkg>.__main__` is excuted.

## 17. `OrderedDict`
* rderedDict preserves the order in which the keys are inserted.
```
from collections import OrderedDict
```

## 18 `@classmethod` vs `@staticmethod`
* [see](https://www.geeksforgeeks.org/class-method-vs-static-method-python/)
* classmethod take cls itself as arg, can change class state, will be reflected in all class instances
```
class C(object):
    @classmethod
    def fun(cls, arg1, arg2, ...):
       ....

    @staticmethod
    def fun(arg1, arg2, ...):
        ...
```

## 19 YAML format.
* [documentation](https://pyyaml.org/wiki/PyYAMLDocumentation), the following sections are helpful.
  * `YAML syntax`
  * `YAML tags and Python types`
* [Yaml syntax](https://yaml.org/spec/1.1/#id857168)
* [Yaml cookbook](https://yaml.org/YAML_for_ruby.html)
* [nice tutorial](https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/)
* install and import
``` 
pip install pyyaml
import yaml
```
* read yaml file
```
cfg_dict = yaml.full_load(open(cfg_file, 'r'))
```

