# SPLAB Code Demonstration
## Motivation:
* Demo our applications (for LAN)
    * e.g. speech recognition, speaker verification, event classification, emotion recognition
* Requirements
    * all phd students
    * time response is not critical
    * easy to demo, better provide one click demo

## Design Architectures
### Overall
* Client-Server Architec using `Jupyter notebook`
   * Each app as a service on the server side
   * `Jupyter notebook` allow powerfull application side design 
### IP, Port, Directories
* Each app serves at a unique `LAN_IP:PORT`
* Each app has a jupyterlab dir under `/data/splab_demo/`
```
# LAN_IP: PORT map to a notebook-dir
192.168.11.214:7771/lab :: /data/splab_demo/acoustic_scene_classification
192.168.11.214:7772/lab :: /data/splab_demo/speech_enhancement
```
* Audio will be uploaded to the following directory (using `jupyter lab`) 
```
/data/splab_demo/acoustic_scene_classification/audio
```
* Create your `.ipynb` file and run your program iside your juypter notebook

## Server side
### 1. Install and setup jupyter ipython kernel for your python environment
``` bash
# activate environment
source activate myenv

# install jupyter
pip install jupyter

# install ipykernel module
pip install ipykernel

# create Ipython kernel for myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```
### 2. Config jupyter to allow remote acess with password
* 2.1 generate notebook config file
``` bash
# will generate ~/.jupyter/jupyter_notebook_config.py
jupyter notebook --generate-config
```
* 2.2 config password for jupyter notebook
      * SECURITY WARNING: leak of password will allow anyone to acess your data and program
```
# will generate ~/.jupyter/jupyter_notebook_config.json, which save the password hash
jupyter notebook password
```

### 3. Create your demo app directory inside `/data/splab_demo/`
* also you can put your excutable inside your application directory
``` bash
mkdir /data/splab_demo/acoustic_scene_classification/
```

### 4. Start the jupyter notebook/lab server

* `ip=0.0.0.0` to allow acess from any remote ip 
* `--port` and `--notebook-dir` *SHOULD* match your demo application!!
* `--no-browser`: dont automatically open webbrowser
* `&`: run backend so that terminal can be closed
``` bash
jupyter notebook --ip=0.0.0.0 --port=7771 --no-browser --notebook-dir=/data/splab_demo/acoustic_scene_classification &
```

# Client side
* open web browser and visit `http://<ip>:<port>/lab`
* input password
* create your jupyter notebook inside the directory, e.g. `main.ipynb`
* call your program inside a `code cell`
```
# ! : means call a bash command
!/home/songhongwei/anaconda2/envs/asc_mt/bin/python main.py audio/tram-vienna-285-8639-a.wav
```
# Q&A
> Q: For matlab applications?  
> A: Two options, 
>> option 1, just call matlab inside the `code cell` (prefered)  
```
# ! : means call a bash command
!/path/to/matlab main.m audio/tram-vienna-285-8639-a.wav
```
>> option 2: install jupyter notebook kernels for matlab. (Needs to install matlab kernels, not covered here)
