# Manjaro 系统安装与配置
### 1.升级系统
#### 1.1修改更新源
```bash
# 对国内的镜像源进行测速及选择
sudo pacman-mirrors -i -c China -m rank
# 编辑pacman config
sudo nano /etc/pacman.conf
# 找到[mutilib]相关配置的位置在下方
# 添加如下信息：
[archlinuxcn]
SigLevel = Never
Server = https://mirrois.tuna.tsinghua.edu.cn/archlinuxcn/$arch
# 保存后执行
# 更新同步源及导入GPG key
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
# 同步源并更新系统
sudo pacman -Syyu
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
#### 安装vim
```
# 先自行复制.ssh
git clone git@github.com:qiuyue77/.config.git
cp -r .config/vim ~/.vim
sudo pacman -S vim
vim # 然后等待安装插件
```
#### 安装neovim
```
sudo pacman -S neovim
```
需要python2的支持
```
sudo pacman -S python2-pip
python2 -m pip install --user --upgrade pynvim
```
需要python3的支持
```
pip3 install --user pynvim
```

<++>
nvim与系统之间的复制粘贴，需要`xsel`或`xclip`
```
sudo pacman -S xsel
```

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
sudo cnpm isntall -g fis3
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
##### 安装自动跳转、语法建议、语法高亮插件
```
sudo pacman -S autojump
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

##### i3窗口与窗口之间设置空格
```
sudo pacman -S i3-gaps
# i3 config中最后添加
gaps inner 8
```

##### 删除i3窗口下的状态栏
```
sudo pacman -R i3status
```

##### 设置分辨率
```
vim ~/.Xresources
Xft.dpi: 200 # 显示器分辨率高的可选择200， 低的可选择不设置
# 保存退出
reboot
```

#### 安装alacrity 及设置   （更新terminal）
```
sudo pacman -S alacritty
# 编辑i3默认打开alacritty
vim ~/.config/i3/config
# 找到 start a terminal 修改为
bindsym $mod+Return exec alacritty
# 复制alacritty.yml到~/.config/alacritty
```

#### 安装dmenu 及设置  (i3的导航菜单栏)
```
sudo pacman -S dmemu
# i3中默认快捷键win+d 打开
```

#### 安装xorg及设置 （更改硬件设置，主要更改键位）
```
sudo pacman -S xorg
xmodmap -pke > ~/.xmodmap
vim ~/.xmodmap
# 新建窗口 win+enter
xev # 可查看键盘键位所代表的key值
# 尽量复制自己的文件
# 更新key值后，刷新xmodmap
xmodmap ~/.xmodmap
# i3 config 中添加自启动
exec_killall xmodmap
xmodmap ~/.xmodmap
```

#### 安装lxappearance及设置（主题）
```
sudo pacman -S lxappearance
lxappearance # 选择主题
```

#### 安装feh及设置（壁纸）
```
sudo pacman -S feh
# 安装feh管理器
sudo pacman -S variety
# win+s 输入 variety 设置壁纸等
```

#### 安装compton及设置（渲染器）
```
sudo pacman -S compton
# win+s 输入 compton 渲染
# 复制编辑~/.config/picom.conf
```

#### 安装输入法及设置
```
sudo pacman -S fcitx fcitx-im fcitx-configtool
sudo pacman -S fcitx-sogoupinyin
# 如果未安装成功，需添加yaourt来安装qtwebkit-bin
# sudo pacman -S yaourt
# sudo pacman -S base-devel
# yaourt -S qtwebkit-bin
# sudo pacman -S fcitx-sogoupinyin
vim ~/.xprofile
# 添加如下文本
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

#### 安装chrome开源版chromium
```
sudo pacman -S chromium
```

#### 安装yandex-browser
```
sudo pacman -S yandex-browser-beta
```
##### 修复yandex-browser不能播放H5视频问题
```
sudo pacman -U ~/.config/browser/chromium-codecs-ffmpeg-extra.....
```


#### 安装vlc，视屏播放器
```
sudo pacman -S vlc
```

#### 安装virtualbox，虚拟机
```
sudo pacman -S virtualbox
```

#### 安装ranger，文件管理器
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
```bash
sudo pacman -S w3m
```
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

#### 安装polybar，屏幕上方显示状态栏
```
sudo pacman -S polybar
mkdir ~/.config/polybar && cp /usr/share/doc/polybar/config ~/.config/polybar/
# 默认打开方式来
polybar example
# 自启动的脚本设置
vim launch.sh
killall polybar
polybar example
# 保存，并更改为可自行文件
chmod +x launch.ch
vim ~/.config/i3/config
# 添加如下
exec_always ~/.config/polybar/launch.ch
# 刷新i3 config
```
#### 安装docker
```
sudo pacman -S docker
# 启动docker服务
sudo systemctl start docker
# 查看docker服务的状态
sudo systemctl status docker
# 设置docker开机启动服务
systemctl enable docker
```

#### 安装网易云音乐
```
sudo pacman -S netease-cloud-music
```

#### 安装微信！！！！（未成功）
```
sudo pacman -S aurman
aurman -S deepin-wechat
```
#### 安装WPS
```
sudo pacman -S wps-office
```

#### 安装Typora

```bash
sudo pacman -S typora
```
#### 安装pdf阅读器`zathura`
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

### 3.i3桌面优化

