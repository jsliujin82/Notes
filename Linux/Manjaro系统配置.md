# Manjaro 系统安装与配置
### 1. 升级系统

#### 1.1 修改更新源及基本设置
**1.1.1 设置镜源**

对国内的镜像源进行测速及选择
```bash
sudo pacman-mirrors -i -c China -m rank
```
编辑`pacman config`
```bash
sudo vi /etc/pacman.conf
```
找到`[mutilib]`相关配置的位置在下方
添加如下信息：
```
# 1. 如果选择清华：
[archlinuxcn]
SigLevel = Never
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
# 2. 如果选择的上海交大
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux-cn/$arch
# 3. 如果选择中科大
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = http://mirrors.ustc.edu.cn/archlinuxcn/$arch
```
保存后,更新同步源及导入GPG key
```bash
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```
同步源并更新系统
```bash
sudo pacman -Syyu
```
**1.1.2 安装`yaourt`并设置镜源**
```
sudo pacman -S yaourt
```
更换`yaourt`源
```bash
sudo cp /etc/yaourtrc /etc/yaourtrc.backup # 备份
sudo vi /etc/yaourtrc
```
将`AURURL`一栏取消备注，并更改：
```
AURURL="https://aur.tuna.tsinghua.edu.cn"
```

**1.1.3 安装`yay`并设置镜源**
```
sudo pacman -S yay
```
更改`yay`源
```
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```
查看`yay`当前的源
```
yay -P -g
```
`yay`配置文件位置
```
~/.config/yay/config.json
```

**1.1.4 SSH公钥**

优先使用自己的备份，记得
```bash
sudo chmod 600 ~/.ssh/id_rsa
```

生成SSH公钥
```bash
ssh-keygen
```

 查看公约
```
cat ~/.ssh/id_rea.pub
```

**1.1.5 git 设置**

git config 设置
```bash
git config --global user.name 'qiujl_Manjaro_PC'
git config --global user.email 'qiuyue77@outlook.com'
```

如果出现 git status 不能正常显示中文，输入
````bash
git config --global core.quotepath false 
````

不检查`filemode`,设置:

```bash
git config --global core.filemode false
```



**1.1.6 pip修改pip3源**
可直接复制`.config`中的`.pip`文件

```bash
mkdir ~/.pip && vim ~/.pip/pip.conf
```

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

##### 1.7 安装`deb`

安装`debtap`

```bash
yay -S debtap
```

升级`debtap`

```bash
sudo debtap -u
```

**使用方法**

```bash
sudo debtap xxxxx.deb
```

安装时会提示输入包名，以及`license`。包名随意，`license`就填``GPL``吧
上述操作完成后会在`deb`包同级目录生成`×.tar.xz`文件，直接用`pacman`安装即可

```bash
sudo pacman -U x.tar.xz
```

#### 1.2安装font

中文字体
```bash
yay -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei adobe-source-han-mono-cn-fonts adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts nerd-fonts-source-code-pro
```
`wps`需要字体

```bash
sudo pacman -S ttf-wps-fonts
```
`Emoji`

```bash
yay -S ttf-linux-libertine ttf-inconsolata ttf-joypixels ttf-twemoji-color noto-fonts-emoji ttf-liberation ttf-droid
```

查看系统中的字体（中文字体）

```bash
fc-list
fc-list :lang=zh
```

编辑`/etc/local.conf`

```bash
LANG=en_US.UTF-8
LC_ADDRESS=zh_CN.UTF-8
LC_IDENTIFICATION=zh_CN.UTF-8
LC_MEASUREMENT=zh_CN.UTF-8
LC_MONETARY=zh_CN.UTF-8
LC_NAME=zh_CN.UTF-8
LC_NUMERIC=zh_CN.UTF-8
LC_PAPER=zh_CN.UTF-8
LC_TELEPHONE=zh_CN.UTF-8
LC_TIME=zh_CN.UTF-8
```

`local.conf`设置解释

```txt
1. LC_COLLATE
定义该环境的排序和比较规则
2. LC_CTYPE
用于字符分类和字符串处理，控制所有字符的处理方式，包括字符编码，字符是单字节还是多字节，如何打印等。是最重要的一个环境变量。
3. LC_MONETARY
货币格式
4. LC_NUMERIC
非货币的数字显示格式
5. LC_TIME
时间和日期格式
6. LC_MESSAGES
提示信息的语言。另外还有一个LANGUAGE参数，它与LC_MESSAGES相似，但如果该参数一旦设置，则LC_MESSAGES参数就会失效。 LANGUAGE参数可同时设置多种语言信息，如LANGUANE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"。
7. LANG
LC_*的默认值，是最低级别的设置，如果LC_*没有设置，则使用该值。类似于 LC_ALL。
8. LC_ALL
它是一个宏，如果该值设置了，则该值会覆盖所有LC_*的设置值。注意，LANG的值不受该宏影响。
```



