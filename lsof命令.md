# linux lsof命令详解
# 简介
lsof(list open files) 是一个列出当前系统打开的文件。
在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等。系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。 因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表，这对系统监测以及排错将是很有帮助的。
# 输出信息含义
在终端下输入lsof即可显示系统打开的文件，因为 lsof 需要访问核心内存和各种文件，所以必须以 root 用户的身份运行它才能够充分地发挥其功能。
直接输入lsof，输出如下：
```shell
COMMAND    PID TID  USER   FD      TYPE DEVICE    SIZE              NODE NAME        init         1      root  cwd       DIR    0,2    4096  1688849860343691 /           init         1      root  rtd       DIR    0,2    4096  1688849860343691 /           init         1      root  txt       REG    0,2  591344  1970324837054349 /init       init         1      root  mem       REG    0,0                     79757 /init (path dev=0,2, inode=1970324837054349)                                                     init         1      root    3w      CHR   1,11          5629499534227761 /dev/kmsg   init         1      root    0u      CHR    1,3         13229323905415507 /dev/null   init         1      root    1u      CHR    1,3         13229323905415507 /dev/null   init         1      root    2u      CHR    1,3         13229323905415507 /dev/null   init         1      root    4u      CHR  10,50         11540474045159737 /dev/lxss   init         1      root    5u  unknown                                  /unknown (stat: Operation not permitted)                                                        
```
lsof输出各列信息的意义如下：
