1. 在需要建立项目的文件夹内右键打开git bash 
>1. 基本信息设置
>>1. 设置用户名
```
    git config --global user.name "qiuyue77"
```
>>2. 设置用户名邮箱
```
    git config --global user.email "qiuyue77@outlook.com"
```

2. 与服务器之间的连接(和linux终端大部分命令一致)
>>3. 从服务器上拷贝下来
```
    git clone https://github.com/qiuyue77/lecture0.git
```
>>4. 关注跟踪文件,并提交到缓存区
```
    git add 文件名
```
>>5. 提交描述
```
    git commit -m "描述内容"
```
>>6. 查看状态,查看暂存文件内部正在进行的操作的当前状态  
```
    git status
```
>>7. 将本地库内容发送至服务器
```
    git push
```
>>8. 将服务器库中内容的最新情况来回本地,
>>   可能存在别人和自己在同一时间端修改的情况,会出现冲突
>>   pull后git bash会有提醒,并在VScode呈现出来,
>>   选择想要的版本
```
    git pull
```
>>9.  查看存储库中的日志
```
    git log
```
>>10. 查看日志的大概情况
```
    git reflog
```
>>11. 回滚到之前的版本,hard后跟哈希值(只需前几位数能匹配就行)
```
    git reset
     > git reset --hard <commit>
     > git reset --hard origin/master
```

3. 本地之间
>1.进入项目的根文件夹(.git文件夹是仓库存放文件的的)
```
    git init
```
>2. 将工作区域的文件传到缓存区
```
    git add 文件名
```
>3. 提交到仓库并提交描述
```
    git commit -m "描述内容"
```
>4. 查看状态,查看仓库文件和工作区正在进行的操作的当前状态  
```
    git status
```
>5. 删除文件
>>  1. 先删除本地文件
>>    rm -rf 文件名
>>  2. 删除暂存区的文件
>>    git rm 文件名
>>  3. 提交操作
>>    git commit -m "描述内容"