## pyenv安装

### 安装依赖:

1. #### 在`Ubuntu`

   ```bash
   sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
   libreadline-dev libsqlite3-dev llvm libncurses5-dev libncursesw5-dev \
   xz-utils tk-dev libffi-dev liblzma-dev python-openssl

   sudo apt install libedit-dev
   ```

2. #### 在`Manjaro`

   ```bash
   pacman -S --needed base-devel openssl zlib bzip2 readline sqlite llvm ncurses xz tk libffi python-pyopenssl 

   yay -S ncurses5-compat-libs
   ```

### 安装`pyenv`

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

### 配置Pyenv的环境变量以及Pyenv启动的语句

(已复制了`.zshrc`,可不用设置)

1. #### 配置`Pyenv`环境变量

   ```bash
   echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
   echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
   ```

2. #### 设置`Pyenv`启动语句

   ```bash
   echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
   ```

3. #### 刷新配置

   ```bash
   exec "$SHELL"
   ```

   或者

   ```bash
   source ~/.zshcr
   ```

### Pyenv的使用

1. 命令功能表

   | 命令           | 功能                                                  |
   | -------------- | ----------------------------------------------------- |
   | commands       | 列出所有的可执行的`pyenv`命令                         |
   | global         | 查看或者设置全局`Python`环境                          |
   | local          | 查看或者设置局部`Python`环境                          |
   | shell          | 查看或者设置临时`Python`环境                          |
   | install --list | 查看`Pyenv`能安装什么版本的`Python`                   |
   | install        | 安装`Python`版本                                      |
   | uninstall      | 卸载已安装的`Python`版本                              |
   | rehash         | 重建环境变量                                          |
   | version        | 列出当前环境的`Python`版本                            |
   | versions       | 列出`Pyenv`能设置的`Python`版本,星号表示当前活动版本  |
   | which          | 显示pyenv在运行给定命令时将调用的可执行文件的完整路径 |
   | whence         | 列出安装了给定命令的所有Python版本                    |

2. 部分命令详解:

   1. `install`

      ```bash
      Usage: pyenv install [-f] [-kvp] <version>
             pyenv install [-f] [-kvp] <definition-file>
             pyenv install -l|--list
             pyenv install --version

        -l/--list          列出所有可安装的版本
        -f/--force         既是已经安装了该版本,也要继续安装
        -s/--skip-existing 如果该版本已经安装了就跳过安装

        python-build options:

        -k/--keep          Keep source tree in $PYENV_BUILD_ROOT after installation
                           (defaults to $PYENV_ROOT/sources)
        -p/--patch         Apply a patch from stdin before building
        -v/--verbose       Verbose mode: print compilation status to stdout
        --version          Show version of python-build
        -g/--debug         生成调试版本
      ```

      1. 用国内源安装版本:

         因为国外服务器下载速度太慢,可更换为国内源进行下载,以版本`3.8.2`为例

         ```bash
         v=3.6.8;wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v
         ```

         + 其中`v=3.8.2`表示将要安装的版本
         + `wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/` 指的是把 Python下载到pyenv的安装路径下的`cache`文件夹下。
         + 然后执行安装该版本

      2.

   2. `global`,`local`,`shell`

      + `global`

        通过将版本名称写入`~/.pyenv/version`文件来设置要在所有shell中使用的Python的全局版本。该版本可以被特定于应用程序的`.python-version`文件覆盖，也可以通过设置`PYENV_VERSION`环境变量来覆盖。

      + `local`

        通过将版本名称写入`.python-version`当前目录中的文件来设置本地特定于应用程序的Python版本。该版本覆盖全局版本，并且可以通过设置`PYENV_VERSION`环境变量或`pyenv shell` 命令来覆盖自身。

      + `shell`

        通过`PYENV_VERSION` 在shell中设置环境变量来设置特定于shell的Python版本。此版本覆盖特定于应用程序的版本和全局版本。

      一次指定某一个版本

      ```bash
      pyenv global/local/shell x.x.x
      ```

      一次指定多个版本

      ```bash
      pyenv global/local/shell x.x.x x.x.x
      ```

      取消本地/外壳版本

      ```bash
      pyenv local/shell --unset
      ```

   3. `rehash`

      为pyenv（即，`~/.pyenv/versions/*/bin/*`）已知的所有Python二进制文件安装填充程序 。在安装新版本的Python之后运行此命令，或安装提供二进制文件的软件包。

## 插件`pyenv-virtualenv`安装及使用

1. ### 安装

   1. #### `pyenv-virtualenv`

      ```bash
      git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
      ```

   2. #### 配置环境变量

      ```bash
      echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
      ```

   3. #### 刷新配置

      ```bash
      source ~/.zshrc
      ```

2. ### 使用

   1. #### 创建虚拟环境

      ```bash
      pyenv virtualenv x.x.x env_x  # 虚拟环境名
      ```

   2. #### 删除虚拟环境

      ```bash
      pyenv uninstall env_x  
      pyenv virtualenv-delete env_x
      ```

   3. #### 列出现在的虚拟环境

      ```bash
      pyenv virtualenvs
      ```

   4. #### 激活虚拟环境

      ```bash
      pyenv activate env_x
      ```

   5. #### 退出虚拟环境

      ```bash
      pyenv deactivate
      ```

3. ### 其他

