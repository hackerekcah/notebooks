# Tensorflow

## [tensorflow-gpu and CUDA version compatbility](https://www.tensorflow.org/install/source#linux)

## [install](https://www.tensorflow.org/install/pip#3.-%E5%AE%89%E8%A3%85-tensorflow-pip-%E8%BD%AF%E4%BB%B6%E5%8C%85)
* CUDA9.0 compatible with tensorlfow <1.13
* install tensorflow-gpu 1.12, only compatible with python <= 3.6
```
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.12.0-cp36-cp36m-linux_x86_64.whl
```

## Tensorflow and Keras
Keras 2.2.5 is the last version with tensorflow 1

## Tensorflow version
* 1.15 is the last 1.X release.
* For 1.15 tensorflow is in same package with tensorflow-gpu
```
conda install tensorflow=1.15
```
* 1.13.1 is compatible with CUDA10.0, 1.14 and 1.15 is not

