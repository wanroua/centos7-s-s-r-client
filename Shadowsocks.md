####  <center>搭建</center>

```
yum install epel-release -y
yum -y groupinstall "development tools"
yum install gcc gettext autoconf libtool automake make pcre-devel asciidoc xmlto c-ares-devel libev-devel libsodium-devel mbedtls-devel  libsodium -y   #需更新本地yum源

vim /etc/yum.repos.d/epel.repo 
[librehat-shadowsocks]
name=Copr repo for shadowsocks owned by librehat
baseurl=https://copr-be.cloud.fedoraproject.org/results/librehat/shadowsocks/epel-7-$basearch/
type=rpm-md
skip_if_unavailable=True
gpgcheck=1
gpgkey=https://copr-be.cloud.fedoraproject.org/results/librehat/shadowsocks/pubkey.gpg
repo_gpgcheck=0
enabled=1
enabled_metadata=1

yum install shadowsocks-libev -y 




编辑启动脚本
/usr/lib/systemd/system/shadowsocks-libev.service
[Service]
Type=simple
EnvironmentFile=/etc/sysconfig/shadowsocks-libev
User=root
Group=root
LimitNOFILE=32768
ExecStart=/usr/bin/ss-server -c $CONFFILE $DAEMON_ARGS





编辑配置文件
/etc/shadowsocks-libev/config.json
{
    "server":"45.78.47.87",
    "server_port":443,
    "local_port":1080,
    "password":"malageBI0.0",
    "timeout":5,
    "method":"aes-256-cfb"
}
```

