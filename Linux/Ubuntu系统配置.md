

### 1 安装vim

```bash
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim
# 卸载ppa
sudo add-apt-repository --remove ppa:jonathonf/vim 
```

### 2 更新Ubuntu镜像源

> [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)

```bash
sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
sudo vim /etc/apt/sources.list
```

```
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
```

### 3. 安装pip3，修改pip3源
```bash
sudo apt install python3 python3-dev
```

```bash
mkdir ~/.pip
vim ~/.pip/pip.conf
```

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

```bash
sudo apt install python3-pip
```

### 4. 安装softwares

#### [搜狗拼音输入法](https://pinyin.sogou.com/linux/?r=pinyin)

#### [yandex浏览器](https://browser.yandex.com/)

#### [安装zsh、oh-my-zsh](https://blog.csdn.net/qq_14824885/article/details/81098091)

> 1. **安装zsh**
>
>    ```bash
>    sudo apt install zsh
>    ```
>
> 2. **将默认shell改为zsh**
>
>    ```bash
>    chsh -s /bin/zsh # 注意：不要使用sudo root用户
>    chsh -s /usr/bin/zsh # 本地当前用户
>    ```
>
> 3. **配置密码文件，解决chsh:PAM认证失败的问题**
>
>    ```bash
>    sudo vim /etc/passwd
>    ```
>
>    将第一行的/bin/bash改成/bin/zsh，这个是root用户
>
> 4. **安装Git**
>
>    ```bash
>    sudo apt install git
>    ```
>    **安装oh-my-zsh**
>
>    ```bash
>    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
>    ```
>
> 5. **软连接`zshrc`**
>
>    修改`zsh/zshrc`中`ZSH`路径,以免报错
>
>    ```bash
>    export ZSH="/home/xxxx/.oh-my-zsh"
>    ```
>
>    
>
>    ```bash
>    cd ~
>    rm .zshrc
>    ln -s ~/.comfig/zsh/wsl_zshcr /zshrc
>    ```
>
> 6. **安装zsh-syntax-highlighting语法高亮插件**
>
>    ```bash
>    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
>     echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
>    source ~/.zshrc
>    ```
>
> 7. **安装zsh-autosuggestions语法历史记录插件**
>
>    ```bash
>    git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
>    ```
>
>    添加到配置：
>
>    ```bash
>    vim ~/.zshrc
>    #搜索 plugins=
>    # 在括号中加上 
>    zsh-autosuggestions # 不同插件用空格隔开
>    # 在plugins 下加上 
>    setopt no_nomatch  # 如果不加的话就不能使用*
>    # 在最后一行 添加
>    source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
>    source ~/.zshrc
>    ```
>
> 8. ##### 安装z.lua 跳转目录插件
>
>    安装`lua`
>    ```bash
>    wget http://www.lua.org/ftp/lua-5.3.5.tar.gz
>    tar -zxvf lua-5.3.5.tar.gz
>    cd lua-5.3.5/
>    make linux test
>    ```
>    如果报错，应该是依赖不全
>    ```bash
>    sudo apt install libreadline7 libreadline-dev
>    make linux test
>    make install
>    ```
>    安装`z.lua`
>    ```bash
>    cd .config && git clone https://github.com/skywind3000/z.lua.git
>    ```
>    在`.zshrc` 中添加
>    ```
>    eval "$(lua ~/.config/z.lua/z.lua  --init zsh once enhanced)"
>    ```
>
> 9. **配置主题**
>
>    ```bash
>    sudo vim ~/.zshrc
>    # 找到ZSH_THEME=，修改=后面引号内的内容
>    ZSH_THEME="ys"
>    ```

#### 安装常用 app

