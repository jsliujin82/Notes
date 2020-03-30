## V2ray 科学上网方式

### 一、服务器端

#### 1. 一键式

下载安装一键v2ray：

```bash
# Ubuntu
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_v2ray.sh)
# CentOS
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray.sh)
```

等待等待一段时间，上面的命令会安装一些依赖

成功后，输入想设置的端口号

确认后会出现如下内容，用于客户端

```
v2ray配置信息：
 IP(address):  45.76.45.182
 端口(port)：12580
 id(uuid)：2240e5dd-4a03-44cf-aa9f-bcbde65bdcb4
 额外id(alterid)： 74
 加密方式(security)： auto
 传输协议(network)： tcp
```

查看配置运行状态/参数的命令是：

```bash
# Ubuntu
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_v2ray.sh) info
# CentOS
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray.sh) info
```

更改端口、alterid最简单的办法是重新运行一次一键脚本

 卸载命令：

```bash
# Ubuntu
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_v2ray.sh) uninstall
# CentOS
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray.sh) uninstall
```

#### 2. 非一键式

一键安装`v2ray`

```bash
bash <(curl -L -s https://install.direct/go.sh)
```

终端会打印出端口号和UUID

```bash
# 查看端口 Port
cat /etc/v2ray/config.json | grep port
# 查看 id (UUID)
cat /etc/v2ray/config.json | grep id
# 查看额外id（alterId）
cat /etc/v2ray/config.json | grep alterId
```

开启`v2ray`

```bash
systemctl start v2ray
```

自启动或关闭自启动

```bash
sudo systemctl enable v2ray
sudo systemctl disable v2ray
```

#### 3.日志

```bash
sudo tail /var/log/v2ray/error.log # 如果设置了重定向了可查看
sudo journalctl -b -u v2ray
```



### 二、 客户端

#### 1. Windows

需要 [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472)  和 [Microsoft Visual C++ 2015 Redistributable (x86)](https://www.microsoft.com/en-us/download/details.aspx?id=53840) 

[下载地址](https://github.com/2dust/v2rayN/releases)

尽量不在C盘，会有权限情况

选择`V2RayN.exe`，启动后会在桌面右下方托盘出现图标，点击图标进入软件

点`服务器`下拉菜单中的`vmess服务器`

依次填写：**服务器地址（域名或ip）、端口、用户id、额外id、加密方式一般都是auto，传输协议一般是tcp。别名随便写就可以**

**如果使用了伪装等高级技术，伪装域名要填写配置的主机名，输入伪装路径，同时底层传输安全要选择tls，allowinsecure选择true（没使用伪装不要选！）**

点击主界面上的`参数设置`，在`Core:基础设置`中将`Http代理`，尽量选择`开启PAC，并自动配置PAC（PAC模式）`

设置完成

#### 2. Android

[下载地址](https://github.com/2dust/v2rayNG/releases)

新安装app，要记得设置：

> `设置`，选择`分应用代理`，然后选择需要代理的app
>
> `路由设置`，`预定义规则`选择`绕过局域网及大陆地址`

#### 3. Linux

##### 3.1 非GUI端

**Ubuntu**

[非客户端下载地址](https://github.com/v2ray/v2ray-core/releases) 

```bash
sudo bash <(curl -L -s https://install.direct/go.sh) --local ./v2ray-linux-64.zip
```

配置文件`/etc/v2ray/config.json`，可复制

```bash
sudo vim /etc/v2ray/config.json
```

使用

```bash
sudo service v2ray restart
```



**Manjaro**

```
yay -S v2ray
```

配置文件`/etc/v2ray/config.json`，可复制

```bash
sudo vim /etc/v2ray/config.json
```
写完以后要把`vpoint_vmess_freedom.json`删除

添加日志路径

```
sudo mkdir /var/log/v2ray
```

然后就可以通过 `systemctl` 来控制 v2ray 的运行了。

```bash
systemctl start v2ray
```

```
systemctl restart v2ray
```

```
systemctl stop v2ray
```

```
systemctl status v2ray
```

终端里面也要上网

```
yay -S proxychains-ng
sudo vim /etc/proxychains.conf
```

文件末尾添加：

```
socks5 127.0.0.1 1080
```

然后，大多数的终端软件就可以上网了，例如：

```bash
proxychains curl www.google.com
```

**日志**

```bash
sudo tail /var/log/v2ray/error.log # 如果设置了重定向了可查看
```

**PAC代理**

PAC代理方面，不同的Linux桌面环境会有区别，可以自行搜索关键词：设置pac “系统名+版本号”。这里以Ubuntu18.04作为示范：

打开设置—Network—Network Proxy，选择Automatic模式，填入file://pac文件绝对路径，如果你的pac配置文件放到了/etc/v2ray/proxy.pac，那么就填入:file:///etc/v2ray/proxy.pac

##### 3.2 GUi端

[GUI客户端项目](https://github.com/jiangxufeng/v2rayL)

在自己的电脑上重新打包程序，具体方法如下（参考）

1. 运行`git clone https://github.com/jiangxufeng/v2rayL.git`
2. 进入项目文件夹，然后运行`pip install -r requirements.txt`
3. 运行`cd v2rayL-GUI && pyinstaller -F v2rayLui.py -p config.py -p sub2conf_api.py -p v2rayL_api.py -p v2rayL_threads.py -p utils.py -i images/logo.ico -n v2rayLui`
4. 打包后运行`mv dist/v2rayLui /usr/bin/v2rayL/v2rayLui` 替换安装时下载的程序

安装

```bash
bash <(curl -s -L http://dl.thinker.ink/install.sh)
```

更新

```bash
bash <(curl -s -L http://dl.thinker.ink/update.sh)
```

删除

```bash
bash <(curl -s -L http://dl.thinker.ink/uninstall.sh)
```

程序被安装至`/usr/bin/v2rayL/v2rayLui`

### 三、浏览器相关

先安装插件`SwitchOmege_Chromium.crx`

可以直接导入`OmegaOptions.bak`和`proxy.pac`

如果没有上述文件，则：

`SwichyOmega`设置中，`auto switch`中，在规则网站填这个：

```tzt
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
```























