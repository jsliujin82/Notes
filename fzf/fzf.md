## fzf安装配置说明
1.安装`fzf`
```
# manjaro
sudo pacman -S fzf
# Ubuntu
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```
安装`ag` 需要用到`yay`
```
# manjaro
yay -S the_silver_searcher
# Ubuntu
sudo apt install silversearcher-ag
```
`fzf` 预览功能需要用到`ccat`

>`Manjaro`:
>
>```bash
>yay -S ccat
>```
>
>`Ubuntu`:
>
>ubuntu软件源里面是没有的，所以我就介绍一个通用的安装方法吧，首先下载二进制文件，[发行版网址](https://github.com/jingweno/ccat/releases)
>
>```bash
>wget https://github.com/jingweno/ccat/releases/download/v1.1.0/linux-amd64-1.1.0.tar.gz
>```
>
>解压，并将二进制文件复制到`/usr/local/bin`
>
>```bash
>tar -zxvf linux-amd64-1.1.0.tar.gz
>cd linux-amd64-1.1.0
>sudo mv ccat /usr/local/bin
>```
>
>接着给这个文件赋予可执行权限
>
>```bash
>sudo chmod +x /usr/local/bin/ccat
>```
>
>



2.终端添加`fzf` 功能
编辑`~/.zshrc`,添加
```bash
# fzf
export FZF_DEFAULT_OPTS='--bind ctrl-k:down,ctrl-i:up --preview "[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (ccat --color=always {} || highlight -O ansi -l {} || cat {}) 2> /dev/null | head -500"'
export FZF_DEFAULT_COMMAND='ag --hidden --ignore .git -g ""'
export FZF_COMPLETION_TRIGGER='\'
export FZF_TMUX_HEIGHT='80%'
export FZF_PREVIEW_COMMAND='[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (ccat --color=always {} || highlight -O ansi -l {} || cat {}) 2> /dev/null | head -500'
source ~/.config/zsh/key-bindings.zsh
source ~/.config/zsh/completion.zsh
```
3.`ranger` 添加`fzf` 功能，可以直接用备份文件
`~/.config/ranger/rc.conf` 添加
```
map <C-f> fzf_select
```
`~/.config/ranger/commands.py` 添加对应脚本
```python
class fzf_select(Command):
    pass
```

4. `nvim` 添加`fzf` 功能
`~/.config/nvim/init.vim` 中添加
```
# Plug中添加
Plug 'junegunn/fzf.vim'
# 插件设置中可直接复制原init.vim
...
```

