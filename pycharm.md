[install blog](https://itsfoss.com/install-pycharm-ubuntu/)

# Install Using [Snap](https://snapcraft.io/docs/getting-started)
* Snap is a packaging software since Ubuntu1604.
* The packages dont have any dependencies.
```
# add /snap/bin to $PATH
```
export PATH="/snap/bin/:$PATH"
```
# list packages installed using snap
snap list

# install 
snap install <pkg>

# update
snap refresh <pkg>

# remove
snap remove <pkg>
```

## Snap install pycharm
```
sudo snap install pycharm-professional --classic

# update
sudo snap refresh pycharm-professional

# run
pycharm-professional

# run if /snap/bin is not added to $PATH
snap run pycharm-professional
```

## Debug slow
* [this solution](https://stackoverflow.com/questions/39371676/debugger-times-out-at-collecting-data)
* Enable Gevent compatible in the Python Debugger settings
