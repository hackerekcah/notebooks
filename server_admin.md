# Server
## Server fix log
### 
* 211 电源坏，表现：电源灯不亮，性能下降。解决：热插拔，更换。
* 213 内存坏，启动自检时报内存错，显示具体内存卡槽位置。
  * 关机，打开机箱（机箱后有两个小螺丝，卸掉即可），找到对应内存卡槽，更换重启即可。
* 213 硬盘slot5坏
  * 表现：机器一直报警：嘟-嘟-...,确认方法：在RAID界面，按ctrl+R，PD Mgmt菜单，显示slot5 硬盘Failed. 其他硬盘online。
  * 解决：热插拔硬盘，重启，按ctrl+R，更换硬盘，看到硬盘状态由failed变为Rebuild，具体可看到progress百分比，一般Rebuild需要两天。

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

## [IPMI Introduction by IBM](https://www.ibm.com/developerworks/cn/linux/l-ipmi/index.html)
* Intelligent Platform Management Interface
* Talk to a hardware called BMC(Baseboard Management Controller)
* [`ipmitool`](https://github.com/ipmitool/ipmitool)
* [`ipmiutil`](http://ipmiutil.sourceforge.net/)

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
* 微信公众号报修：浪潮专家服务

## [RAID](https://www.cnblogs.com/oneasdf/p/9367152.html)
* [知乎RAID帖](https://www.zhihu.com/question/20131784)
* [浪潮RAID配置](http://www.4008600011.com/archives/429?jdfwkey=mvahm3)
* server RAID:
```
slot0, SSD-SATA, 447G,
slot1-5, SATA, 1.8T
```

## [SCSI wikipedia](https://en.wikipedia.org/wiki/SCSI)
* Small Computer System Interface, a standards for physically connecting computer to peripherials
* A physical implementation of SCSI is SAS(Serial attached SCSI), common used as hard disk connection

## SAS vs SATA
* SATA, cheap, for personal computers, power and data use seperate lines
* SAS, expensive, for servers, more reliable, power and data in one single line

