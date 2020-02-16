# Manjaro 系统安装与配置
### 0.用Hyper-v安装

#### 0.1设置`Hyper-V `

> 在`Hyper-V Settings`选中 `Allow enhanced session mode`
>
> 在`Virtual Switch Manager`中建立`External network（外部网络）`的网络交换机

#### 0.2 创建虚拟机，注意以下

> 选择`Generation 2`
>
> 选择`镜像加载`

#### 0.3设置创建好的虚拟机`Settings for New_Manjaro`

*`Security`*中取消勾选`Enable Secure Boot'

#### 0.4 进入系统后，会如愿以偿的被卡住



### 1.升级系统

#### 1.1修改更新源
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
**安装`yaourt`**
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

**安装`yay`**
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

#### 1.2安装font
中文字体
```bash
yay -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei adobe-source-han-mono-cn-fonts adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
```
`wps`需要字体
```
sudo pacman -S ttf-wps-fonts
```
`Emoji`
```
yay -S ttf-linux-libertine ttf-inconsolata ttf-joypixels ttf-twemoji-color noto-fonts-emoji ttf-liberation ttf-droid
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

#### 1.5更新dns（不需要就不用执行）
```
# Arch Linux/Manjaro with Network Manager: 
sudo systemctl restart NetworkManager.service

# Arch Linux/Manjaro with Wicd: 
sudo systemctl restart wicd.service
```

#### 1.6双屏扩展
先查看视频输出端口
```bash
xrandr -q
```
设置双屏
```
xrandr --output LVDS1 --auto --output HDMI1 --auto --right-of LVDS1
```


### 2.安装app

#### 安装nodejs
```
# 由于vim插件coc需要nodejs，须先安装
sudo pacman -S nodejs npm
# 更改npm镜像源
npm config set registry https://registry.npm.taobao.org
# 安装cnpm，使用其他镜像（例子为淘宝镜像）
sudo npm install -g cnpm
# 安装yarn
sudo cnpm install -g yarn
# 前端脚手架工具
sudo cnpm install -g fis3
```
#### 安装vim
```
# 先自行复制.ssh
git clone git@github.com:qiuyue77/.config.git
cp -r .config/vim ~/.vim
sudo pacman -S vim
vim # 然后等待安装插件
```
#### 安装`neovim`
```
sudo pacman -S neovim
```
需要`python2`的支持
```
sudo pacman -S python2-pip
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
sudo pacman -S xsel
```

#### 更改pacman的状态
```
sudo -E vim /etc/pacman.conf
/Color
# 去掉Color前的备注
# 保存退出
sudo pacman -Syy # 刷新pacman
```

#### 更新shell 为 zsh
```bash
sudo pacman -S zsh # manjaro应该自带zsh
# 查看已安装shells列表
cat /etc/shells
# 安装`oh-my-zsh`
git clone https://github.com/ohmyzsh/ohmyzsh.git
sh ohmyzsh/tools/install.sh
# 更换shell
chsh -s /bin/zsh
chsh -s /usr/bin/zsh
```
复制粘贴自己的zshrc
```
cp ~/.config/zsh/zshrc ~/.zshrc
```
注意修改'zsh'路径

##### 安装自动跳转、语法建议、语法高亮插件
```bash
# sudo pacman -S autojump
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
##### 安装z.lua 跳转目录插件
```
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

##### 统一安装相关app
sudo pacman -S i3-gaps alacritty dmenu xorg lxappearance feh variety 

|--------------|--------------------------|----------------------------|
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
|--------------|--------------------------|----------------------------|

##### 删除i3窗口下的状态栏
```
sudo pacman -R i3status
```

##### xorg设置
```
xmodmap -pke > ~/.xmodmap
vim ~/.xmodmap
# 新建窗口 win+enter
xev # 可查看键盘键位所代表的key值
# 尽量复制自己的文件
# 更新key值后，刷新xmodmap
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
```
sudo pacman -S yandex-browser-beta vlc virtualbox ranger w3m apvlv docker netease-cloud-music wps-office typora 
```
|---------------------|------------------|-----------------------------------|
| 名称                | 描述             | 备注                              |
|---------------------|------------------|-----------------------------------|
| yandex-browser-beta | `yandex` 浏览器  | H5视频问题                        |
| vlc                 | 视频播放器       |                                   |
| virtualbox          | 虚拟机           |                                   |
| ranger              | 目录文件管理器   | 按照下方说明设置                  |
| w3m                 | 浏览器           | 用于`ranger`预览图片              |
| apvlv               | `PDF`阅读器      | 配合`ranger`使用,配置在`~/.apvlv` |
| docker              | `docker`虚拟机   | 查看是否自启动                    |
| netease-cloud-music | 网易云音乐       |                                   |
| wps-office          |                  | 安装`wps`相关字体                 |
| typora              | `Markdown`阅读器 |                                   |
|---------------------|------------------|-----------------------------------|

##### chrome开源版chromium
由于与yandex有冲突，故不安装
```
sudo pacman -S chromium
```

##### 修复yandex-browser不能播放H5视频问题
```
sudo pacman -U ~/.config/browser/chromium-codecs-ffmpeg-extra.....
```

##### ranger配置
```
sudo pacman -S ranger
# 初始化ranger config 并改变ranger初始加位置
ranger --copy-config=all
export RANGER_LOAD_DEFAULT_RC=FALSE
# 复制自己的ranger config
```
插件[`Ranger Plugin`](https://github.com/ranger/ranger/wiki/Plugins):

[ranger_deviconsi](https://github.com/alexanderjeurissen/ranger_devicons)

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
```
# 启动docker服务
sudo systemctl start docker
# 查看docker服务的状态
sudo systemctl status docker
# 设置docker开机启动服务
systemctl enable docker
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
配置`shadowsocks` 新建修改`~/.config/shadowsocks/local.json` ``
```
{
	"server": "xxx.xxx.xxx.xxx",
	"server_port": 1002,
    "local_address": "127.0.0.1",
    "local_port": 1080,
	"password": "xxxxx",
	"method": "aes-256-cfb",
    "timeout": 300,
    "fast_open": true,
	"remarks": ""
}
```
启动`shadowsocks` 服务
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
设定全局代理，编辑`.zshrc`
```
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export ftp_proxy=http://127.0.0.1:8118
```

