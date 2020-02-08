## fzf安装配置说明
1.安装`fzf`
```
# manjaro
sudo pacman -S fzf
# Ubuntu
sudo apt install fzf
```
安装`ag` 需要用到`yay`
```
yay -S the_silver_searcher
```
`fzf` 预览功能需要用到`ccat`
```
yay -S ccat
```

2.终端添加`fzf` 功能
编辑`~/.zshrc`,添加
```bash
# fzf
export FZF_DEFAULT_OPTS='--bind ctrl-k:down,ctrl-i:up --preview "[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (ccat --color=always {} || highlight -O ansi -l {} || cat {}) 2> /dev/null | head -500"'
export FZF_DEFAULT_COMMAND='ag --hidden --ignore .git -g ""'
export FZF_COMPLETION_TRIGGER='\'
export FZF_TMUX_HEIGHT='80%'
export FZF_PREVIEW_COMMAND='[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (ccat --color=always {} || highlight -O ansi -l {} || cat {}) 2> /dev/null | head -500'
export RANGER_LOAD_DEFAULT_RC=FALSE
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

