# clash
* Local HTTP/HTTPS/SOCKS server
* [github](https://github.com/Dreamacro/clash)
  * see config examples
  * download cinfig file and put it in ```~/.config/clash/config.yaml```
* [release version](https://github.com/Dreamacro/clash/releases)
  * decompress downloaded file using ```gunzip -d clash.gz```

# Using VPN for python script
* add these line
``` python
os.environ['http_proxy'] = 'http://127.0.0.1:7890'
os.environ['https_proxy'] = 'http://127.0.0.1:7890'
```
