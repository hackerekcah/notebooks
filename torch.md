# PyTorch Notes
## 1.Install
```
conda create -n torch3 python=3.6 pip
conda install pytorch torchvision -c pytorch
```
### 1.1 command line cuda
```
CUDA_VISIBLE_DEVICES=5 python myapp.py
```
## 2. PyTorch vs Numpy
* Conceptually the same, BUT, PyTorch could use GPU acceleration.
* Tenosr to numpy (**NOTE: share same memory**):
` b = a.numpy()`
* numpy to tensor (**NOTE: share same memory**):
` b = torch.from_numpy(a)`

## 3.autograd package
### 3.1 basics
* Forword pass will build a computation graph, autograd will do automatic differenciation on this graph
* Each operator of the computation graph should be an autograd operator/function
* We could define our own autograd function by inheriting torch.autograd.Function class, and define forword and backword methods
### 3.2 record history
* Each tensor has `.grad` `.requires_grad` attribute
	* `.grad` accumulate gradient if `.requires_grad == True`
	* set `a.requires_grad_ = True` to change attributes in-place
* [`b = a.detach()` vs `b = a.data`](https://pytorch.org/blog/pytorch-0_4_0-migration-guide/#what-about-data)
	* operation on `b` will not be recorded in autograd history, however change `b` in-place will affect `a`
	* `.detach()` guarentee to be safe, i.e. if changed `b` in-place, when `.backward()` through `a` will trigure error

## 4.[torch.nn.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module)
* Porvide high level api above Tensor and autograd.
* Resemble the concept of layers, define forward computation and hold learnable parameters (internel state).
* Modules could have Modules inside, which will construct a tree-like hiearachy inside.
* User defined model should subclass Module class
### 4.1 Methods
* `.parameters()` return all learnable parameters as a generator
* `.zero_grad()` zeros all parameters' grad
* `.to(torch.device/torch.dtype/torch.tensor)` 
  * move parameters & buffers to device/dtype/tensor(i.e. device+dtype) (modify in-place)
* `.cuda(device) / .cpu()` move paramters & buffers to gpu / cpu
* `.modules() / .children() / .named_modules() / .named_children() / .named_parameters()`
* `.apply(fn)` apply function recursively to each Modules inside

### 4.2 Define my own buffers
```
# register in __init__, then can use self.my_buffer_name in forward()
self.register_buffer('my_buffer_name', torch.tensor(3.0, dtype=torch.float))
```

## 5.Control flow and dynamic graph
* Each forward pass will define a graph on the fly (dynamicly), thus could use python control flow (if / for etc.)

## 6.Weight sharing
* weight sharing by reusing the same `Module` instance multi-times in forward pass

## 7.torch.Tensor (see [examples](https://jhui.github.io/2018/02/09/PyTorch-Basic-operations/))
### [Attributes](https://pytorch.org/docs/stable/tensor_attributes.html)
* `torch.dtype`
* `torch.device`
  * device type and ordinal, e.g. torch.device("cpu") / torch.device("cuda:0")
  * specify by string('cuda:0'), by torch.device('cuda:0'), or by int(0)(*legacy*) 
  * if not specify ordinal, then torch.cuda.current_device() will be used
* `torch.layout`: memory layout of the tensor
### Tensor metadata
* `mt.size()` / `mt.shape` # same output
* `mt.type()` / `mt.dtype` # sampe output
* type conversion: [`.type(dtype)`](https://pytorch.org/docs/stable/tensors.html#torch.Tensor.type) methods
    * `x.type(torch.FloatTensor)` # convert to FloatTensor
    * `x.as_type(y)` # convert to same dtype as y
    * see a list of torch [dtypes](https://pytorch.org/docs/stable/tensors.html)
### Tensor creation
* torch.Tensor(2,3) is a simplification of torch.FloatTensor(2,3) # uninitialized
* if already have tensor myt, create a new with similar type but diff size by myt.new_\*() methods
  * .new_full(size,...) / .new_ones(size,...) / .new_empty(size,...) / .new_zeros(size,...) 
* create tensor with same attribute (size, requires_grad...)
	* `torch.*_like()` / `a.*_like()`
* if pre-existing data, `torch.tensor()` is lke `numpy.array()`
  * `torch.tensor(data...)`, copies data and new a tensor
### Random initialized tensor
```
torch.manual_seed(1)	# reproducibility
v = torch.rand(2, 3)            # Initialize with random number (uniform distribution)
v = torch.randn(2, 3)           # With normal distribution (SD=1, mean=0)
v = torch.randperm(4)           # Size 4. Random permutation of integers from 0 to 3
```
 ## 8. [Model Save and Load](https://pytorch.org/tutorials/beginner/saving_loading_models.html#)
 ### 8.1 save and load model parameters (recommended)
 * convention: mypath end with *.pt* / *.pth*
 ```
 torch.save(model.state_dict(),mypath)
 mymodel.load_state_dict(torch.load(path))
 ```
 ### 8.2 save and load whole model
 * convention: mypath end with *.pt* / *.pth*
 ```
 torch.save(mymodel, mypath)
 mymodel = torch.load(mypath)
 ```
 
 ### 8.3 torch.optim also has .state_dict() methods
 ### 8.4 save a general checkpoint, save a dict
 * convention: PATH end with *.tar*
 ```
 torch.save({
            'epoch': epoch,
            'model_state_dict': model.state_dict(),
            'optimizer_state_dict': optimizer.state_dict(),
            'loss': loss,
            ...
            }, PATH)
```
### 8.5 load partial model / partial parameters, set *strict=False*
```
# allow state 'key' mismatch between saved model and target model
modelB.load_state_dict(torch.load(PATH), strict=False)
```
### 8.6 save&load cross device, see [torch.load](https://pytorch.org/docs/stable/torch.html#torch.load)
* if save on GPU, load on CPU
```
device = torch.device("cpu")
model.load_state_dict(torch.load(PATH, map_location=device))
```
* if save on GPU, load on GPU
```
device = torch.device("cuda:1")
model.load_state_dict(torch.load(PATH))
model.to(device)
input = input.to(device) # move input to device and overide
```
* if save on CPU, load on GPU
```
device = torch.device("cuda")
model.load_state_dict(torch.load(PATH, map_location="cuda:0"))
model.to(device)
input = input.to(device) # move input to device and overide
```
## 9. Weight initialization
## 10. [Data Loading & Processing](https://pytorch.org/tutorials/beginner/data_loading_tutorial.html)
### 10.1related class
`from torch.utils.data import Dataset, DataLoader` 
### 10.2 What?  
`Dataset`: sample indexing & transform
`DataLoader`: batching, shuffle, parallel loading
### 10.3 `Dataset` How?
* 1.define a new class inheriting `Dataset`
* 2.impleting `__len__`, `__getitem__`, perform binded `transformation` when return sample
```
class FaceLandmarksDataset(Dataset):
    def __init__(self, csv_file, root_dir, transform=None):
        self.transform = transform

    def __len__(self):
        return len(self.landmarks_frame)

    def __getitem__(self, idx):
        if self.transform:
            sample = self.transform(sample)
        return sample
```
* 3.Define *callable* transform class, and input to `Dataset` object
```
class MyTransformClass:
	#define __call__ to make class callable
	def __call__(self, sample):
		pass

tsfm = MyTransformClass(params)
transformed_sample = tsfm(sample)
```
* 3.1could also compose transform and input to `Dataset` object
```
composed = transforms.Compose([Rescale(256),
			       RandomCrop(224)])
```
### 10.4 `DataLoader` How?
```
dataloader = DataLoader(transformed_dataset, batch_size=4,
                    	shuffle=True, num_workers=4)
			
for i_batch, sample_batched in enumerate(dataloader):
	pass
```
### 11. Losses
#### 11.1[CrossEntropy](https://github.com/hackerekcah/notebooks/blob/master/cross_entropy.ipynb)
* The following two way both ok
    * `target`: need to be `torch.LongTensor` type, not one-hot but `int` labels
```
# by default, return mean loss of the observations in a minibatch
criterion = torch.nn.CrossEntropyLoss()

# alternatively, return sum loss of the observations in a minibatch, by set `reduction='sum'`
# criterion = torch.nn.CrossEntropyLoss(reduction='sum')

loss = criterion(logits, target)
```
#see mnist [example](https://github.com/pytorch/examples/tree/master/mnist)
```
log_pred = F.log_softmax(logits, dim=1)
loss = F.nll_loss(log_pred, target)
```
### 12. random number generator
#### 12.1 get/set cpu/cuda rng state
```
>>> print(torch.get_rng_state(), torch.get_rng_state().size())
tensor([ 41, 118,   0,  ...,   0,   0,   0], dtype=torch.uint8) torch.Size([5048])

>>> print(torch.cuda.get_rng_state(4), torch.cuda.get_rng_state(4).size())
tensor([  8, 163, 175,  ...,   0,   0,   0], dtype=torch.uint8) torch.Size([824016])

>>> print(torch.cuda.get_rng_state(5), torch.cuda.get_rng_state(5).size())
tensor([178, 199, 104,  ...,   0,   0,   0], dtype=torch.uint8) torch.Size([824016])
```
### 13. reproducible results
``` python
def set_seed(seed):
    torch.manual_seed(seed)
    torch.cuda.manual_seed(seed)
    np.random.seed(seed)
    random.seed(seed)
    # must set to True
    torch.backends.cudnn.deterministic = True
```

### 14. `model.eval()` vs with `torch.no_grad():`
* `model.eval()` make `BatchNorm` and `Dropout` layer works in evaluation mode
* `torch.no_grad()` ignore gradient calculation, faster and less memory usage during evaluation

### 15. move tensor to cpu or gpu
```
# must assign back to the tensor
my_tensor = my_tensor.cpu()
```

### 16. [torch.nn.DataParallel](https://pytorch.org/docs/stable/nn.html?highlight=torch%20nn#dataparallel)
* [tutorial](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html)
* [multi-gpu example](https://pytorch.org/tutorials/beginner/former_torchies/parallelism_tutorial.html)
* split the mini-batch of samples into multiple smaller mini-batches 

```
# a list of gpu ids (int)
args.device_ids = [0, 1, 2, 3]

# wrap model for multi-gpus
if len(args.device_ids) > 1:
	model = torch.nn.DataParallel(module=model, device_ids=args.device_ids)

# must move model to the first gpu of the id list
device = torch.device('cuda:{}'.format(args.device_ids[0]))

model = model.to(device)
```

## 17. hooks
*  `Module.register_forward_hook(self, hook)`
* The hook is called after .forward() is called
```
# forward hook's signature
hook(module, input, output) -> None or modified output
```
* `register_backward_hook(self, hook)`
  * The :attr:`grad_input` and :attr:`grad_output` may be tuples if the module has multiple inputs or outputs.
  * The hook is called every time the gradients with respect to module inputs are computed
```
# backward hook's signature
hook(module, grad_input, grad_output) -> Tensor or None
```
