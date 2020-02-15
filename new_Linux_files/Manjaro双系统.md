

### 双系统安装时 

记得于win所在键盘内建立`efi`分区，该分区的文件系统为`FAT32`，指定该分区为`/bbot/efi`分区，注意选择保留方式设置

### 分区情况 以250GB为例

| 文件系统   | 挂载点  | 大小   | 说明                                                       |
| :--------: | :------ | -----: | :--------------------------------------------------------- |
| ext4       | /boot   | 512M   | 系统启动及内核升级所用                                     |
| ext4       | /       | 50GB   | 根目录，唯一必须挂载目录                                   |
| ext4       | /var    | 40GB   | 用作缓存或者日志记录                                       |
| swap       |         | 4GB    | 交换分区，可能不设，一般为内存2倍，但内存>12GB，可设为4GB  |
| ext4       | /home   | 100GB  | 家目录                                                     |

### 双系统`Grub`启动顺序修改

在进入系统时，grub选择项中需要记住需要修改默认启动项的序号，第一个序号为0。

修改`grub`默认配置文件

```
sudo vi /etc/default/grub
```

把

```
GRUB_DEFAULT=saved
GRUB_TIMEOUT=10
```

修改为

```
GRUB_DEFAULT=2 # 假设默认启动的是第3个
GRUB_TIMEOUT=3 # 将等待时间修改为3秒
```

重新生成配置文件

```
grub-mkconfig -o /boot/grub/grub.cfg
```

### 同步双系统时间为当地时间
```
sudo timedatectl set-local-rtc 1 # 将硬件时间UTC改为CST，双系统时间保持一致
sudo ntpdate time.windows.com # 更新一下时间，确保时间无误
sudo hwclock --localtime --systohc # 将时间更行到硬件上
```


