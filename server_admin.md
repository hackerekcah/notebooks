# Server
## wakeonlan
```
211 ac:1f:6b:21:d4:da
212 ac:1f:6b:21:d4:d0
213 ac:1f:6b:21:d4:dc
214 ac:1f:6b:21:d4:de
215 ac:1f:6b:21:d1:cc
216 ac:1f:6b:21:d4:ce
```
## softlink a lot files into one direction
```
# if too many files under onedir, then it will reprot long argument error
ln -s onedir/* another_dir/

# use this instead
for i in unbalanced_train_segments/audio/*;do ln -s $i train_all_symbolic/$(basename $i);done
```

## xrdp connect to existing session
```
ps aux |grep Xvnc

# output, process id is 6838
songhon+  6838  0.6  0.0 179508 116956 ?       S    Jun12 171:54 Xvnc :10 -geometry 1920x1080 -depth 24 -rfbauth /home/songhongwei/.vnc/sesman_songhongwei_passwd -bs -ac -nolisten tcp -localhost -dpi 96

# the first line have 6838/Xvnc, so port is 5910
songhongwei@node04:~$ sudo netstat -putan |grep Xvnc
tcp        0      0 127.0.0.1:5910          0.0.0.0:*               LISTEN      6838/Xvnc       
tcp        0      0 127.0.0.1:5911          0.0.0.0:*               LISTEN      31073/Xvnc      
tcp        0      0 127.0.0.1:5912          0.0.0.0:*               LISTEN      22962/Xvnc      
tcp        0      0 127.0.0.1:5910          127.0.0.1:49408         ESTABLISHED 6838/Xvnc    
```
## mount
* 例如在 216，安装 `sshfs`
  ```bash
  sudo apt-get install sshfs
  ```
  
* Create empty path for mounting
  ```bash
  sudo mkdir /data/songhongwei/proj/fairseq
  sudo mkdir /data/songhongwei/ESC-50/audio
  ```
  
* 查看 `uid`
  ```bash
  cat /etc/passwd | grep songhongwei
  ```

* 远程挂载
```bash
211 1008
212 1007
215 1010
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/proj/fairseq/ /data/songhongwei/proj/fairseq/
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/proj/hcpc /data/songhongwei/proj/hcpc/
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/ESC-50/audio/ /data/songhongwei/ESC-50/audio/
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/dcase2018_baseline /data/songhongwei/dcase2018_baseline
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/UrbanSound8K /data/songhongwei/UrbanSound8K
sudo sshfs -o allow_other,uid=1010,gid=1010 songhongwei@192.168.11.214:/data/songhongwei/genres /data/songhongwei/genres
# 查看参数帮助
sshfs -h
```
  
* 取消挂载
  ```bash
  sudo umount /data/songhongwei/proj/fairseq
  
  # sometimes fail because some file opened / used, check using list open file command. `+D` means recursive
  lsof +D /data/songhongwei/proj/fairseq
  ```
## Server fix log
### 
* 211 电源坏
  * 表现：电源灯不亮，性能下降。
  * 解决：热插拔，更换。
* 213 内存坏
  * 表现：启动自检时报内存错，但不影响正常开机。文字显示具体损坏的内存卡槽位置。
  * 关机，打开机箱（机箱后有两个小螺丝，卸掉即可），找到对应内存卡槽，更换重启即可。
* 213 硬盘slot5坏
  * 表现：机器一直报警：嘟-嘟-...。确认方法：在RAID界面，按ctrl+R，PD Mgmt菜单，显示slot5 硬盘Failed. 其他硬盘online。
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

## Install Matlab R2019b
* 单机多用户安装，需要向网络中心发邮件阐明：
  * 1主机系统
  * 2主机mac地址，作为hostid。
* 网络中心会提供:
  * 对应的license.lic文件
  * File Installatin Key (FIK)
