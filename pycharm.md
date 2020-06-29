[install blog](https://itsfoss.com/install-pycharm-ubuntu/)

# Install Using Snap
* Snap is a packaging software since Ubuntu1604.
* The packages dont have any dependencies.
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
snap run pycharm-professional
```
