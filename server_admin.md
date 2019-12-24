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
# add in /etc/rc.local file on server
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

## du's frp run on xshell
``` bash
# new a screen
screen -S du

# run on new screen
sudo /home/duzhihao/frp_0.25.3_linux_amd64/frpc -c /home/duzhihao/frp_0.25.3_linux_amd64/frpc.ini &
```

## Install xrdp and mate
1.安装mate
```
sudo apt-get update
sudo apt-get install mate-core mate-desktop-environment mate-notification-daemon	
```
2.安装xrdp
```
sudo apt-get install xrdp
```
3.配置xrdp默认使用mate桌面系统
```
sudo sed -i.bak '/fi/a #xrdp multiple users configuration \n mate-session \n' /etc/xrdp/startwm.sh
```

## [IPMI](https://www.ibm.com/developerworks/cn/linux/l-ipmi/index.html)
* Intelligent Platform Management Interface
* Talk to BMC(Baseboard Management Controller)
* `ipmitool` and `ipmiutil`

## `ldconfig`
* config dynamic linker run-time bindings
* search pathes
```
/etc/ld.so.conf.d/
```
* Find dynamic library 
```
# e.g. find where libcuda.so is installed
ldconfig -p | grep libcuda
```

## check server serial number
* get hardware infomation using `lshw`
```
sudo lshw -C system
```
```
213:
    product: NF5568M4 (Default string)
    vendor: Inspur
    version: 0123456789
    serial: 217336903

```

* [Inspur ipmitool](http://www.4008600011.com/archives/15141#2linux)
```
# install ipmitool
sudo apt-get install ipmitool

# install ipmi device driver, will show up as /dev/ipmi0
modprobe ipmi_msghandler
modprobe ipmi_devintf
modprobe ipmi_si
```

* [Inspur NF5568M4 BMC 设置](http://www.4008600011.com/archives/9942)
