# Server

## [Intel MKL, Math Kernel Library](https://software.intel.com/en-us/mkl/choose-download/linux)
* register and download .tar.gz file
* command line `sudo ./install.sh` or GUI `sudo ./install_GUI.sh`
* using sudo, will install systemwise at `/opt/intel/`

## check reboot
```bash
last | grep reboot
```

## change owner, both user and group
``` bash
sudo chown -R songhongwei:songhongwei /data/songhongwei
```

## Disk usage
```bash

#check general disk usage
df -hl

# check current path disk usage, list all children of `/data/`
sudo du --max-depth=1 -h /data/

# check disk usage of a folder
sudo du -sh /data/songhongwei/
```

## ssh from outside university
``` bash
# add in rc.local on server
sudo shellinaboxd --no-beep --disable-ssl --disable-ssl-menu --port=60004 -s /:SSH:192.168.11.214 &

# add a port forwarding in router

# user side
1. login to vpn.hit.edu.cn
2. http://202.118.228.98:6000X, where X from 1-5
```

## du's frp
``` bash
# add in rc.local
sudo /home/duzhihao/frp_0.25.3_linux_amd64/frpc -c /home/duzhihao/frp_0.25.3_linux_amd64/frpc.ini &
```

