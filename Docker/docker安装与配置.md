## 安装 docker、docker-compose

#### 1.Linux
**1.1 安装docker**

*Ubuntu*
```bash
sudo apt update
sudo apt install  apt-transport-https  ca-certificates curl  software-properties-common
curl -fsSL  https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt install update
sudo apt install docker-ce
```
*Manjaro*
```bash
sudo pacman -S docker
```

**1.2 安装docker-compose**

查看 [`docker-compose`最新版本](https://github.com/docker/compose/releases)

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

将当前用户 添加到docker用户组中

```bash
sudo usermod -aG docker USER_NAME
sudo gpasswd -a ${USER} docker
```

添加docker镜像

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ne7hlub7.mirror.aliyuncs.com"]
}
EOF
```
设置系统自启动

```bash
sudo systemctl daemon-reload 
sudo systemctl restart docker
```

**Windows**

 创建`hyperv.cmd`
 添加

 ```cmd
 pushd "%~dp0"
 dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
 for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
 del hyper-v.txt
 Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
 ```

 管理员身份执行 `hyderv.cmd`

 重启

 在控制面板-程序和功能 打开 `Hyper-V`

 管理员身份运行`cmd`

 ```cmd
 REG ADD "HKEY_LOCAL_MACHINE\software\Microsoft\Windows NT\CurrentVersion" /v EditionId /T REG_EXPAND_SZ /d Professional /F
 ```

 安装`ocker for windows`

 安装是注意取消勾选window容器（默认不会勾选）

 如果是`WSL`环境中，打开`Docker Desktop for windows` 的 `setting`

` General `中，勾选 ` Expose daemon on tcp://localhost:2375 without TLS`

 在`WSL`中编辑

 ```bash
 vim ~/.zshrc # 最后一行添加
 export DOCKER_HOST='tcp://0.0.0.0:2375'
 source ~/.zshrc
 ```
