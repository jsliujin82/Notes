
### 4.ssh 进入购买的服务器

```bash
ssh root@XXX.XXX.XXX.XXX   # 这里XXX代表你服务器的ip地址
```

> [Ubuntu 16.04下Shadowsocks服务器端安装及优化](https://www.polarxiong.com/archives/Ubuntu-16-04%E4%B8%8BShadowsocks%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E5%AE%89%E8%A3%85%E5%8F%8A%E4%BC%98%E5%8C%96.html)

### 5.安装setuptools
```bash
apt install python3-pip
pip3 install --upgrade setuptools
pip3 install git+https://github.com/shadowsocks/shadowsocks.git@master
```

### 6. 新建一个文件 shadowsocks.json 并添加如下内容

> 这个用于自定义配置文件允许多用户使用
>
> 注意port_password里面是每个端口对应的密码,重要
>
> _comment里面是每个端口的备注，不重要

```bash
sudo vim /etc/shadowsocks.json
```

```json
{
    "server":"0.0.0.0",
    "server_ipv6":"[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "timeout":300,
    "method":"aes-256-cfb",
    "port_password":
        {
            "80": "!t78Ttpb",
            "1001": "P!6j%3Dn",
            "1002": "5wOhq$Go",
            "1003": "UpGpHiN4",
            "1004": "9Jx9uDZr",
            "1005": "Nqq32Uqw",
            "1006": "KgTP&My5",
            "1007": "w#Sr7KWv",
            "1008": "lXKyc1d5",
            "1009": "OWUBu4%8"
        },
    "_comment":
        {
            "80": "commentof80",
            "1001": "commentof1001",
            "1002": "commentof1002",
            "1003": "commentof1003",
            "1004": "commentof1004",
            "1005": "commentof1005",
            "1006": "commentof1006",
            "1007": "commentof1007",
            "1008": "commentof1008",
            "1009": "commentof1009"
        },
    "protocol": "",
    "protocol_param": "",
    "obfs": "",
    "obfs_param": "",
    "redirect": "", 
    "dns_ipv6": false,
    "fast_open": false
} 
```

> [shadowsocks.json 配置文件各项说明](https://github.com/ssrarchive/shadowsocks-rss/wiki/config.json)

### 7.1  启动、关闭服务、

```bash
ssserver -c /etc/shadowsocks.json -d start   # 开启服务
ssserver -c /etc/shadowsocks.json -d restart # 重启服务
ssserver -c /etc/shadowsocks.json -d stop    # 停止服务
```

### 7.2 设置自动启动

```bash
sudo vim /etc/systemd/system/shadowsocks-server.service

[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks.json
Restart=on-abort

[Install]
WantedBy=multi-user.target

#启动Shadowsocks：
sudo systemctl start shadowsocks-server
#设置开机启动Shadowsocks：
sudo systemctl enable shadowsocks-server
```

### 8.1 查看日志

```bash
sudo tail -f /var/log/shadowsocks.log
```

### 9 防火墙

```bash
sudo apt install firewalld
sudo vim /etc/firewalld/zones/public.xml
```

```xml
<?xml version="1.0" encoding="utf-8"?> <zone>
    <short>Public</short>
    <description>Shadowsocks port</description>
    <service name="dhcpv6-client"/>
    <service name="ssh"/>
    <port protocol="udp" port="80"/>
    <port protocol="tcp" port="80"/>
    <port protocol="udp" port="1001"/>
    <port protocol="tcp" port="1001"/>
    <port protocol="udp" port="1002"/>
    <port protocol="tcp" port="1002"/>
    <port protocol="udp" port="1003"/>
    <port protocol="tcp" port="1003"/>
    <port protocol="tcp" port="1004"/>
    <port protocol="udp" port="1004"/>
    <port protocol="tcp" port="1005"/>
    <port protocol="udp" port="1005"/>
    <port protocol="tcp" port="1006"/>
    <port protocol="udp" port="1006"/>
    <port protocol="tcp" port="1007"/>
    <port protocol="udp" port="1007"/>
    <port protocol="tcp" port="1008"/>
    <port protocol="udp" port="1008"/>
    <port protocol="tcp" port="1009"/>
    <port protocol="udp" port="1009"/>
</zone>
```

```bash
systemctl stop firewalld	# 停止防火墙
systemctl start firewalld	# 开启防火墙
systemctl status firewalld.service # 状态
```

查看端口打开情况

```bash
netstat -aptn
```



### 10 客户端

> 以Ubuntu为例

```bash
sudo apt install shadowsocks
sslocal -c local.json
```

> local.json 是配置文件内容如下:

```json
{
	"server": "xxx.xxx.xxx.xxx",
	"server_port": 1001, 
	"password": "Password",
	"method": "aes-256-cfb",
	"remarks": ""
}
```

> 注80端口不容易被封，用来救急用，平时不用，如果连80端口也被封了，可能需要换台服务器了



### 11 优化

#### 开启BBR

运行`lsmod | grep bbr`，如果结果中没有`tcp_bbr`，则先运行

```bash
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

运行

```bash
sysctl -p # 保存生效
```

运行

```bash
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
```

若均有`bbr`，则开启BBR成功。

#### 优化吞吐量

新建配置文件

```bash
sudo vim /etc/sysctl.d/local.conf
```

复制粘贴

```bash
# max open files
fs.file-max = 51200
# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
net.ipv4.tcp_max_tw_buckets = 5000
# turn on TCP Fast Open on both client and server side
net.ipv4.tcp_fastopen = 3
# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1

net.ipv4.tcp_congestion_control = bbr
```

运行

```bash
sysctl --system
```

再编辑之前的`shandowsocks-server.service` 文件

```bash
sudo vim /etc/systemd/system/shadowsocks-server.service
```

在`ExecStart`前插入一行：

```
ExecStartPre=/bin/sh -c 'ulimit -n 51200'
```

修改后的`shandowsocks-server.service`的内容应该是

```
[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStartPre=/bin/sh -c 'ulimit -n 51200'
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/config.json
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

重载`shadowsocks-server.service`：

```bash
sudo systemctl daemon-reload
```

重启`Shandowsocks`

```bash
sudo systemctl restart shadowsocks-server
```

#### 开启TCP Fast Open

`TCP Fast Open`可以降低`Shadowsocks`服务器和客户端的延迟。实际上在上一步已经开启了`TCP Fast Open`，现在只需要在`Shadowsocks`配置中启用`TCP Fast Open`。

编辑`shadowsocks.json`

```bash
sudo vim /etc/shadowsocks.json
```

将`fast_open`的值由`false`修改为`ture`。保存

重启`Shadowsocks`

```bash
sudo systemctl restart shadowsocks-server
```

