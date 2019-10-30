## What is `conda` ?
Conda是包、环境管理器，anaconda是conda的一个版本
## conda [channel](https://conda.io/docs/user-guide/tasks/manage-channels.html)
* conda 本身含有一个默认channel: defalts
* show all channels
```
conda config --show-sources
```
* 添加channel:
```
# 注意：conda的各个channel有可能包含相同的package
# 添加conda channel，其实就是在配置文件添加一个key-value的pair。
conda config --add channels conda-forege
#  优先级高
conda config --prepend channels conda-forge
#  优先级最低
conda config --append channels conda-forge
```
* 删除一个channel
```
vi .condarc 删除相应的项目即可
```
* 配置使用清华的channel，删除default channel
```
添加channel
 conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
 conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
删除defaults：
vim .condarc 删除defualts channel即可
添加defaults
vim .condarc 添加 - defaults

确认：
conda cofig --show
可以看到channels 下面没有default项了
```
* conda安装特定版本库
```
conda install pandas=0.18.1
```

* 删除特定库
```
conda remove pandas
```