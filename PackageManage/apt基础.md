#### `apt-cache`和`apt-get`是apt包的管理工具，他们根据`/etc/apt/sources.list`里的软件源地址列表搜索目标软件、并通过维护本地软件包列表来安装和卸载软件。

查看本机是否安装软件：

```bash
whereis package_name
#或者
which package_name
```



## apt功能:

| apt 命令                                  | 取代的命令(old-ubuntu)                        | 命令的功能                     |
| ----------------------------------------- | --------------------------------------------- | ------------------------------ |
| apt update                                | apt-get update                                | 刷新存储库索引                 |
| apt upgrade                               | apt-get upgrade                               | 升级所有可升级的软件包         |
| apt autoremove                            | apt-get autoremove                            | 自动删除不需要的包             |
| apt full-upgrade                          | apt-get dist-upgrade                          | 在升级软件包时自动处理依赖关系 |
| apt dist-upgrade                          | apt-get dist-upgrade                          | 升级系统                       |
| apt dselect-upgrade                       | apt-get dselect-upgrade                       | 使用`dselect`升级              |
| apt clean                                 | apt-get clean                                 | 清理无用的包                   |
| apt autoclean                             | apt-get autoclean                             | 清理无用的包                   |
| apt-get check                             |                                               | 检查是否有损坏的依赖           |
|                                           |                                               |                                |
| apt search ***packagename***              | apt-cache search ***packagename***            | 搜索应用程序                   |
| apt show ***packagename***                | apt-cache show ***packagename***              | 显示包细节                     |
| apt install ***packagename***             | apt-get install ***packagename***             | 安装软件包                     |
| apt install ***packagename*** --reinstall | apt-get install ***packagename*** --reinstall | 重新安装包                     |
| apt -f install ***packagename***          | apt-get -f install ***packagename***          | 修复安装                       |
| apt remove ***packagename***              | apt-get remove ***packagename***              | 移除软件包                     |
| apt purge ***packagename***               | apt-get purge ***packagename***               | 移除软件包及配置文件           |
| apt depends ***packagename***             | apt-cache depends ***packagename***           | 了解使用依赖                   |
| apt rdepends ***packagename***            | apt-cache rdepends ***packagename***          | 查看该包被哪些包依赖           |
| apt build-dep ***packagename***           | apt-get build-dep ***packagename***           | 安装相关的编译环境             |
| apt source ***packagename***              | apt-get source ***packagename***              | 下载该包的源代码               |



## apt 选项

| 选项 | 说明                                       |
| ---- | ------------------------------------------ |
| -h   | 本帮助文件。                               |
| -q   | 输出到日志 - 无进展指示                    |
| -qq  | 不输出信息，错误除外                       |
| -d   | 仅下载 - 不安装或解压归档文件              |
| -s   | 不实际安装。模拟执行命令                   |
| -y   | 假定对所有的询问选是，不提示               |
| -f   | 尝试修正系统依赖损坏处 `-f = –fix-missing` |
| -m   | 如果归档无法定位，尝试继续                 |
| -u   | 同时显示更新软件包的列表                   |
| -b   | 获取源码包后编译 -V 显示详细的版本号       |
| -c=? | 阅读此配置文件                             |
| -o=? | 设置自定的配置选项，如 -o dir::cache=/tmp  |

