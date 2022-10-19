# Linux


## 开发常用

> 持续更新...

```bash
ls 
cd 
pwd
echo 
mv
rm x.txt
rm x.txt foo.txt
rm -r 
cp -r dir1 dir2 
scp -r
mkdir -p 
df

man
man pwd
man -h
ls -h
man --help

#查看文件内容
cat (Concatenate and print Files)
cat a.txt
cat a.txt b.txt
cat < a.txt
# head and tail (head 10 line ,last 10 line )
head a.txt
tail a.txt
tail -n 5 a.txt
tail -f a.txt 

# / 查找
man less | grep -n sim

##wc 统计单词
word,line and byte count 
man pwd | wc
man wc | wc
```



## chomd

```bash
chmod +X

chmod: (Change Mode)改变文件的权限

chmod +x foo #增加可执行权限/ +W +r
chmod -x foo #移除可执行权限/ -W-r 
chmod 740 foo # 把foo的权限设置成740 
 Owner: 7 = 1 + 2 + 4 = 可执行+可写+ 可读 
 Group: 4 = 4 = 可读
 Others： 0 = 没有任何权限常见：

###常见
644 -rw-r--r-- # default
755 -rwxr-Xr-X
777 -rwxrwxrwx # 危险! use chown/chgrp instead


#添加可执行权限
chomd +x xxx.py 

ll
total 16
-rw-r--r--  1 obsidianxiexxx  staff    21B  7 30 01:13 hellow.txt
-rw-r--r--  1 obsidianxiexxx  staff   316B  7 30 01:15 that.txt
可读可写
#添加可执行权限
chomd +x that.txt 


./that.txt      
./that.txt: line 1: syntax error near unexpected token `Move'
./that.txt: line 1: `              (That  is, CONTROL and LEFTARROW simultaneously.)  Move the cur-'

 ~/repositories/linux-command
 #就算修改名字后缀，也不影响执行情况。
mv that.txt that                                                                                                                            

~/repositories/linux-command
./that 
./that: line 1: syntax error near unexpected token `Move'
./that: line 1: `              (That  is, CONTROL and LEFTARROW simultaneously.)  Move the cur-'
  
  
 PATH=$PATH:$PWD
 
```

## 交互式文件编辑

nano

vim

还有其他交互式工具



##  Which & 环境变量$PATH

可以查找可执行文件

```bash
which app
echo $PATH #全局可执行文件


$PATH环境变量+ which命令
PATH=$PATH: $PWD

echo $PATH
:/Users/huahua/cmd2

$PATH：以：分割的文件夹列表
从哪里去找可执行文件(按照文件夹的顺序查找)
#把当前目录追加到PATH中
which: locate a program file in user's path % which my_echo
/Users/huahua/cmd2/my_echo

my_echo hello world! # Runs from anywhere Hello world!
```

## interpreter

```bash
#!/usr/bin/env python3
Shebang: #! SharpBang or haSHbang # #
使用哪个解释器(interpreter)去解释/运行脚本
必须放在第一行，并使用绝对路径

#!/bin/sh
#!/bin/bash
#!/usr/bin/perl
#!/usr/bin/php

% which python3
/Library/Frameworks/Python. framework/Versions/3.8/bin/python3
# 设置环境,并运行python3
#!/usr/bin/env python3 
```



## Command-line Tools

创建处理管道
下载数据
可以在一个github rozim/ChessData上下载测试数据
读取速度测试

```bash
#*.pgn以参数列表的形式返回所有匹配的文件 cat 1。pgn 2。pgn 。。 100。gpn /dev/nul1 是一个设备文件，它丢弃一切写入的数据但返回写入成功。
cat *.pgn > /dev/null   
#cat即读又是写，如果不是null，读和写或影响只读速度，所以写到黑洞里
定向到null是一个黑洞：丢数据。通常来测试硬盘的读取速度

这将是文件处理速度的上限


```



