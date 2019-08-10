# linux lsof命令详解
# 简介
lsof(list open files) 是一个列出当前系统打开的文件。
在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等。系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。 因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表，这对系统监测以及排错将是很有帮助的。



每行显示一个打开的文件，若不指定条件默认将显示所有进程打开的所有文件。
lsof输出各列信息的意义如下：
- COMMAND：进程的名称 
- PID：进程标识符
- USER：进程所有者
- FD：文件描述符，应用程序通过文件描述符识别该文件。如cwd、txt等 
- TYPE：文件类型，如DIR、REG等
- DEVICE：指定磁盘的名称
- SIZE：文件的大小
- NODE：索引节点（文件在磁盘上的标识）
- NAME：打开文件的确切名称

FD 列中的文件描述符cwd 值表示应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改,txt 类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序。

其次数值表示应用程序的文件描述符，这是打开该文件时返回的一个整数。如上的最后一行文件/dev/initctl，其文件描述符为 10。u 表示该文件被打开并处于读取/写入模式，而不是只读(R)或只写 (w) 模式。同时还有大写 的W 表示该应用程序具有对整个文件的写锁。该文件描述符用于确保每次只能打开一个应用程序实例。初始打开每个应用程序时，都具有三个文件描述符，从 0 到 2，分别表示标准输入、输出和错误流。所以大多数应用程序所打开的文件的 FD 都是从 3 开始。

与 FD 列相比，Type 列则比较直观。文件和目录分别称为 REG 和 DIR。而CHR 和 BLK，分别表示字符和块设备；或者 UNIX、FIFO 和 IPv4，分别表示 UNIX 域套接字、先进先出 (FIFO) 队列和网际协议 (IP) 套接字。

# lsof命令常用的选项包括
- -i， 显示socket文件描述符。该选项的使用方法是:
```Shell
lsof -i [46] [protocol][@hostname][ipaddr][:service|port]
```
其中，4表示IPv4协议，6表示IPv6协议；protocol指定传输层协议，可以是TCP或者UDP；hostname指定主机名； ipaddr指定主机的IP地址； service指定服务名； port指定端口号。比如，要显示所有连接到主机 192.168.1.108 的ssh服务的socket文件描述符，可以使用命令：
```Shell
lsof -i@192.168.1.108:22
```
如果-i选项后不指定任何参数，则lsof命令将显示所有socket文件描述符。
- -u， 显示指定用户启动的所有进程打开的所有文件描述符。
- -c， 显示指定的命令打开的所有文件描述符。比如要查看websrv程序打开了那些文件描述符，可以使用如下命令。
```Shell
lsof -c websrv
```
- -p， 显示指定进程打开的所有文件描述符。
- -t， 仅显示打开了目录文件描述符的进程的PID。
我们还可以直接将文件名作为lsof命令的参数，以查看那些进程打开了该文件。如下所示：
```Shell
root@sus:/opt# lsof /opt
COMMAND  PID USER   FD   TYPE DEVICE SIZE             NODE NAME                      
bash      20 root  cwd    DIR    0,2 4096 1407374883676941 /opt                      
bash     320 root  cwd    DIR    0,2 4096 1407374883676941 /opt                      
squid   1230 root  cwd    DIR    0,2 4096 1407374883676941 /opt                      
lsof    1296 root  cwd    DIR    0,2 4096 1407374883676941 /opt                      
lsof    1297 root  cwd    DIR    0,2 4096 1407374883676941 /opt                      
```

用lsof命令查看squid服务器打开的文件描述符：
```Shell             
root@sus:/opt# ps -ef | grep -v grep | grep squid                                   
root      1230     1  0 Aug09 ?        00:00:00 squid                                
proxy     1232  1230  0 Aug09 ?        00:00:00 (squid-1)                            
proxy     1233  1232  0 Aug09 ?        00:00:00 (logfile-daemon) /var/log/squid/access.log                              
root@sus:/opt# lsof -p 1230                                                         
COMMAND  PID USER   FD   TYPE DEVICE    SIZE              NODE NAME                  
squid   1230 root  cwd    DIR    0,2    4096  1407374883676941 /opt                  
squid   1230 root  rtd    DIR    0,2    4096  1688849860343691 /                     
squid   1230 root  txt    REG    0,2 6111144  1407374884035128 /usr/sbin/squid       
squid   1230 root  mem    REG    0,0                    123507 /lib/x86_64-linux-gnu/libnss_systemd.so.2 (path dev=0,2, inode=1970324837098099)                           
squid   1230 root  mem    REG    0,0                    123490 /lib/x86_64-linux-gnu/libnsl-2.27.so (path dev=0,2, inode=1970324837098082)                                
squid   1230 root  mem    REG    0,0                    123502 /lib/x86_64-linux-gnu/libnss_nis-2.27.so (path dev=0,2, inode=1970324837098094)
squid   1230 root  mem    REG    0,0                    123492 /lib/x86_64-linux-gnu/libnss_compat-2.27.so (path dev=0,2, inode=1970324837098084)
squid   1230 root  mem    REG    0,0                    123497 /lib/x86_64-linux-gnu/libnss_files-2.27.so (path dev=0,2, inode=1970324837098089)
squid   1230 root  mem    REG    0,0                    141466 /usr/lib/x86_64-linux-gnu/libicudata.so.60.2 (path dev=0,2, inode=3659174697379994)
squid   1230 root    0u   CHR    1,3         13229323905415507 /dev/null
squid   1230 root    1u   CHR    1,3         13229323905415507 /dev/null
squid   1230 root    2u   CHR    1,3         13229323905415507 /dev/null
squid   1230 root    3u   REG    0,2    3686 19703248369908335 /var/log/squid/cache.log
squid   1230 root    4u   REG    0,9       8  5910974511282951 /run/shm/squid-cf__metadata.shm
squid   1230 root    5u   REG    0,9    8216  4222124651141627 /run/shm/squid-cf__queues.shm
squid   1230 root    6u   REG    0,9      44  2251799814167036 /run/shm/squid-cf__readers.shm
squid   1230 root    7u   CHR    1,3         13229323905415507 /dev/null
squid   1230 root    3u   IPv4   43816  0t0                TCP localhost:13579
squid   1230 root    4u   0000   0,9    0                  TCP anon_inode
```
# 输出信息含义
在终端下输入lsof即可显示系统打开的文件，因为 lsof 需要访问核心内存和各种文件，所以必须以 root 用户的身份运行它才能够充分地发挥其功能。
直接输入lsof，输出如下：
- COMMAND，执行程序所使用的的终端命令(默认仅显示前9个字符)
- PID，文件描述符所属进程的PID。
- USER，拥有该文件描述符的用户的用户名。
- FD，文件描述符的描述。其中cwd表示进程的工作目录， rtd表示用户的根目录，txt表示进程运行的程序代码，mem表示直接映射到内存中的文件(本例中都是动态库)。有的FD是以 "数字 + 访问权限" 表示的，其中数字是文件描述符的具体数值，访问权限包括r（可读）、w（可写）和 u（可读可写）。如果出现，0u、1u、2u分别表示标准输入、标准输出和标准错误输出；**3u表示处于LISTEN状态的监听socket**；**4u表示epoll内核事件表对应的文件描述符**。
- TYPE，文件描述符的类型。其中DIR是目录，REG是普通文件，CHR是字符设备文件，IPv4是IPv4类型的socket文件描述符，0000是未知类型。更多文件描述符的类型参考lsof命令的man手册。
- DEVICE，文件所属设备。对于字符设备和块设备，其表示方法是 “主设备号，次设备号” 。由
