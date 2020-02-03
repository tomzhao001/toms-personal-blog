---
title: Shell笔记
categories: 
- [Linux, Shell]
tags: 
- Linux
- Shell
---

# Shell 命令

## lsof

源代码： `kill -9 $(lsof -t -i:4000)`

lsof其实就是 list open files 的缩写，就是列出打开正在运行的文件，所以这个的输出有点像`ps`，会包含进程号PID。

```
# lsof /var
COMMAND     PID     USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
syslogd     350     root    5w  VREG  222,5        0 440818 /var/adm/messages
syslogd     350     root    6w  VREG  222,5   339098   6248 /var/log/syslog
cron        353     root  cwd   VDIR  222,5      512 254550 /var -- atjobs
```



从参数来看，`-i`是列出IP Sockets，`-n`就是不要解析DNS，`-P`则是不要解析端口号。
比如，源代码中的 `lsof -n -P -i:4000` 就会得到：

```
COMMAND   PID    USER   FD   TYPE            DEVICE SIZE/OFF NODE NAME
node    42570 tomzhao   13u  IPv6 0x97a2cb3b6369411      0t0  TCP *:4000 (LISTEN)
```



然后就是`-t`，是用来取出PID的，所以这个源代码中的参数就应该是`42570`。

参考链接：https://en.wikipedia.org/wiki/Lsof