
## save and load
* save and load as `npy`, one array at a time
``` python
# .npy will be automatically added, i.e., test.npy
np.save('test', data)
data = np.load('test.npy')
```
* save and load as `npz`, multiple array
```
np.save('test', val1, mykey=val2)

# data will be like a dict, key would be 'arr_0', 'arr_1' if not specified
data = np.load('test.npz')
```

## prevent divide by zero
```
data = np.divide(data-mu, sigma, out=np.zeros_like(data), where=sigma!=0)
```

## [indexing and slicing](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html)
* slice will get a veiw of the original, i.e., a reference, wont copy
* basic indexing
```
# basic syntex, obj can be slice, integer, a tuple of slice or integer
x[obj]

# start:end:step slice
x[1:7:2]
```
* if dim of obj is less than N, then : is assumed
* Ellipsis expands to the number of : objects needed for the selection tuple to index all dimensions. 
```
>>> x = np.array([[[1],[2],[3]], [[4],[5],[6]]])
>>> x[...,0]
array([[1, 2, 3],
       [4, 5, 6]])
```

* `np.newaxis` add new axis, its a alias for 'None'
```
>>> x.shape
(2, 3, 1)
>>> x[:,np.newaxis,:,:].shape
(2, 1, 3, 1)

# or use None
x[:, None, :, :]
```
