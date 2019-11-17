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
## Install GPU driver

* [GPU Driver vs GPU CUDA Toolkit](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#binary-compatibility__table-toolkit-driver)  
1）打开终端，先删除旧的驱动：
```
sudo apt-get purge nvidia*
```
2）Disable Nouveau
* nouveau is a open source gpu driver project for supporting nvidia drivers
* nouveau did not support CUDA.
```
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
# add these line
blacklist nouveau
options nouveau modeset=0

# Regenerate the kernel initramfs
sudo update-initramfs -u
```
* Reboot
```
# reboot
sudo reboot

# check if Nouveau is killed
lsmod | grep nouveau
```

3）重启系统至init 3（文本模式），也可先进入图形桌面再运行init 3进入文本模式，再安装下载的驱动就无问题，

首先我们需要结束x-window的服务，否则驱动将无法正常安装

关闭X-Window，很简单：sudo service lightdm stop，然后切换tty1控制台：Ctrl+Alt+F1即可

4）接下来就是最关键的一步了：sudo./NVIDIA.run开始安装，安装过程比较快，根据提示选择即可最后安装完毕后，重新启动X-Window：sudo service lightdm start，然后Ctrl+Alt+F7进入图形界面；

* [Links to Nvidia official gpu driver download page](https://www.nvidia.com/Download/index.aspx?lang=en-us)
