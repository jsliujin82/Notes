# rcm

#### `rcm` 是专门管理`dotfiles`的插件，[网址](https://github.com/thoughtbot/rcm)

## 安装

1. ##### `Alpine Linux`
  ```
  sudo apk add rcm
  ```

2. ##### `Manjaro`
  ```
  yay -S rcm
  ```

3. ##### `Ubuntu`
  ```
  sudo add-apt-repository ppa:martin-frost/thoughtbot-rcm
  sudo apt-get update
  sudo apt-get install rcm
  ```

## 使用

官网介绍[链接](http://thoughtbot.github.io/rcm/rcm.7.html)

### 4个命令

```bash
mkrc – 将文件转换为由 rcm 管理的隐藏文件
lsrc – 列出由 rcm 管理的文件
rcup – 同步由 rcm 管理的隐藏文件
rcdn – 删除 rcm 管理的所有符号链接
```

### 快速使用，以`zsh`为例：

将`.zshrc`文件转换成由`rcm`管理文件,`-t`是将此次文件放到标签文件`zsh`中,`-v`显示详情

```bash
mkrc -v -t zsh ~/.zshrc
```

查看是否转换成功，需要配置`rcrc`文件，且只显示`TAGS`中包含的`tags`和未打标签的文件

```bash
lsrc
```

将`rcm`管理的`zsh`标签文件同步出来

```bash
rcup -v -t zsh
```

删除`rcm`管理的相关符号链接

```bash
rcdn -v -t zsh
```

### **命令详情：**

1. #### [**`mkrc`**](http://thoughtbot.github.io/rcm/mkrc.1.html) 

   `mkrc	[-ChoqSsVvUu] [-t tag] [-d dir] [-B hostname] files`

   **参数：**

   **-B** *HOSTNAME*

   ​	use the supplied hostname instead of computing one. Implies **-o**.

   **-C**

   ​	copy instead of symlinking when installing the rc file back into your home directory

   **-d** *DIR*

   ​	install dotfiles under the specified directory. This can be specified multiple times.

   **-h**

   ​	show usage instructions.

   **-o**

   ​	install dotfiles into the host-specific directory

   **-q**

   ​	decrease verbosity

   **-S**

   ​	treat the specified rc files as files to be symlinked, even if they are directories

   **-s**

   ​	if the rc file is a file, symlink it; otherwise, make a directory structure as described in [rcup(1)](http://thoughtbot.github.io/rcm/rcup.1.html) in the section *[ALGORITHM](http://thoughtbot.github.io/rcm/mkrc.1.html#x414c474f524954484d)*. This is the default.

   **-t** *TAG*

   ​	install dotfiles according to tag

   **-U**

   ​	the specified files or directories are to be installed without a leading dot.

   **-u**

   ​	the specified files or directories are to be installed with a leading dot. This is the default.

   **-v**

   ​	increase verbosity. This can be repeated for extra verbosity.

   **-V**

   ​	显示版本号show the version number.

2. #### [`**lsrc**`](http://thoughtbot.github.io/rcm/lsrc.1.html) 

   `lsrc [-FhqVv] [-B hostname] [-d dir] [-I excl_pat] [-S excl_pat] [-s excl_pat] [-t tag] [-U excl_pat] [-u excl_pat] [-x excl_pat] [files ...]`

   **参数：**

   **-B** *HOSTNAME*

   ​	treat *host-HOSTNAME* as the host-specific directory instead of computing it based on the computer's hostname

   **-d** *DIR*

   ​	list dotfiles from the DIR. This can be specified multiple times.

   **-F**

   ​	show symbols next to each file indicating status information. Supported symbols are `@` which indicates that the file is a symlink, `$` which indicates it's a symlinked directory, and `X` to indicate that the file is a copy. More details on copied files and symlinked directories can be found in [rcrc(5)](http://thoughtbot.github.io/rcm/rcrc.5.html) under the documentation on **COPY_ALWAYS** and **SYMLINK_DIRS**, respectively.

   **-h**

   ​	show usage instructions.

   **-I** *excl_pat*

   ​	include the files that match the given pattern. This is applied after any **-x** options. It uses the same pattern language as **-x**; more details are in the *[EXCLUDE PATTERN](http://thoughtbot.github.io/rcm/lsrc.1.html#x4558434c554445205041545445524e)* section. Note that you may have to quote the exclude pattern so the shell does not evaluate the glob.

   **-S** *excl_pat*

   ​	symlink the directories that match the given pattern. This option can be repeated. You may need to quote the pattern to prevent the shell from swallowing the glob.

   **-s** *excl_pat*

   ​	if a directory matches the given pattern, recur inside of it instead of symlinking. See *[EXCLUDE PATTERN](http://thoughtbot.github.io/rcm/lsrc.1.html#x4558434c554445205041545445524e)* for more details. This is the opposite of the **-S** option, and can be used to undo it or the **SYMLINK_DIRS** setting in your `rcrc`configuration. It can be repeated, and the pattern may need to be 	quoted to protect it from your shell.

   **-t** *TAG*

   ​	list dotfiles according to TAG

   **-U** *excl_pat*

   ​	the rc files or directories matching this pattern will not be symlinked or created with a leading dot. This option can be repeated. You may need to quote the pattern to prevent the shell from swallowing the glob.

   **-u** *excl_pat*

   ​	if an rc file or directory matches the given pattern, it must be dotted. See *[EXCLUDE PATTERN](http://thoughtbot.github.io/rcm/lsrc.1.html#x4558434c554445205041545445524e)* for more details. This is the opposite of the **-U** option, and can be used to undo it or the **UNDOTTED** setting in your `rcrc` configuration. This option can be repeated. You may need to quote the pattern to prevent the shell from swallowing the glob.

   **-V**

   ​	show the version number.

   **-v**

   ​	increase verbosity. This can be repeated for extra verbosity.

   **-q**

   ​	decrease verbosity

   **-x** *excl_pat*

   ​	exclude the files that match the given pattern. This option can be repeated. Quote the pattern if it contains a valid shell glob.

   *files ...*

   ​	only list the specified file(s)

3. #### [`**rcup**`](http://thoughtbot.github.io/rcm/rcup.1.html) 

   `rcup[-CfhiKkqVv] [-B hostname] [-d dir] [-g] [-I excl_pat] [-S excl_pat] [-s excl_pat] [-t tag] [-U excl_pat] [-u excl_pat] [-x excl_pat] [files ...]`

   **参数：**

   **-B** *HOSTNAME*

   ​	treat *host-HOSTNAME* as the host-specific directory instead of computing it

   **-C**

   ​	copy the files instead of symlinking them

   **-d** *DIR*

   ​	install dotfiles from the *DIR*. This can be specified multiple times.

   **-f**

   ​	if the rc file already exists in your home directory but does not match the file in your dotfiles directory, remove the rc file then create the symlink

   **-g**

   ​	print to `stdout` a standalone shell script that will run the **rcup** command as specified. Nothing on your filesystem will be modified by **rcup** when this flag is passed.

   **-h**

   ​	show usage instructions.

   **-I** *EXCL_PAT*

   ​	install rc files that match *EXCL_PAT* despite being excluded by the **-x** flag or a setting in [rcrc(5)](http://thoughtbot.github.io/rcm/rcrc.5.html). This can be repeated with additional patterns. 

   **-i**

   ​	if the rc file already exists in your home directory but does not match the file in your dotfiles directory, prompt for how to handle it. This is the default

   **-K**

   ​	skip pre- and post-hooks

   **-k**

   ​	run pre- and post-hooks. This is the default.

   **-S** *EXCL_PAT*

   ​	any rc file that matches *EXCL_PAT* is installed as if it were a file (using a symlink) instead of as if it were a directory (by making a directory). This option can be repeated.

   **-s** *EXCL_PAT*

   ​	any file that matches *EXCL_PAT* is installed as normal, in accordance with the *[ALGORITHM](http://thoughtbot.github.io/rcm/rcup.1.html#x414c474f524954484d)* section below. This is the opposite of **-S**. This option can be repeated.

   **-t** *TAG*

   ​	install dotfiles according to *TAG*

   **-U** *EXCL_PAT*

   ​	any rc file that matches *EXCL_PAT* is installed without a leading dot. This option can be repeated. See the documentation of the **-U** option in [lsrc(1)](http://thoughtbot.github.io/rcm/lsrc.1.html) for more information.

   **-u** *EXCL_PAT*

   ​	any rc file that matches *EXCL_PAT* is installed with a leading dot. This is the opposite of **-U**. This option can be repeated. This is the default. 

   **-q**

   ​	decrease verbosity

   **-V**

   ​	show the version number.

   **-v**

   ​	increase verbosity. This can be repeated for extra verbosity. Verbose messages are printed to `stderr`.

   **-x** *EXCL_PAT*

   ​	do not install rc files that match *EXCL_PAT*. This can be repeated with additional patterns. 

   *files*

   ​	only install the specified file(s)

4. #### [`**rcdn**`](http://thoughtbot.github.io/rcm/rcdn.1.html) 

   `rcdn[-hKkqVv] [-B hostname] [-d dir] [-I excl_pat] [-S excl_pat] [-s excl_pat] [-t tag] [-U excl_pat] [-u excl_pat] [-x excl_pat] [files ...]`

   **参数：**

   **-B** *HOSTNAME*

   ​	treat *host-HOSTNAME* as the host-specific directory instead of computing it

   **-d** *DIR*

   ​	remove rc files from the *DIR*. This can be specified multiple times.

   **-h**

   ​	show usage instructions.

   **-I** *EXCL_PAT*

   ​	remove rc files that match *EXCL_PAT* despite being excluded by the **-x** flag or a setting in [rcrc(5)](http://thoughtbot.github.io/rcm/rcrc.5.html). This can be repeated with additional patterns. 

   **-K**

   ​	skip pre- and post-hooks

   **-k**

   ​	run pre- and post-hooks. This is the default.

   **-q**

   ​	decrease verbosity

   **-S** *EXCL_PAT*

   ​	when removing dotfiles, any file that matches *EXCL_PAT* should be treated as a file that was symlinked, even if it is a directory. This can be repeated.

   **-s** *EXCL_PAT*

   ​	when removing dotfiles, any directory that matches *EXCL_PAT* should be recurred upon like normal. This is the default, and is the opposite of **-S**. This can be repeated.

   **-t** *TAG*

   ​	remove dotfiles according to *TAG*

   **-U** *EXCL_PAT*

   ​	any rc file or directory that matches *EXCL_PAT* is considered to have been installed without a leading dot when removing them. Must be specified for rc files or directories that were indeed installed without a leading dot. This can be repeated. 

   **-u** *EXCL_PAT*

   ​	any rc file or directory that matches *EXCL_PAT* is considered to have been installed with a leading dot when removing them. This is the default, and is the opposite of **-U**. This can be repeated. 

   **-v**

   ​	increase verbosity. This can be repeated for extra verbosity.

   **-V**

   ​	show the version number.

   **-x** *EXCL_PAT*

   ​	do not remove rc files that match *EXCL_PAT*. This can be repeated with additional patterns. 

   *files*

   ​	only remove the specified file(s)

### 配置文件

​	`rcrc`默认在`~/.rcrc`

- #### **格式：**

  ```
  X=“abc efg”
  ```

- #### **实例:**

  ```bash
TAGS="i3 python alacritty"
  EXCLUDES="something"
```
  
- #### **参数：**

  - **COPY_ALWAYS**

    always copy files that match the listed globs, never symlink them.

  - **DOTFILES_DIRS**

    the source directories for dotfiles. The first in the list is the source to which dotfiles created using [mkrc(1)](http://thoughtbot.github.io/rcm/mkrc.1.html) are installed. The default value is *~/.dotfiles*

  - **EXCLUDES**

    a space-separated list of exclude patterns. 

  - **HOSTNAME**

    the hostname for this computer. This is normally computed using the [hostname(1)](http://thoughtbot.github.io/rcm/hostname.1.html) command, but this command is non-standard and can prove unreliable. The **HOSTNAME** variable forces a known hostname.

  - **TAGS**

    the default tags.

  - **SYMLINK_DIRS**

    a space-separated list of patterns. Directories matching a pattern are symlinked instead of descended. 

  - **UNDOTTED**

    a space separated list of patterns. Files matching this pattern will be symlinked without a leading dot. If a file is also in **SYMLINK_DIRS**, then the directory will be symlinked without a leading dot. 	