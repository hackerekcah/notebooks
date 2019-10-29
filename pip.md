
## pypi 清华mirror
``` python
# tstinghua pypi index mirror
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

# set as default index
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
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

## pip install specific pakcage
```
# --upgrade works with upgrade and downgrade
pip install --upgrade werkzeug==0.12.2
```
