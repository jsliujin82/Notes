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

   

4. #### `git clone` 自己定义的`config`文件

   1. `git`设置

      `git config` 设置

      ```bash
      git config --global user.name 'qiujl_wsl'
      git config --global user.email 'qiuyue77@outlook.com'
      ```

      

      如果出现 `git status `不能正常显示中文，输入

      ```bash
      git config --global core.quotepath false 
      ```

      

   2. `.config`

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

      

   5. 软连接`zshrc`

      ```bash
      cd ~
      rm .zshrc
      ln -s ~/.comfig/zsh/zshcr /zshrc
      ```

      

      1. 

   6. 

   > 1. **软连接`zshrc`**
   >
   >    ```bash
   >    cd ~
   >    rm .zshrc
   >    ln ~/.comfig/zsh/wsl_zshcr /zshrc
   >    ```
   >
   > 2. **安装zsh-syntax-highlighting语法高亮插件**
   >
   >    ```bash
   >    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
   >     echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
   >    source ~/.zshrc
   >    ```
   >
   > 3. **安装zsh-autosuggestions语法历史记录插件**
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
   > 4. ##### 安装z.lua 跳转目录插件
   >
   >    安装`lua`
   >
   >    ```bash
   >     wget http://www.lua.org/ftp/lua-5.3.5.tar.gz
   >    tar -zxvf lua-5.3.5.tar.gz
   >     cd lua-5.3.5/
   >    make linux test
   >    ```
   >
   >    如果报错，应该是依赖不全
   >
   >    ```bash
   >    sudo apt install libreadline7 libreadline-dev
   >    make linux test
   >    make install
   >    ```
   >
   >    安装`z.lua`
   >
   >    ```bash
   >    cd .config && git clone https://github.com/skywind3000/z.lua.git
   >    ```
   >
   >    在`.zshrc` 中添加
   >
   >    ```
   >    eval "$(lua ~/.config/z.lua/z.lua  --init zsh once enhanced)"
   >    ```
   >
   > 5. **配置主题**
   >
   >    ```bash
   >    sudo vim ~/.zshrc
   >    # 找到ZSH_THEME=，修改=后面引号内的内容
   >    ZSH_THEME="ys"
   >    ```

   #### 

6. #### 安装neovim

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

   ```bash
   sudo apt install software-properties-common
   ```

   

7. 

8. 还有

### WSL相关问题

1. `su` 问题

   如果出现 `su: Authentication failure`问题,先执行下面命令,设置`root`密码

   ```bash
   sudo passwd root
   ```

   

2. 还有