## 重定向管道

重定向 ：改变输入输出设备

```bash
标准输入<stdin>/标准输出<stdout>：控制台键盘/屏幕
echo hello > hellow.txt  #redirect to a file 
echo word >> hellow.txt  #append to a file
cat < hellow.txt  #use file as stdin <> read from file 

#########管道:前一个命令的标注输出作为下一个程序的标准输入
man less | grep sim
man less | grep sim | grep That > that.txt # a multiple pipes
#..
yes：一直输出y
yes AMD YES!' # -直输出 AMD YES!
yes 'yes' > yes.txt

Usages: Skip user confirmation
yes | sudo apt-get install

```



## 创建处理管道

管道中其实是并行执行的。

搜索指定行

```bash
cat *.pgn | grep "Result"
```

最简单的版本

```bash
cat *.pgn | grep "Result" sort uniq -c 
```

知识点:

- sort对输入文本进行排序
- uniq -C 统计每个独立行的出现次数仅对已排序文件有效

## 多进程处理任务

管道'|'中的命令其实是并行执行的。

```bash
sleep 3 | sleep 5 | echo '8'
```



## find

find ：查找文件，打印匹配文件名到标准输出

```bash
 find * | grep test   
```

xargs ：将参数列表(文件名)分段并执行命令
-n 每次(最多)取几个参数
-P最多几个命令同时并行执行
最后再用一个mawk合并结果



## stress & top & ps

### stress

不是电脑自带的。

> stress给系统增加负载或者进行压力测试

```bash
-t/--timeout N # N秒后超时
-c/--cpu N #孵化N个worker,死循环运行sqrt() / CPU 
-i/--io N # 孵化N个worker,死循环运行sync() / IO
-m/--vm N # 孵化NTworker,死循环运行malloc()/free() / Memory 
-d/--hdd N # 孵化N个worker,死循环运行write()/unlink() / Disk 
stress --cpu 8 --io 4 --vm 2 - -vm-bytes 128M --timeout 10s
```

### top

显示或更新排序过的进程信息（按照排名显示，显示有限）

- **默认按照CPU占用率排序**

### ps

ps (Process Status)显示进程状态（所有进程）

- 默认只显示当前用户有控制终端的进程

```bash
ps aux # 显示所有进程，包括其他用户的
ps aux | grep Chrome | wc -l # 看一看Chrome起了多少个进程
ps aux | grep -i chrome | wc -l

```





## Kill & killall

kill 终止或者给进程发信号

```bash
kill - signal_number/- signal_name PID
 # 默认发送15/TERM (software termination signal)
kill PID
kill -9/-KILL PID #强力杀进程ه
```

Killall按照名字终止进程

```bash
和kill一样，但是用名字作为参数
#如果是大众命令就有可能误伤
killall bash / killall Python 
killall -9 stress

```



## command+c vs command+z

都能让程序“停止”

Ctrl+C 向进程发送SIGINT中断信号，通常进程会终止 

Ctrl+Z 想进程发送SIGTSTP停止信号，把前台进程放入后台并挂起

- 进程还存在
- 打开的端口还会被占用





## 模拟客户端

- linux：nc 模拟客户端连接
- windows：curl 模拟客户端连接

## 其他

1. iperf：网络性能测试工具。

2. traceroute：识别数据包到达主机的路径。

3. ss：获取有关socket的详细信息。

4. bmon：监控实时带宽。

5. dig：列出所有的DNS

6. iftop：带宽情况统计工具。

7. tcpdump：网络数据包捕获和分析工具，堪称Linux下网络神器。

8. ip：分配和配置网络参数。

9. nmcli：用于进行网络连接故障排查。

10. vnstat：监控当前网络流量和网络带宽情况。

11. ```bash
    jps　　 //显示所有JAVA进程
    jps -l 　　//显示所有JAVA进程详情名
    ```

    


