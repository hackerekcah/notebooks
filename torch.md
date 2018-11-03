# PyTorch Notes
## 1.Install
```
conda create -n torch3 python=3.6 pip
conda install pytorch torchvision -c pytorch
```
## 2. PyTorch vs Numpy
PyTorch could use GPU acceleration.

## 3.autograd package
* Forword pass will build a computation graph, autograd will do automatic differenciation on this graph
* Each operator of the computation graph should be an autograd operator/function
* We could define our own autograd function by inheriting torch.autograd.Function class, and define forword and backword methods

## 4.[torch.nn.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module)
* Porvide high level api above Tensor and autograd.
* Resemble the concept of layers, define forward computation and hold learnable parameters (internel state).
* Modules could have Modules inside, which will construct a tree-like hiearachy inside.
* User defined model should subclass Module class
### 4.1Methods
* .parameters() return all learnable parameters as a generator
* .zero_grad() zeros all parameters' grad
* .to(torch.device/torch.dtype/torch.tensor) move parameters & buffers to device/dtype/tensor(i.e. device+dtype) (modify in-place)
* .cuda(device) / .cpu() move paramters & buffers to gpu / cpu
* .modules() / .children() / .named_modules() / .named_children() / .named_parameters()
* .apply(fn) apply function recursively to each Modules inside

## 5.Control flow and dynamic graph
* Each forward pass will define a graph on the fly (dynamicly), thus could use python control flow (if / for etc.)

## 6.Weight sharing
* weight sharing by reusing the same Module instance multi-times in forward pass

## torch.Tensor
### Tensor creation
* if already have tensor myt, create a new with similar type but diff size by myt.new_\* methods
** .new_full(size,...) / .new_ones(size,...) / .new_empty(size,...) / .new_zeros(size,...) 