* 查看本机mac地址
```
# HWaddr field of eno1 is mac address
sudo ifconfig -a
```
* 安装lsb库，否则network license server无法启动
```
sudo apt-get install lsb
```
* 挂载iso文件
```
sudo mkdir /mnt/iso

sudo mount -t auto -o loop ./R2019b_Linux.iso /mnt/iso
```
* 安装network license server
```
# 进入安装界面，输入license.lic的路径和FIK，选择只安装Network License Server
sudo /mnt/iso/install

# 手动启动license manager
/usr/local/MATLAB/R2019b/etc/lmstart

# 为保证license manager开机自动启动，在rc.local中加入如下一行
# 注意，这里用任意用户即可，但不可使用root运行lmstart
su songhongwei -c "/usr/local/MATLAB/R2019b/lmstart"
```
* 此时，会产生`/usr/local/MATLAB/R2019b/etc/licence.dat`文件，文件中的前两行如。所有使用此License Server的client均需使用此.dat文件作为license。
```
# SERVER 主机名 MAC地址，端口号
SERVER node04 AC1F6B21D4DE 27000
DAEMON MLM "/usr/local/MATLAB/R2019b/etc/MLM"
```
* 安装matlab client
```
# 输入FIK和/usr/local/MATLAB/R2019b/etc/licence.dat文件
# 选择安装除Network License Server之外的所有产品。
sudo /mnt/iso/install
```
* 查看是否安装成功
```
/usr/local/MATLAB/R2019b/bin/matlab
```
* 安装教程
* [install concurrent matlab](https://cn.mathworks.com/matlabcentral/answers/152585-concurrent-matlab-8-1-r2013a)

## Matlab license manager 管理工具
* 管理License的工具书
  * `/usr/local/MATLAB/R2019b/etc/LicenseAdministration.pdf`
* `/usr/local/MATLAB/R2019b/etc/glnxa64/lmgrd`
```
./lmgrd -c <PATH_TO_LICENSE_FILE> -l <PATH_TO_LOG_FILE>
```
* 若输入matlab指令，显示`unable to connect to license server`错误，一般是license server未启动，运行
```
/usr/local/MATLAB/R2019b/etc/lmstart
```

## check cpu info
* `lscpu`
```
songhongwei@node04:~$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                32
On-line CPU(s) list:   0-31
Thread(s) per core:    2
Core(s) per socket:    8
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Model name:            Intel(R) Xeon(R) CPU E5-2667 v4 @ 3.20GHz
Stepping:              1
CPU MHz:               1200.750
CPU max MHz:           3600.0000
CPU min MHz:           1200.0000
BogoMIPS:              6401.48
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              25600K
NUMA node0 CPU(s):     0-7,16-23
NUMA node1 CPU(s):     8-15,24-31
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb intel_pt tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm rdseed adx smap xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm arat pln pts
```
* `less /proc/cpuinfo`
  * list details about individual cpu cores
  * will get 32 processor page in our server
```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 79
model name      : Intel(R) Xeon(R) CPU E5-2667 v4 @ 3.20GHz
stepping        : 1
microcode       : 0xb000010
cpu MHz         : 1228.500
cache size      : 25600 KB
physical id     : 0
siblings        : 16
core id         : 0
cpu cores       : 8
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 20
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb intel_pt tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm rdseed adx smap xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm arat pln pts
bugs            :
bogomips        : 6399.84
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

processor       : 1
...(similar page for each processor!!!)
```
* calculate cpu FLOPS
  * Formula: 
```
  FLOPS = sockets * (cores per socket) * (number of clock cycles per second) * (number of floating point operations per cycle).
  FLOPS = 2 * 8 * 2(threads per cores) * 3.2GHz * 
```
  * intel website
  * [Export Compliance Metrics for Intel® Microprocessors](https://www.intel.com/content/www/us/en/support/articles/000005755/processors.html)
  * [Export Compliance Metrics, Page 2](https://www.intel.com/content/dam/support/us/en/documents/processors/APP-for-Intel-Xeon-Processors.pdf)
```
GFLOPS 409.6

# we have two chips, so 
GFLOPS 409.6 * 2 = 819.2 GFLOPS = 0.8192 TFLOPS
```
* GFLOPS vs TFLOPS
```
Name	Unit	Value
kiloFLOPS	kFLOPS	10^3
megaFLOPS	MFLOPS	10^6
gigaFLOPS	GFLOPS	10^9
teraFLOPS	TFLOPS	10^12
petaFLOPS	PFLOPS	10^15
```
## memory
* check total memory
```
> grep MemTotal /proc/meminfo
MemTotal:       528269840 kB = 512 GB
```
## GPU
* [Tesla K80 main page](https://www.nvidia.com/en-gb/data-center/tesla-k80/)
* [Tesla K80 datasheet](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/TeslaK80-datasheet.pdf)
```
Peak double-precision floating point performance (board)   1.87 Tflops

# we have 3 Tesla K80, so
1.87 * 3 = 5.61 TFLOPS
```
