## Hierarchical Data Format
* Every object (Groups or Dataset) in an HDF5 file has a name
* Names are arranged in a POSIX-style hierarchy with `/`-separators
### Open h5 file
* `f` is the entry, as well as a root group.
```
f = h5py.File('mytestfile.hdf5', 'r')
```
### Groups
* like folders, can have contents (i.e. datasets), and subfolders (i.e. sub groups)
* works like a python dict, `.keys()`, `.values()`, `.items()`, `iter()` and `get()`
* create group
```
>>> grp = f.create_group("bar")
>>> grp.name
'/bar'
>>> subgrp = grp.create_group("baz")
>>> subgrp.name
'/bar/baz'
```
* iterating over a group only yields its directly-attached members
* iter over whole file using `.visit()` and `.visititems()` methods of group.
  * [.visit()](http://docs.h5py.org/en/stable/high/group.html#Group.visit)
```
>>> def printname(name):
...     print name
>>> f.visit(printname)
mydataset
subgroup
subgroup/another_dataset
subgroup2
subgroup2/dataset_three
```
### Dataset

### Attributes, also works like dict
```
dset.attrs['temperature'] = 99.5
```
