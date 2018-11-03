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

## 7.torch.Tensor
### [Attributes](https://pytorch.org/docs/stable/tensor_attributes.html)
* torch.dtype
* torch.device
  * device type and ordinal, e.g. torch.device("cpu") / torch.device("cuda:0")
  * specify by string('cuda:0'), by torch.device('cuda:0'), or by int(0)(*legacy*) 
  * if not specify ordinal, then torch.cuda.current_device() will be used
* torch.layout: memory layout of the tensor
### Tensor creation
* if already have tensor myt, create a new with similar type but diff size by myt.new_\*() methods
  * .new_full(size,...) / .new_ones(size,...) / .new_empty(size,...) / .new_zeros(size,...) 
* if pre-existing data
 * torch.tensor()
 
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
modelB.load_state_dict(torch.load(PATH), strict=False)
```
### 8.6 save&load cross device, see [torch.load](https://pytorch.org/docs/stable/torch.html#torch.load)
* if save on GPU, load on CPU
    device = torch.device("cpu")
    model.load_state_dict(torch.load(PATH, map_location=device))
* if save on GPU, load on GPU
    device = torch.device("cuda:1")
    model.load_state_dict(torch.load(PATH))
    model.to(device)
    input = input.to(device)
* if save on CPU, load on GPU
    device = torch.device("cuda")
    model.load_state_dict(torch.load(PATH, map_location="cuda:0"))
    model.to(device)
