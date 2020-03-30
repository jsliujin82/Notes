以滴答清单为例：

需要安装`electron`

```bash
sudo pacman -S nodejs
sudo cnpm install -g electron #全局安装
#或者通过yarn安装
yarn global add electron
```

创建滴答清单app的执行文件

```bash
mkdir -p ~/Prog/dida && cd ~/Prog/dida
```

- 初始化

  ```bash
  yarn init
  ```

  全部回车

- 创建`index.js`，并写入：

  ```js
  const { app, BrowserWindow, Menu } = require('electron')
  Menu.setApplicationMenu(null);
  app.on('ready', ()=>{
  	let win = new BrowserWindow()
  	win.loadURL('https://dida365.com/#q/all/tasks')
  })
  ```

- 运行一下命名，测试一下

  ```bash
  electron .
  ```

创建`dida`的执行文件

```bash
sudo nvim /bin/dida
```

添加以下内容：

```bash
electron /home/qiujl/Prog/dida
```

增加执行权限

```bash
sudo chmod +x /bin/dida
```



可以食用了

