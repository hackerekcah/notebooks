# Local
```
sslocal -c /etc/shadowsocks.json -d start

# /etc/shadowsocks.json
{
"server":"205.185.117.151",
"server_port":40001,
"local_address": "192.168.11.211",
"local_port":7070,
"password":"password1",
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
    "server":"205.185.117.151",
    "server_port":40001,
    "local_port":40001,
    "password":"password1",
    "timeout":600,
    "method":"aes-256-cfb"
}

# stallion
https://manage.buyvm.net/login
b234abee3b06457a

# ssh server
ssh root@205.185.117.151
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
https://s3.amazonaws.com/outline-vpn/invite.html?admin_embed#/zh-CN/invite/ss%3A%2F%2FY2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpaR0s2RVhvbmZkaUM%3D%40205.185.117.151%3A20162%2F%3Foutline%3D1
```

# Wlan
```
202.118.228.98
255.255.255.0
202.118.228.254

#DNS
202.118.224.100
202.118.224.101
```

# MAC
```
00-E0-4C-3C-E4-0E
```

# PPPOE
```
zhl60401
zhl60801
```
