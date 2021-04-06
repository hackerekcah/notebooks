
###  实验室外网如何访问服务器？
* **非校园网环境**下载 [easyconnect](http://static.hit.edu.cn/vpn/) 连接到学校的 vpn
  * URL 填写: https://ivpn.hit.edu.cn
 
* 通过 easyconnect 连接到学校 vpn, 或在校园里连接了 HIT-wlan 后，使用域名 `ddns.hitsplab.xyz` 登录远程桌面或 ssh
 
    ```bash
    # 远程桌面
    ddns.hitsplab.xyz:33894

    # ssh
    ssh -p 22214 shw@ddns.hitsplab.xyz
    ```
 ## Home directory is set to
 * put all your data and code under this directory
 ``` bash
 /data/shw
 ```
