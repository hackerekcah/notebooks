# clash
* Local HTTP/HTTPS/SOCKS server
* [github](https://github.com/Dreamacro/clash)
  * see config examples
  * download cinfig file and put it in ```~/.config/clash/config.yaml```
* [release version](https://github.com/Dreamacro/clash/releases)
  * decompress downloaded file using ```gunzip -d clash.gz```
* `config.yaml`
  * control clash using web api [api](https://clash.gitbook.io/doc/restful-api/common)
```
# control port
external-controller: 127.0.0.1:9090

# examples
http://127.0.0.1:9090/proxies
http://127.0.0.1:9090/rules
http://127.0.0.1:9090/configs
```
# Using VPN for python script
* add these line, and open VNP to serve the port
``` python
os.environ['http_proxy'] = 'http://127.0.0.1:7890'
os.environ['https_proxy'] = 'http://127.0.0.1:7890'
```
