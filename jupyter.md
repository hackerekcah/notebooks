# 1.Jupyter vs Jupyter Notebook
Jupyter Notebook is an web app, part of Jupyter project

# 2. [Jupyter Notebook](https://jupyter-notebook.readthedocs.io/en/stable/)

# 3. Kernels
Kernels wraps languages & provice calculation for frontend, e.g. ipython for python
## 3.1 [Install Kernels](https://ipython.readthedocs.io/en/latest/install/kernel_install.html)
- install kernel specs for envs
```
source activate myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```
## 3.2 [Kernel Specs](https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs)
- what is kernelspec  
each kernelspec is a directory installed by ipykernel, contains configuration of a kernel
- list available kernelspecs
```
jupyter kernelspec list
````
- delete a kernel spec  
delete the kernelspec directory

# 4 [Migics in Ipython](https://ipython.readthedocs.io/en/stable/interactive/magics.html)
Magic commands start with %
## 4.1 Line magics
* set up env
```
%env CUDA_VISIBLE_DEVICES 5
or
%set_env CUDA_VISIBLE_DEVICES 5
```
* matplotlib show plot  
	%matplotlib inline
* print info an object
	%pinfo \[obj\] / %pinfo2 \[obj\]
	same as: obj? / obj??
* print signature of a callable
	%pdef \[callable\]
* import numpy, matplotlib and activate interactive support
	%pylab
* run shell command
	!COMMAND #run command
	!!COMMAND #run command and get output as a list
* print variables
	%who	#pirnt interactive variables
	%whos	#print interactive variables, with extra infomation