#### 1.3耳机声卡设置

```bash
sudo pacman -S pavucontrol
pavucontrol
```

#### 1.4解决git clone速度慢问题（可能有效）
[IP地址查询网址](https://www.ipaddress.com/)
[DNS查询网址](http://tool.chinaz.com/dns?type=1&host=&ip=)
查询下面地址的网址的IP地址

```txt
github.com
github.global.ssl.fastly.net
```
将查询到IP地址编辑到`/etc/hosts`中
```
***.***.***.*** github.com
***.***.***.*** github.global.ssl.fastly.net
# 例子
192.30.253.113 github.com
151.101.25.194 github.global.ssl.fastly.net
192.30.253.121 codeload.github.com
199.232.4.133 raw.githubusercontent.com
```

#### 1.5 更新dns（不需要就不用执行）
```
# Arch Linux/Manjaro with Network Manager: 
sudo systemctl restart NetworkManager.service

# Arch Linux/Manjaro with Wicd: 
sudo systemctl restart wicd.service
```

#### 1.6 双屏扩展
先查看视频输出端口
```bash
xrandr -q
```
设置双屏
```
xrandr --output LVDS1 --auto --output HDMI1 --auto --right-of LVDS1
xrandr --output LVDS1 --auto --output VGA1 --auto --right-of LVDS1
```

### 2.安装app

#### 安装nodejs
由于`nvim`插件`coc`需要`nodejs`，须先安装
```bash
sudo pacman -S nodejs npm
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

修改`yarn`源为`taobao`源

```bash
yarn config set registry https://registry.npm.taobao.org/
```

#### 安装vim(未安装，`nvim`代替)

```
git clone git@github.com:qiuyue77/.config.git
cp -r .config/vim ~/.vim
sudo pacman -S vim
vim # 然后等待安装插件
```

#### 安装`neovim`
```bash
sudo pacman -S neovim
```
需要`python2`的支持
```bash
sudo pacman -S python2-pip
python2 -m pip install --user --upgrade pynvim
```
需要`python3`的支持
```bash
pip3 install --user pynvim
```
需要`npm,yarn`支持
```bash
sudo npm install -g neovim
yarn global add neovim
```

nvim与系统之间的复制粘贴，需要`xsel`或`xclip`
```bash
sudo pacman -S xsel
```

复制`nvim`配置，设置软链接`init.vim`

```bash
cd ~/.comfig/nvim/
ln -s init_Manjaro.vim init.vim
```

#### 更改pacman的状态

```bash
sudo -E vim /etc/pacman.conf
/Color
# 去掉Color前的备注
# 保存退出
sudo pacman -Syy # 刷新pacman
```

#### 更新shell 为 zsh
```bash
sudo pacman -S zsh # manjaro应该自带zsh
```
查看已安装shells列表
```bash
cat /etc/shells
```

安装`oh-my-zsh`

>方法1：
>```bash
>git clone https://github.com/ohmyzsh/ohmyzsh.git
>sh ohmyzsh/tools/install.sh
>```
>方法2：
>```bash
>sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
>```
>方法3：
>```bash
>sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
>```

更换shell
```bash
chsh -s /bin/zsh
chsh -s /usr/bin/zsh
```
修改`zsh/zshrc`中`ZSH`路径,以免报错

```bash
export ZSH="/home/xxxx/.oh-my-zsh"
```

建立软连接

```bash
cd ~
rm .zshrc
ln -s ~/.config/zsh/Manjaro_zshrc ~/.zshrc
```
**注意修改'zsh'路径**

##### 语法建议、语法高亮插件
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
##### 安装z.lua 跳转目录插件
```bash
sudo pacman -S lua
cd .config && git clone https://github.com/skywind3000/z.lua.git
```
在`.zshrc` 中添加
```
eval "$(lua ~/.config/z.lua/z.lua  --init zsh once enhanced)"
```

#### 安装i3 及相关设置
```
sudo pacman -S i3
reboot
```
##### 登录界面时，选择i3登录，进入系统后，选择默认添加config，并选择win键为super键
##### 刷新i3 的config设置默认快捷键为 win+shift+r
##### 尽量复制自己的设置
```
# 启动i3自动启动程序 exec_always 
# 启动i3自动关闭程序 exec_killall 
# --no-startup-id 当启动了某些并不支持启动提醒的某脚本或程序时，鼠标指针会逗留在忙碌状态六十秒以上。为防止此现象，凡是 exec 命令都均加 --no-startup-id
```

##### 设置分辨率
```
vim ~/.Xresources
Xft.dpi: 200 # 显示器分辨率高的可选择200， 低的可选择不设置
# 保存退出
reboot
```

#### 统一安装相关app
```bash
sudo pacman -S i3-gaps alacritty dmenu xorg lxappearance feh variety compton polybar
```

| 名称         | 作用                     | 备注                       |
|--------------|--------------------------|----------------------------|
| i3-gaps      | i3窗口与窗口之间设置空格 |                            |
| alacritty    | 终端                     | 复制`Alacritty.yml`        |
| dmenu        | i3的导航菜单栏           | `win+d`打开                |
| xorg         | 更改硬件设置，主要是键位 | 复制xmodmap,`win+shift+n`  |
| lxappearance | 主题设置                 | 输入`lxappearance`设置主题 |
| feh          | 壁纸软件                 | 配合`variety`使用          |
| variety      | `feh`管理器              | 输入`variety`设置壁纸      |
| compton      | 渲染器（透明）           | 复制`picom.conf`           |
| polybar      | 状态栏                   | 复制`polybar/`             |

##### 删除i3窗口下的状态栏
```
sudo pacman -R i3status
```

##### xorg设置

[`xmodmap`详情](https://wiki.archlinux.org/index.php/Xmodmap)

```
xmodmap -pke > ~/.xmodmap
vim ~/.xmodmap
```

新建窗口 win+enter
```
xev # 可查看键盘键位所代表的key值
```

尽量复制自己的文件
更新key值后，刷新xmodmap
```
xmodmap ~/.xmodmap
```

#### 安装输入法及设置

```bash
sudo pacman -S fcitx fcitx-im fcitx-libpinyin kcm-fcitx fcitx-configtool fcitx-sogoupinyin   fcitx-sunpinyin
sudo pacman -U https://arch-archive.tuna.tsinghua.edu.cn/2019/04-29/community/os/x86_64/fcitx-qt4-4.2.9.6-1-x86_64.pkg.tar.xz
```

修改`~/.xprofile`
```
vim ~/.xprofile
```
添加如下文本
```
export GTK2_RC_FILES="$HOME/.gtkrc-2.0"
export LC_CTYPE=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```
激活修改后的`~/.xprofile`，并重启

```bash
source ~/.xprofile
reboot
```

如果搜狗输入法异常，删除`.config`中有关`sogou`的所有文件再重启

#### 安装常用app
```bash
sudo pacman -S yandex-browser-beta visual-studio-code-bin vlc virtualbox ranger w3m docker netease-cloud-music wps-office typora
```
安装常用打包基本工具
```bash
sudo pacman -S base-devel 
```
安装 `apvlv`
```bash
yaourt -S apvlv
```
| 名称                   | 描述             | 备注                                |
|------------------------|------------------|-------------------------------------|
| yandex-browser-beta    | `yandex` 浏览器  | H5视频问题                          |
| visual-studio-code-bin | `vscode`编辑器   | 安装插件                            |
| vlc                    | 视频播放器       |                                     |
| virtualbox             | 虚拟机           |                                     |
| ranger                 | 目录文件管理器   | 按照下方说明设置                    |
| w3m                    | 浏览器           | 用于`ranger`预览图片                |
| pandoc                 | 文档转换器       | 安装相关文档的插件                  |
| apvlv                  | `PDF`阅读器      | 配合`ranger`使用,配置在`~/.apvlv`   |
| docker                 | `docker`虚拟机   | 查看是否自启动,安装`docker-compose` |
| netease-cloud-music    | 网易云音乐       |                                     |
| wps-office             |                  | 安装`wps`相关字体                   |
| typora                 | `Markdown`阅读器 |                                     |

##### chrome开源版chromium
由于与yandex有冲突，故不安装
```
sudo pacman -S chromium
```

##### 修复yandex-browser不能播放H5视频问题
```
sudo pacman -U ~/.config/browser/chromium-codecs-ffmpeg-extra.....
```

##### 安装texlive
```bash
sudo pacman -S texlive-core texlive-bin texlive-most texlive-lang texlive-langchinese texlive-latexextra
```


如果没有安装`latexextra`,这个时候还会出一个神奇的错误,缺少`environ.sty`,也就是`environ.sty not found`.

不过事实上,我们即使安装了,也有可能会出现缺少别的一些东西.

**安装`texlive-localmanager-git`**
这个是`archlinux`的一个脚本,在`aur`上,需要使用`yaourt`进行安装:

```bash
yaourt texlive-localmanager-git
```

**使用`localmanager`安装缺少的部分**
接下来就是缺什么装什么的时候了,使用`tllocalmgr`进入命令行,之后缺什么install什么就行了 
大概方法:

```bash
tllocalmgr
```

之后在`manager`中,根据缺少的文件名:

```bash
install packagename
texhash
```

注意,一定要使用`texhash`,否则安装没有效果.

##### ranger配置

预览图片还可用`ueberzug`

```bash
pip3 install --user ueberzug
```

预览代码高亮需要`bat`，解压缩需要`atool`：

```bash
sudo pacman -S bat atool
```

初始化`ranger config` 并改变`ranger`初始加位置

```
ranger --copy-config=all
export RANGER_LOAD_DEFAULT_RC=FALSE
```

复制自己的ranger config
然后软连接自己的`ranger config`：

```bash
cd ~/.config/ranger
ln -s rc_Manjaro.conf rc.conf
ln -s rifle_Manjaro.conf rifle.conf
```

插件[`Ranger Plugin`](https://github.com/ranger/ranger/wiki/Plugins):

>[`ranger_deviconsi`](https://github.com/alexanderjeurissen/ranger_devicons)

自定义命令[Costom Commands](https://github.com/ranger/ranger/wiki/Custom-Commands) :

图片预览[Image Previews](https://github.com/ranger/ranger/wiki/Image-Previews) :

默认打开编辑器更改为nvim
编辑`~/.profile`,将

```
export EDITOR=/usr/bin/nano
```
更改为
```
export EDITOR=/usr/bin/nvim
```
编辑`~/.zshrc`，添加如下
```
export EDITOR=/usr/bin/nvim
export VISUAL=/usr/bin/nvim
```


##### docker相关
启动docker服务
```bash
sudo systemctl start docker
```
查看docker服务的状态
```bash
sudo systemctl status docker
```
设置docker开机启动服务
```bash
systemctl enable docker
```
将当前用户添加到`docker`用户组中

```bash
sudo usermod -aG docker USER_NAME
sudo gpasswd -a ${USER} docker
```

添加`docker`镜像

```bash
sudo nvim /etc/docker/daemon.json
```
输入以下内容：
```
{
  "registry-mirrors": ["https://ne7hlub7.mirror.aliyuncs.com"]
}
```
重新加载
```
sudo systemctl daemon-reload 
sudo systemctl restart docker
```

**安装`docker-compose`**
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
查看`docker-compose`版本
```bash
docker-compose --version
```


##### 微信！！！！（未成功）
```
sudo pacman -S aurman
aurman -S deepin-wechat
```
##### pdf阅读器`zathura`，未找到配置相关项，故未安装
`zathura-pdf-poppler`为`zathura` 必须插件
```
sudo pacman -S zathura zathura-pdf-poppler
```

#### 安装及配置`shadowsocks` `privoxy`
```
sudo pacman -S shadowsocks privoxy
```
复制`~/.config/shadowsocks/local.json`
```bash
sslocal -c ~/.config/shadowsocks/local.json

```
配置`privoxy` 编辑`/etc/privoxy/config`
搜索到`listen-address 127.0.0.1:8118`,保证其未被注释;
在搜索`forward-socks5t / 127.0.0.1:1080 .`,取消注释
保存退出，再启动`privoxy`
```
systemctl start privoxy
```
设定全局代理，编辑`.zshrc`，复制的`.zshrc`已经设置相关函数
```
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export ftp_proxy=http://127.0.0.1:8118
```

另外的全局代理func

```bash
proxy () {
  export ALL_PROXY="socks5://127.0.0.1:1080"
  export all_proxy="socks5://127.0.0.1:1080"
  #echo -e "Acquire::http::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/nul    l
  #echo -e "Acquire::https::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/nu    ll
  curl https://ip.gs
}

 noproxy () {
   unset ALL_PROXY
   unset all_proxy
   #sudo sed -i -e '/Acquire::http::Proxy/d' /etc/apt/apt.conf
   #sudo sed -i -e '/Acquire::https::Proxy/d' /etc/apt/apt.conf
   curl https://ip.gs
 }
```

