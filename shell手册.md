# 1、Shell逻辑判断符号 _与_或_非_Shell_-a_-e_!作用

# 2、shell中的if条件语句格式如下
```Shell
SYSTEM=`uname -s`                   #获取系统信息
if [ $SYSTEM = "Linux" ] ; then     #如果是Linux
    echo "Linux" 
elif [ $SYSTEM = "FreeBSD" ] ; then   #如果是FreeBSD
    echo "FreeBSD" 
elif [ $SYSTEM = "Solaris" ] ; then  #如果是Solaris
    echo "Solaris" 
else #如果不是上述三种...
    echo "Unknow System?" 
fi  
```
切记，[] 和逻辑判断之间需要由空格， if判断结束需要用fi标识
# 3、if中常用的逻辑判断有以下几种
- 字符串判断
```Shell
str1 = str2     # 当两个串有相同内容、长度时为真 
str1 != str2    # 当串str1和str2不等时为真 
-n str1         # 串非空为真 
-z str1         # 空串为真 
str1            # 当串str1为非空时为真
```
- 数字判断
```Shell
int1 -eq int2　　　　两数相等为真
int1 -ne int2　　　　两数不等为真
int1 -gt int2　　　　int1大于int2为真
int1 -ge int2　　　　int1大于等于int2为真
int1 -lt int2　　　　int1小于int2为真
int1 -le int2　　　　int1小于等于int2为真
```
- 文件判断
```Shell
-r file    用户可读为真
-w file    用户可写为真
-x file    用户可执行为真
-f file    件为正规文件为真
-d file    文件为目录为真
-c file    文件为字符特殊文件为真
-b file    文件为块特殊文件为真
-s file    文件大小非0时为真
-t file    当文件描述符(默认为1)指定的设备为终端时为真
```
- 逻辑判断
```Shell
-a    与
-o    或
!     非
```
下面是总结
```Shell
[ -a FILE ]  如果 FILE 存在则为真。 
[ -b FILE ]  如果 FILE 存在且是一个块特殊文件则为真。  
[ -c FILE ]  如果 FILE 存在且是一个字特殊文件则为真。  
[ -d FILE ]  如果 FILE 存在且是一个目录则为真。  
[ -e FILE ]  如果 FILE 存在则为真。  
[ -f FILE ]  如果 FILE 存在且是一个普通文件则为真。  
[ -g FILE ] 如果 FILE 存在且已经设置了SGID则为真。 [ -h FILE ]  如果 FILE 存在且是一个符号连接则为真。  
[ -k FILE ]  如果 FILE 存在且已经设置了粘制位则为真。  
[ -p FILE ]  如果 FILE 存在且是一个名字管道(F如果O)则为真。  
[ -r FILE ]  如果 FILE 存在且是可读的则为真。  
[ -s FILE ]  如果 FILE 存在且大小不为0则为真。  
[ -t FD ]  如果文件描述符 FD 打开且指向一个终端则为真。  
[ -u FILE ]  如果 FILE 存在且设置了SUID (set user ID)则为真。  
[ -w FILE ]  如果 FILE 如果 FILE 存在且是可写的则为真。  
[ -x FILE ]  如果 FILE 存在且是可执行的则为真。  
[ -O FILE ]  如果 FILE 存在且属有效用户ID则为真。  
[ -G FILE ]  如果 FILE 存在且属有效用户组则为真。  
[ -L FILE ]  如果 FILE 存在且是一个符号连接则为真。  
[ -N FILE ]  如果 FILE 存在 and has been mod如果ied since it was last read则为真。  
[ -S FILE ]  如果 FILE 存在且是一个套接字则为真。  
[ FILE1 -nt FILE2 ]  如果 FILE1 has been changed more recently than FILE2, or 如果 FILE1 exists and FILE2 does not则为真。  
[ FILE1 -ot FILE2 ]  如果 FILE1 比 FILE2 要老, 或者 FILE2 存在且 FILE1 不存在则为真。  
[ FILE1 -ef FILE2 ]  如果 FILE1 和 FILE2 指向相同的设备和节点号则为真。  
[ -o OPTIONNAME ]  如果 shell选项 “OPTIONNAME” 开启则为真。  
[ -z STRING ]  “STRING” 的长度为零则为真。  
[ -n STRING ] or [ STRING ]  “STRING” 的长度为非零 non-zero则为真。  
[ STRING1 == STRING2 ]  如果2个字符串相同。 “=” may be used instead of “==” for strict POSIX compliance则为真。  
[ STRING1 != STRING2 ]  如果字符串不相等则为真。 
[ STRING1 &lt; STRING2 ] 如果 “STRING1” sorts before “STRING2” lexicographically in the current locale则为真。 [ STRING1 &gt; STRING2 ]  如果 “STRING1” sorts after “STRING2” lexicographically in the current locale则为真。  
[ ARG1 OP ARG2 ] “OP” is one of -eq, -ne, -lt, -le, -gt or -ge. These arithmetic binary operators return true if “ARG1” is equal to, not equal to, less than, less than or equal to, greater than, or greater than or equal to “ARG2”, respectively. “ARG1” and “ARG2” are integers.
```
# 4、shell 中的$0 $1 $* $@ $# $$ $? $() $(())
```Shell
$0: 脚本本身文件名称
$1: 命令行第一个参数，$2为第二个，以此类推
$*: 所有参数列表
$@: 所有参数列表
$#: 参数个数
$$: 脚本运行时的PID
$?: 脚本退出码

∗与@的区别

当命令行为test.sh 1 2 3
"$*“表示"1 2 3”
"$@“表示"1” “2” “3”
二者没有被引号括起来时是一样的都为"1 2 3"，只有当被引号括起来后才表现出差异
```
