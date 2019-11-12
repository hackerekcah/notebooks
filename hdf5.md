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
* create nested group
```
f.create_group(“/some/long/path”)
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
* similar to numpy array
* each dataset has basic attribute like numpy array
```
.shape
.size
.dtype
```
* indexing like numpy array
* create dataset
```
# using existing array
>>> arr = np.arange(100)
>>> dset = f.create_dataset("init", data=arr)
```
### Attributes, also works like dict
```
dset.attrs['temperature'] = 99.5
```
* Attributs有如下特点
  * 可以由任何numpy array或者scalar创建
  * 每个attribute应该小 <64k
  * 不可以slice，读的时候必须整个读

