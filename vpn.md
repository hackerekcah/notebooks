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
* specify config
```
# by default is ~/.config/clash/configl.yaml
./clash -d conf_dir
```
# Using VPN for python script
* add these line, and open VNP to serve the port
``` python
os.environ['http_proxy'] = 'http://127.0.0.1:7890'
os.environ['https_proxy'] = 'http://127.0.0.1:7890'
```

# V2ray
* [v2ray-core](https://github.com/v2ray/v2ray-core)
* [Project V, v2ray.com](https://www.v2ray.com/)
  * [basics](https://www.v2ray.com/chapter_00/workflow.html#internals)
  * [for newbies](https://www.v2ray.com/chapter_00/start.html)
  *  V2Ray 可同时开启多个协议支持，包括 Socks、HTTP、Shadowsocks、VMess 等。每个协议可单独设置传输载体，比如 TCP、mKCP、WebSocket 等。
    * vmess: a encryption protocal
* [v2Box.cloud](v2box.cloud)
  * [vps provider]
  ```
  https://sub.paasmi.com/subscribe/85070/NqP4ZLGJN8XO?mode=
  ```
  * what missed? 3 at last

* client
  * [v2rayN](https://github.com/2dust/v2rayN)
    * windows v2ray client
  * [qv2ray](https://github.com/Qv2ray/Qv2ray)
    * see [doc](https://qv2ray.github.io/en/getting-started/)
    * [See config v2ray core](https://qv2ray.github.io/en/getting-started/step2.html#download-v2ray-core-files)
    * core excutable file: /home/songhongwei/.config/qv2ray/vcore/v2ray
    * assets directory: /home/songhongwei/.config/qv2ray/vcore/
    * socks 7891, https 7890
