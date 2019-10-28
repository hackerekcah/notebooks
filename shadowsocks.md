# Local
```
sslocal -c /etc/shadowsocks.json -d start

# /etc/shadowsocks.json
{
"server":"198.98.62.68",
"server_port":40004,
"local_address": "192.168.11.211",
"local_port":7070,
"password":"password4",
"timeout":600,
"method":"aes-256-cfb"
}

```

# Server
``` bash
# in etc/rc.local
ssserver -c /etc/shadowsocks.json -d start

# new config file: /etc/shadowsocks.json
{
  "timeout": 600,
  "method": "aes-256-csb",
  "port_password":
  {
    "40001": "password1",
    "40002": "password2",
    "40003": "password3"
  },
  "_comment":
  {
    "40001": "xiaoming",
    "40002": "lilei",
    "40003": "mike"
  }
}

# add in /etc/hosts on shadowsocks server, to prevent google robot detection, as in https://www.skyfly.xyz/2017/08/02/Casual/SolutionForGoogleRecaptcha/

2607:f8b0:4005:801::200e google.com
2607:f8b0:4005:801::200e www.google.com
2404:6800:4005:807::2003 google.com.hk
2404:6800:4005:807::2003 www.google.com.hk
2404:6800:4005:807::2003 google.com.tw
2404:6800:4005:807::2003 www.google.com.tw

# stallion
https://manage.buyvm.net/login
b234abee3b06457a

# ssh server
ssh root@198.98.62.68
# passwd
604

# check ssserver running
ps aux |grep ssserver
# if not,run it
ssserver -c /etc/shadowsocks.json -d start
# start ssserver with a watcher program
nohup check_ssserver.sh >/dev/null 2>&1

# if any problem
https://buyvm.net/ -> client area

```

* not safe way
```
# on server, any 443 port pack send to self, 443 is the https port
ssh -D 0.0.0.0:443 root@127.0.0.1

# on local
set proxy, socks5, port 443
```

# for iphone
* install `outline manager` on windows, run code from the last setup on remote server.
    * to get the share-able link
* install `outline` client for iphone from the ios app store
* share this link
```
https://s3.amazonaws.com/outline-vpn/invite.html?admin_embed#/zh-CN/invite/ss%3A%2F%2FY2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpuTUN2dVp6VEptVUE%3D%40198.98.62.68%3A48900%2F%3Foutline%3D1
```
