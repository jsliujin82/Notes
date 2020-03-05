## Windows Terminal 相关设置

Windows Terminal 设置见[***网页***](https://www.jianshu.com/p/31bf9f9c0fb1)

Windows Terminal 主题设置见[***网页***](https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/windowsterminal)



## WSL 安装

### win10设置

1. 在“设置”-“更新与安全”-“开发者选项”选项中，启用开发人员模式。
2. 在“控制面板”-“程序”-“启用或关闭`Windows`功能”，确认“`Windows Subsystem for Linux（Beta）`”功能为勾选状态。

### 启用WSL2

1. #### `windows10`的版本要求于`Windows 10 18917`及其之上的版本.若版本升级最新也不够,则可通过升级`Windows Insider`来达到版本升级,

   可通过`cmd`查看当前`Windows`版本

   ```bash
   ver
   ```

   

2. #### 启用虚拟机平台可选组件

   在`PowerShell`中以管理员身份运行以下命令:

   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

   完成后,重启电脑.

   

3. #### 设置`WSL`发行版为`WSL2`,在`PowerShell`中运行:

   将默认的`WSL`发行版设置成为`WSL2`

   ```powershell
   wsl --set-default-version 2
   ```

   

   如果已经安装了某个发行版,则可以单独设置某个发行版为`wsl2`,如:

   ```powershell
   wsl --set-version Ubuntu-18.04 2
   ```

   

   验证使用的`WSL`版本

   ```powershell
   wsl -l -v
   ```

   

### WSL 安装

​	“应用商店”中搜索`Ubuntu`，安装`Ubuntu-18.04`，安装结束后,

​	首次打开需要打开`Ubuntu 18.04`,初始化并设置密码

​	之后均可在设置好了的`Windows Terminal`中打开

### WSL卸载

在`cmd`中以管理员身份运行：

+ 列出已安装的发行版

  ```PowerShell
  wslconfig /l
  ```

  

+ 删除指定发行版,如删除`Ubuntu-18.04`

  ```powershell
  wslconfig /u Ubuntu-18.04
  ```

  

### WSL 设置

1. #### 修改`apt`源:

   ```bash
   sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
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
      git clone git@github.com:qiuyue77/wsl_config.git ~/.config
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

      

   8. 安装`z.lua`
      
      ```bash
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
   cd ~ && git clone https://github.com/ranger/ranger.git ~/.ranger
   cd .ranger && sudo make install
   ```

   预览代码高亮需要`bat`,解压缩需要`atool`：

   `bat`在Ubuntu中不一定能通过`apt`下载,因此可以通过安装包`deb`安装,[下载地址](https://github.com/sharkdp/bat/releases)

   ```bash
   sudo dpkg -i bat_0.12.1_amd64.deb
   ```

   安装`atool`

   ```bash
   sudo apt install bat atool
   ```

   

9. #### 安装`FZF`

10. #### 安装`docker`和`docker-compose`

    安装依赖
    
    ```bash
sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
    ```

    添加`Docker`的`GPG`公钥
    
    ```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

    添加软件仓库
    
    ```bash
    sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
       $(lsb_release -cs) \
   stable"
    ```

    安装`docker`
    
    ```bash
    sudo apt update
    sudo apt install docker-ce
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





