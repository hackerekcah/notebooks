# Mean Teacher Experiment
## USPS->MNIST
* USPS
``` 
train: X.shape=(7291, 1, 28, 28), y.shape=(7291,)
val: X.shape=(0, 1, 28, 28), y.shape=(0,), 
test: X.shape=(2007, 1, 28, 28), y.shape=(2007,)
```
* MNIST
```
train: X.shape=(60000, 1, 28, 28), y.shape=(60000,), 
val: X.shape=(0, 1, 28, 28), y.shape=(0,), 
test: X.shape=(10000, 1, 28, 28), y.shape=(10000,)
```

## Code reading notes 
* combine batches
combine 1 batch from src domain and 1 batch from dst domain, concat and feed together to the net.  
    * Thus BatchNorm layer will use stats from both distributions, which would probably be bad.  
    * Calculate consistency loss from both labeled src domain and unlabeled dst domain
    
* unsup_weight
default to 3, why?
```
loss_expr = clf_loss + unsup_loss * unsup_weight
```

* `compute_aug_loss`
given student prob and teacher prob, calc consistency loss


