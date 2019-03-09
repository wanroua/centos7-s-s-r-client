`centos-7下安装shadowsocks-client`

```bash
1.安装pip #仅支持python2
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
python  get-pip.py
#安装Pip
pip install  --upgrade  pip
pip install shadowsocks
#安装shadowsocks

2.设置配置文件
mkdir /etc/shadowsocks
vim /etc/shadowsocks/shadowsocks.json
{
    "server":"x.x.x.x",  # Shadowsocks服务器地址
    "server_port":1035,  # Shadowsocks服务器端口
    "local_address": "127.0.0.1", # 本地IP
    "local_port":1080,  # 本地端口
    "password":"password", # Shadowsocks连接密码
    "timeout":300,  # 等待超时时间
    "method":"aes-256-cfb",  # 加密方式
    "fast_open": false,  # true或false。开启fast_open以降低延迟，但要求Linux内核在3.7+
    "workers": 1  #工作线程数 
}

3.配置启动脚本
vim /usr/lib/systemd/system/shadowsocks.service
[Unit]
Description=Shadowsocks
[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/shadowsocks.json
[Install]
WantedBy=multi-user.target

4.启动shadowsocks
systemctl  enable  shadowsocks.service
#设置开机启动
systemctl  start   shadowsocks.service
#启动

curl --socks 127.0.0.1:1234  http://httpbin.org/ip
{
  "origin": "x.x.x.x"       #你的Shadowsock服务器IP
}
#测试是否成功，不知道为什么显示301
```

`安装privoxy`

```bash
yum -y install https://mirrors.aliyun.com/epel/7Server/x86_64/Packages/p/privoxy-3.0.26-1.el7.x86_64.rpm

systemctl enable privoxy
systemctl start privoxy
systemctl status privoxy

修改配置文件
vim /etc/privoxy/config
forward-socks5t / 127.0.0.1:1080 . #转发到本地端口，注意最后有个点

设置http、https代理
vim 、/etc/profile
PROXY_HOST=127.0.0.1
export all_proxy=http://$PROXY_HOST:8118
export ftp_proxy=http://$PROXY_HOST:8118
export http_proxy=http://$PROXY_HOST:8118
export https_proxy=http://$PROXY_HOST:8118
export no_proxy=localhost,172.16.0.0/16,192.168.0.0/16.,127.0.0.1,10.10.0.0/16

# 重载环境变量
source /etc/profile
测试：
curl -I www.google.com

```



