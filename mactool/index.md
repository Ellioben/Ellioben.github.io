# mac/linux好用的工具


mac/linux好用的工具



## crontab

1、Linux和Mac下操作crontab都是一致的

2、配置文件都在/etc/crontab下，如果没有就创建。

3、crontab参数

　　crontab [-u user] file crontab [-u user] [ -e | -l | -r ]

- -u user：用来设定某个用户的crontab服务；
- file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
- -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
- -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
- -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
- -i：在删除用户的crontab文件时给确认提示。

 

4、配置环境变量，打开open ~/.bash_profile文件添加以下内容;

　　EDITOR=vim crontab -e；export EDITOR

5、crontab的文件格式

```bash
eg：* * * * * sh /xxxxx/davecron.sh >>/xxxxx/davecron.log
```



### 查看当前的cron job

```bash
sudo crontab -l                                                                                                                                          
35 22 * * * /Users/xxx/shell/openSafari.sh
```

### 添加cron job

```bash
sudo crontab -e 
35 22 * * * /Users/xxx/shell/openSafari.sh
#编辑或添加需要执行的时间和任务
 * 第1列分钟0～59
 * 第2列小时0～23（0表示子夜）
 * 第3列日1～31
 * 第4列月1～12
 * 第5列星期0～7（0和7表示星期天）
 * 第6列要运行的命令
 #添加完成查看
 sudo crontab -l                                                                                                                                          

```

## jq
> json文件解析工具



## typora
> md文件“所见即所得”

 当然也有一些不足
 so make your typora perfect--->>https://github.com/Ellioben/
 

## OrbStack
> 谁下谁知道，非常好用的docker，虚拟机客户端
秒级启动
