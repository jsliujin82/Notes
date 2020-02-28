Windows Terminal 设置见[***网页***](https://www.jianshu.com/p/31bf9f9c0fb1)

## WSL 安装

### win10设置

1. 在“设置”-“更新与安全”-“开发者选项”选项中，启用开发人员模式。
2. 在“控制面板”-“程序”-“启用或关闭`Windows`功能”，确认“`Windows Subsystem for Linux（Beta）`”功能为勾选状态。

### WSL 安装

“应用商店”中搜索“Ubuntu”，安装“Ubuntu-18.04”，安装结束后

在`Windows Terminal`中设置并打开

### WSL卸载

WSL的卸载操作如下：

```bash
wslconfig /l
# 从列表中选择要卸载的发行版（例如Ubuntu）并键入命令
wslconfig /u Ubuntu
```

### WSL 设置

1. #### 修改`apt`源:

   ```bash
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
   sudo vim /etc/apt/sources.list
   ```

   ```bash
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   ```

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

   

2. #### 安装`python3.8`,修改`pip`源:

   1. 安装`python`

      ```bash
      sudo apt install python3.8
      sudo apt install python3.8-dev python3.8-venv
      ```

      设置默认`python3`为`pyhton3.8`

      ```bash
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 2
      
      sudo update-alternatives --config python3
      # 选择python3.8
      ```

      ---

      **报错处理**

      - 如果**报错**`ModuleNotFoundError: No module named 'apt_pkg'`

      ```bash
      sudo apt install --reinstall python3-apt
      sudo cp apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.so
      ```

      - 如果**报错**`ImportError: cannot import name '_gi' from partially initialized module 'gi'`

      则修改`/usr/bin/add-apt-repository`的`#!/usr/bin/python3`为`#!/usr/bin/python3.6`

      

      ---

      `python3.8`相关脚本安装在`/home/jqiu/.local/bin`中,所以需要将其放到`$PATH`中

      ```bash
      sudo vim /etc/profile
      ```

      在最后一行添加:

      ```bash
      export PATH="/home/jqiu/.local/bin:$PATH"
      ```

      以下命令检查`pip`,`ipython3`是否正常:

      ```bash
      pip3 -V
      ipython3 # 如果显示不存在,则下执行第2项,再执行下方第3项
      ```

      

   2. 修改pip源

      ```bash
      mkdir ~/.pip && vim ~/.pip/pip.conf
      ```

      ```bash
      [global]
      index-url = https://pypi.tuna.tsinghua.edu.cn/simple
      trusted-host = pypi.tuna.tsinghua.edu.cn
      ```

      安装`pip`

      ```bash
   sudo apt install python3-pip
      ```
      
      更新`pip`
      
      ```bash
      pip3 install --upgrade pip --user
      ```
      
      

   3. 安装`ipython3`

      ```bash
      pip3 install --upgrade ipython --user
      ```

      

3. #### SSH公钥

   复制自己的`.ssh`,然后更改其中的执行权限:

   ```bash
   cp -r xxx/.ssh/ ~/
   sudo chmod 600 ~/.ssh/id_rsa
   ```

   

4. #### Git 

   1. `git`设置

      `git config` 设置

      ```bash
      git config --global user.name 'qiujl_wsl'
      git config --global user.email 'qiuyue77@outlook.com'
      ```

      

      #### 报错

      1. 如果出现 `git status `不能正常显示中文，输入

         ```bash
         git config --global core.quotepath false 
         ```

         

      2. 如果出现不同系统对换行的识别不到位导致的错误

         ```bash
         git config --global core.autocrlf false
         ```

         

   2. `git clone` 自己定义的`config`文件

      ```bash
      git clone git@github.com:qiuyue77/.config.git ~/.config
      ```

      

   4. 

5. #### 安装`zsh`

   1. 安装`zsh`

      ```bash
      sudo apt install zsh
      ```

      

   2. 将默认`shell`改为`zsh`

      ```bash
      chsh -s /bin/zsh # 注意：不要使用sudo root用户
      chsh -s /usr/bin/zsh # 本地当前用户
      ```

      

   3. 配置密码文件,解决`chsh:PAM认证失败的问题

      ```bash
      sudo vim /etc/passwd
      ```

      将第一行的/bin/bash改成/bin/zsh，这个是root用户

      

   4. 安装`oh-my-zsh`

      ```bash
      sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
      ```

      

   5. 设置`zshrc`

      修改`zsh/zshrc`中`ZSH`路径

      ```bash
      export ZSH="/home/XXX/.oh-my-zsh"
      ```

      创建软连接

      ```bash
      cd ~
      rm .zshrc
      ln -s ~/.config/zsh/zshrc .zshrc
      ```

      

   6. 安装语法建议、语法高亮插件

      ```bash
      git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
      git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
      ```

      

   7. 安装`z.lua` 跳转目录插件

      安装`lua`及依赖

      ```bash
      # 依赖
      sudo apt install libreadline7 libreadline-dev 
      wget http://www.lua.org/ftp/lua-5.3.5.tar.gz
      tar -zxvf lua-5.3.5.tar.gz && cd lua-5.3.5/ && make linux test
      sudo make isntall
      ```

      安装`z.lua`

      ```bash
      cd .config && git clone https://github.com/skywind3000/z.lua.git
      ```

      在`.zshrc` 中添加(如果复制了`config`,则不用添加)

      ```bash
      eval "$(lua ~/.config/z.lua/z.lua  --init zsh once enhanced)"
      ```

      

   8. ```bash
      cd .config && git clone https://github.com/skywind3000/z.lua.git
      ```

      在`.zshrc` 中添加,如果已复制`config`可不用添加

      ```bash
      eval "$(lua ~/.config/z.lua/z.lua  --init zsh once enhanced)"
      ```

      

6. #### 安装nodejs

   由于`nvim`插件`coc`需要`nodejs`，须先安装

   ```bash
   sudo apt install nodejs npm
   ```

   如果执行`nodejs`时显示版本太低：

   ```bash
   sudo npm cache clean -f
   sudo npm install -g n
   sudo n stable
   ```

   更改`npm`镜像源

   ```bash
   npm config set registry https://registry.npm.taobao.org
   ```

   安装`cnpm`，使用其他镜像（例子为淘宝镜像）

   ```bash
   sudo npm install -g cnpm
   ```

   安装`yarn`及前端脚手架工具

   ```bash
   sudo cnpm install -g yarn fis3
   ```

   

7. #### 安装`neovim`

   1. 此方法会安装的`nvim`最新版本:

      ```bash
      sudo apt-get install software-properties-common
      ```

      添加`nvim`的源,`stable`表示稳定版,`unstable`表示下载的是最新版

      ```bash
      sudo apt-add-repository ppa:neovim-ppa/stable 
      sudo apt update
      ```

      安装`nvim`

      ```bash
      sudo apt install neovim
      ```

      

   2. 检查`checkhealth`

      根据所报错误,安装依赖:

      需要`python2`的支持

      ```
      sudo apt install python-pip
      python2 -m pip install --user --upgrade pynvim
      ```

      需要`python3`的支持

      ```
      pip3 install --user pynvim
      ```

      需要`npm,yarn`支持

      ```
      sudo npm install -g neovim
      yarn global add neovim
      ```

      nvim与系统之间的复制粘贴，需要`xsel`或`xclip`

      ```
      sudo apt install xclip
      ```

      

8. #### 安装 ranger

   ```bash
   cd ~ && git clone https://github.com/ranger/ranger.git
   cd ranger && sudo make install
   ```

   预览代码高亮需要`bat`,解压缩需要`atool`：

   ```bash
   sudo apt install bat atool
   ```

   

9. #### 安装`FZF`

10. #### 安装`docker`和`docker-compose`

    ```bash
    sudo apt install docker.io
    ```

    编辑`~/.zshrc`,添加:   {如果是复制`config`,可不用添加)

    ```bash
    export DOCKER_HOST='tcp://0.0.0.0:2375'
    ```

    将当前用户 添加到docker用户组中

    ```bash
    sudo usermod -aG docker USER_NAME
    sudo gpasswd -a ${USER} docker
    ```

    查看docker-compose 最新版本 [网址](https://github.com/docker/compose/releases)

    安装docker-compose

    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    docker-compose --version
    ```

    


### WSL相关问题

1. `su` 问题

   如果出现 `su: Authentication failure`问题,先执行下面命令,设置`root`密码

   ```bash
   sudo passwd root
   ```

2. 







