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

# stallion
https://manage.buyvm.net/login
b234abee3b06457a

# ssh server
ssh root@198.98.52.190
# passwd
604

# check ssserver running
ps aux |grep ssserver
# if not,run it
ssserver -c /etc/shadowsocks.json -d start
```