> #### 安装nodejs
>
> 由于`nvim`插件`coc`需要`nodejs`，须先安装
>
> ```bash
> sudo apt install nodejs npm
> ```
>
> 如果`nodejs`，版本太低：
>
> ```bash
> sudo npm cache clean -f
> sudo npm install -g n
> sudo n stable
> ```
>
> 更改`npm`镜像源
>
> ```bash
> npm config set registry https://registry.npm.taobao.org
> ```
>
> 安装`cnpm`，使用其他镜像（例子为淘宝镜像）
>
> ```bash
> sudo npm install -g cnpm
> ```
>
> 安装`yarn`及前端脚手架工具
>
> ```bash
> sudo cnpm install -g yarn fis3
> ```
>
> **安装`neovim`**
>
> ```
> sudo pacman -S neovim
> ```
>
> 需要`python2`的支持
>
> ```
> sudo apt install python-pip
> python2 -m pip install --user --upgrade pynvim
> ```
>
> 需要`python3`的支持
>
> ```
> pip3 install --user pynvim
> ```
>
> 需要`npm,yarn`支持
>
> ```
> sudo npm install -g neovim
> yarn global add neovim
> ```
>
> nvim与系统之间的复制粘贴，需要`xsel`或`xclip`
>
> ```
> sudo apt install xclip
> ```
>
> 软链接`init.vim`
>
> ```bash
> cd ~/.comfig/nvim/
> ln init_wsl.vim init.vim
> ```
>
> **安装`ranger`**
>
> ```bash
> cd ~
> git clone https://github.com/ranger/ranger.git
> cd ranger
> sudo make install
> ```
>
> 复制`ranger_config`
>
> 然后软连接：
>
> ```bash
> cd ~/.config/ranger
> ln rc_wsl.conf rc.conf
> ln rifle_wsl.conf rifle.conf
> ```
>
> 预览图片还可用`ueberzug`
>
> ```bash
> pip3 install --user ueberzug
> ```
>
> 预览代码高亮需要`bat`,解压缩需要`atool`：
>
> `bat`在Ubuntu中不一定能通过`apt`下载,因此可以通过安装包`deb`安装,[下载地址](https://github.com/sharkdp/bat/releases)
>
> ```bash
> sudo dpkg -i bat_0.12.1_amd64.deb
> ```
>
> 安装`atool`
>
> ```bash
> sudo apt install atool
> ```
>
> 
>
> **安装其他app**
>
> ```bash
> sudo apt install neofetch
> ```
>
> 



#### 安装pycharm

> 官方下载[地址](https://www.jetbrains.com/pycharm/download/#section=windows)
>
> 1. **安装过程中需注意点：**
>
> > 勾选64-bit launcher
> >
> > 选择现在重启
>
> 2. **破解过程**
>
> > 复制 jjetbrains-agent.jar 到 **/bin/ 中
> > 修改 pycharm.exe.vmoptions 和 pycharm64.exe.vmoptions 最后一行加入
> > -javaagent:..../bin/jetbrains-agent.jar
> > // 如：-javaagent:D:\Program Files\JetBrains\PyCharm 2018.1\bin\jetbrains-agent.jar
> > License Activation 选择 License server
> > 填入： http://jetbrains-license-server
>
> 3. **添加桌面图标**
>
> > ```bash
> > cd /usr/share/applications
> > sudo gedit pycharm.desktop
> > ```
> >
> > ```
> > # 添加
> > [Desktop Entry]
> > Version=1.0
> > Type=Application
> > Name=Pycharm
> > Icon=/home/qiujl/softwares/pycharm/bin/pycharm.png
> > Exec=sh /home/qiujl/softwares/pycharm/bin/pycharm.sh
> > MimeType=application/x-py;
> > Name[en_US]=pycharm
> > ```
>
> 4. **将pycharm的terminal设置为docker容器内**
>
> > setting -> Tools -> Terminal
> >
> > Shell path 更改为
> >
> > "cmd.exe" /k ""C:\Program Files\Docker\Docker\resources\bin\docker-compose.exe" exec web bash"

#### 安装 docker、docker-compose

> **Ubuntu**
>
> >安装docker-ce
> >
> >```bash
> >sudo apt update
> >sudo apt install  apt-transport-https  ca-certificates curl  software-properties-common
> >curl -fsSL  https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
> >sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
> >sudo apt install update
> >sudo apt install docker-ce
> >```
> >
> >查看docker-compose 最新版本 [网址](https://github.com/docker/compose/releases)
> >
> >安装docker-compose
> >
> >```bash
> >sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
> >sudo chmod +x /usr/local/bin/docker-compose
> >docker-compose --version
> >```
> >
> >将当前用户 添加到docker用户组中
> >
> >```bash
> >sudo usermod -aG docker USER_NAME
> >sudo gpasswd -a ${USER} docker
> >```
> >
> >添加docker镜像
> >
> >```bash
> >sudo mkdir -p /etc/docker
> >sudo tee /etc/docker/daemon.json <<-'EOF'
> >{
> >  "registry-mirrors": ["https://ne7hlub7.mirror.aliyuncs.com"]
> >}
> >EOF
> >sudo systemctl daemon-reload 
> >sudo systemctl restart docker #自系统启动
> >```
>
> **Windows**
>
> > 创建hyperv.cmd
> > 添加
> >
> > ```cmd
> > pushd "%~dp0"
> > dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
> > for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
> > del hyper-v.txt
> > Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
> > ```
> >
> > 管理员身份执行 hyderv.cmd
> >
> > 重启
> >
> > 在控制面板-程序和功能 打开 Hyper-V
> >
> > 管理员身份运行cmd
> >
> > ```cmd
> > REG ADD "HKEY_LOCAL_MACHINE\software\Microsoft\Windows NT\CurrentVersion" /v EditionId /T REG_EXPAND_SZ /d Professional /F
> > ```
> >
> > 安装docker for windows
> >
> > 安装是注意取消勾选window容器（默认不会勾选）
> >
> > 如果是LTS环境中，打开Docker Desktop for windows 的 setting
> >
> > General 中，勾选  Expose daemon on tcp://localhost:2375 without TLS
> >
> > 在TLS中编辑
> >
> > ```bash
> > vim ~/.zshrc # 最后一行添加
> > export DOCKER_HOST='tcp://0.0.0.0:2375'
> > source ~/.zshrc
> > ```

#### 安装vim插件

> [查看相关网址](https://blog.csdn.net/lu_embedded/article/details/76732965)

#### 安装 chromium

> ```bash
> sudo apt install chromium-browser
> ```

#### 安装theFuck

> ```bash
> sudo pip3 install thefuck
> vim ~/.zshrc
> 最后一行加入
> eval $(thefuck --alias FUCK)
> source ~./zshrc
> ```

#### 安装nvm

> ```bash
> cd ~
> git clone https://github.com/nvm-sh/nvm.git .nvm
> cd ~/.vim
> . nvm.sh # 可能需要在bash
> vim ~/.zshrc
> # 最后添加：
> export NVM_DIR="$HOME/.nvm"
> [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
> [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
> # 然后：
> source ~/.zshrc
> # 另开一窗口验证
> nvm --version
> # 安装对应node
> nvm ls-remote
> nvm install v**
> ```

#### 安装yarn

> ```bash
> curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
> echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
> sudo apt update
> sudo apt install --no-install-recommends yarn
> ```

#### 安装shadowsocks

> ```bash
> sudo apt install python-pip
> pip install shadowsocks --user
> ```
>
> 添加ssr
>
> ```bash
> {
>     "server":"149.248.54.196",
>     "server_port":1003,
>     "local_address": "127.0.0.1",
>     "local_port":1080,
>     "password":"eofemerpie",
>     "timeout":10,
>     "method":"aes-256-cfb",
>     "remarks":""
> }
> ```
>
> ```bash
> sslocal -c ~/ss.json # 运行ssr
> ```
>
> 如果报错error:
>
> ```bash
> AttributeError: /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1: undefined symbol: EVP_CIPHER_CTX_cleanup
> ```
>
> ```bash
> vim /home/qiujl/.local/lib/python2.7/site-packages/shadowsocks/crypto/openssl.py
> # 将第52行
> libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,) 
> # 改为
> libcrypto.EVP_CIPHER_CTX_reset.argtypes = (c_void_p,) 
> # 再次搜索cleanup（全文件共2处，此处位于111行），
> # 将
> libcrypto.EVP_CIPHER_CTX_cleanup(self._ctx) 
> # 改为
> libcrypto.EVP_CIPHER_CTX_reset(self._ctx)
> ```

### 5. SSH公钥

> 优先使用自己的备份，记得
>
> ```bash
> sudo chmod 600 ~/.ssh/id_rsa
> ```
>
> ```bash
> # 生成SSH公钥
> ssh-keygen
> # 查看公约
> cat ~/.ssh/id_rea.pub
> ```

### 6. git相关配置

> git config 设置
>
> ```bash
> git config --global user.name 'qiujl'
> git config --global user.email 'qiuyue77@outlook.com'
> ```
>
> 如果出现 git status 不能正常显示中文，输入
>
> ````bash
> git config --global core.quotepath false 
> ````
>
> 不检查`filemode`,设置:
>
> ```bash
> git config --global core.filemode false
> ```
>
> 
