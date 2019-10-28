
## pypi 清华mirror
``` bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

## pip using proxy
* in `/etc/proxychains.conf`
```
# uncomment these lines
strict_chain
proxy_dns
tcp_read_time_out 15000
tcp_connect_time_out 8000
```
* add in `/etc/proxychains.conf` section `[proxy_list]`
```
socks5  192.168.11.211 7070
```
* command
```
proxychains pip install some-package
```
