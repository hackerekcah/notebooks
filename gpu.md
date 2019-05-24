# GPU

## Git hub: Nvidia/apex
* Automatic Mixed Precision (AMP)
    * to speed up net training, utilizing the Tensor Core microarchitecture
    * Tensor Core is enabled Volta, Turing microarchitecture, so not supported for Tesla Kepler series
* AMP is easy to use with 3 lines of code

## [NVIDIA GPU Cloud](https://www.nvidia.com/en-us/gpu-cloud/)
* A hub for GPU optimized software
* Docker containers ready to be download and use

## NVIDIA GPUs
* Desktop/Mobile/Worstation series
* Tesla is designed for worstation/server usage, without graphic output, only for parallel massive computing

## GPU vs CPU
* CPU
   * optimised for sequencial serial processing
   * wide range of instructions
* GPU
   * initially for image rendering
   * Focus on small instruction set
   * Massive parallel computing, focus on throuput
