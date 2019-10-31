
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
