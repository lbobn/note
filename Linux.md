

# 1. Linux目录结构

> `linux ` :/user/data/data.txt         无盘符
>
> `windows` : E:\user\data\data.txt

# 2. Linux命令入门

命令格式
~~~shell
command [-options] [parameter]
~~~
> -options:可选 
> parameter：参数

# 3.  列表命令(ls)

*`ls` 平铺方式列出当前工作目录下内容*

~~~shell
ls [-a -l -h] [linux路径]
~~~
> - `-a` : 表示all，列出所有文件（包括隐藏）
> 
> - `-l `: 以列表展示，可展示更多信息
> 
> - `-h`:列出文件大小，K,M,G
> 
>   选项可组合，如`-a -l`，`-lh`

# 4. 目录切换（cd/pwd)

### cd切换目录

~~~shell 
cd [linux路径]
~~~
> 若无路径，则回到home

### pwd 显示当前目录

~~~shell
pwd
~~~

### 相对路径与绝对路径

*相对路径*

>`~/Desktop`

*绝对路径*

> `/home/lubb/Desktop`

*特殊路径符*

>`.` : 当前路径 
>`..` : 上级路径
>`~` : home路径

# 5.创建目录(mkdir)

```shell 
mkdir [-p] linux路径 
```

> `-p`：可选项，表示自动创建父级不存在的目录，即多层级创建
>
> 参数必须

# 6. 文件操作命令

### 1.  创建文件 `touch`

~~~shell
touch linux路径
~~~

### 2. 查看文件内容 `cat/more`

~~~shell
cat Linux路径 ：全部显示

more Linux路径 ：分页显示，空格翻页，`q`退出查看
~~~

### 3. 复制文件/文件夹 `cp`

~~~shell
cp [-r] 参数1 参数2
~~~

>`-r` ：可选，表示递归，可用于文件夹复制
>
>`参数1` ： 要复制的文件夹或文件路径
>
>`参数2` ： 目标路径

### 4. 移动文件/文件夹 `mv`

~~~shell
mv 参数1 参数2
~~~

> `参数1` ： 要移动的文件夹或文件路径
>
> `参数2` ： 目标路径，目标不存在则改名

### 5.  删除 `rm`

~~~shell
rm [-r -f] 参数1 参数2 …… 参数n
~~~

> `-r` : 文件夹删除
>
> `-f` : 强制删除，无提示（root用户有提示）
>
> 参数可删除多个，也可用 `*` 通配符

# 7. 查找命令 which/find

### 查找命令地址 `which`

~~~shell
which 要查找的命令  :  查找命令所存放的地址
~~~



###  查找文件 `find`

~~~shell
find 起始路径 -name "被查找文件名"    : 可使用通配符

find 起始路径 -size +|-n[kMG]        : +大于，-小于
~~~



# 8. 过滤，统计，管道符 grep，wc，|

### 过滤grep ： 过滤行

~~~shell
grep [-n] 关键字 文件路径   ：-n 是否显示行数
~~~

### 统计wc：统计

~~~shell
wc [-c -m -l -w] 文件路径
~~~

> `-c ` 字节数 `-m` 字符数 `-l` 行数 `-w` 单词数

### 管道符

> `|` : 左边结果作为右边的输入
>
> 如：`cat test.txt | grep "lubb"` 和 `cat test.txt | grep "hello" | wc -l`

# 9. echo， 重定向符， tail

### `echo`输出

~~~shell
echo 输出内容
~~~

> ```` ``` : 反引号内为命令，如```echo 'pwd'```

### 重定向符

> `>` : 覆盖写入文件
>
> `>> `:  追加写入文件

### tail 查看文件末尾

~~~shell
tail [-f -num] Linux路径
~~~

> -num :具体数字，num行
>
> -f :持续跟踪

# 10. vi / vim 编辑器

~~~shell
vi 文件路径
vim 文件路径
~~~

[菜鸟教程]:https://www.runoob.com/linux/linux-vim.html

> 常用：
> `i `-- 切换到输入模式，在光标当前位置开始输入文本。
> `x` -- 删除当前光标所在处的字符。
> `:` -- 切换到底线命令模式，以在最底一行输入命令。
> `a` -- 进入插入模式，在光标下一个位置开始输入文本。
> `o` --在当前行的下方插入一个新行，并进入插入模式。
> `O` -- 在当前行的上方插入一个新行，并进入插入模式。
> `dd` -- 删除当前行。
> `yy` -- 复制当前行。
> `p` -- 粘贴剪贴板内容到光标下方。
> `P` -- 粘贴剪贴板内容到光标上方。
> `u` -- 撤销上一次操作。
> `Ctrl + r` -- 重做上一次撤销的操作。
> `:w` -- 保存文件。
> `:q` -- 退出 Vim 编辑器。
> `:q!` -- 强制退出Vim 编辑器，不保存修改。

# 11. root用户

`su` 命令

~~~shell
su [-] [用户名]
~~~

> `-` 表示切换后加载环境变量
>
> 用户名可省略，默认为root

`sudo` 命令

~~~shell
sudo 其他命令
~~~

> 可以以root权限执行命令
>
> 注：需提前配置 ，使用root用户的 `visudo` 命令编辑 `/etc/sudoers` 文件，
>		在其末尾添加      `lubb ALL=(ALL)		NOPASSWD:ALL`

# 12. 用户，用户组

> *创建组* ：`groupadd 用户组名`
>
> *删除组* ：`groupdel 用户组名`
>
> *创建用户* ：`useradd [-g -d] 用户名`
>
> - -g：指定用户组，需要已存在
> - -d：指定home路径
>
> *删除用户* ：`userdel [-r] 用户名`
>
> - -r : 删除home目录
>
> *修改密码*: `passwd`
>
> *查看用户组* ：`id [用户名]` 无参数则默认当前用户
>
> *修改用户所属组* ：`usermod -aG 用户组 用户名`
>
> *查看全部用户信息* ： `getent passwd`
>
> *查看全部组* ：`getent group`

# 13. 权限修改 chmod / chown

~~~shell
chmod [-R] 权限 文件或文件夹
~~~

> - -R：对文件夹内的全部内容修改
>- 权限 ：u=rwx,g=rwx,o=rwx 或 777
> 

~~~shell
chown [-R] [用户][:][用户组] 文件或文件夹
~~~

> - 如 `chown :root test.txt` 即将test.txt的用户组修改为root

# 14. 快捷键

> 1. ctrl + c
> 2. ctrl + d
> 3. `history`
> 4. !命令前缀，匹配上一个命令
> 5. ctrl + r 搜索历史命令
> 6. ctrl + a | e ，光标移动至头或尾
> 7. ctrl + ← | → ，光标左移/右移一个单词
> 8. ctrl + L 或 clear ，清屏

# 15. 软件安装 yum

*CentOS*

> ~~~shell
> yum [-y] [ install | remove | search ] 软件名    ：  -y 无提示
> ~~~

*Ubuntu*

> ~~~shell
> apt [-y] [ install | remove | search ] 软件名
> ~~~



# 16. 服务启动 systemctl

*可用于系统及第三方服务的启动，第三方需要注册*

~~~shell
systemctl start | stop | status | enable | disable 服务名
~~~

>  系统内置服务 ：
>
> - NetworkManager，主网络服务
> - network，副网络服务
> - firewalld，防火墙服务
> - sshd，ssh服务
>
> 第三方
>
> - ntp : 服务ntpd
> - httpd：httpd

# 17. 软链接 ln

~~~shell
ln -s 被链接 目的地
~~~

> 被链接只能绝对路径

# 18. 时间与时区 date

> 查看时间
>
> ~~~shell
> date [-d] [+格式]
>            - %Y 年
>            - %y 年(后两位)
>            - %m 月
>            - %d 日
>            - %H 时
>            - %M 分
>            - %S 秒
>            - %s 自1970-现在秒数
>       -d : 时间计算
>       如：date -d "+1 day" "+%Y-%m-%d",可选year,month,day,hour,minute,second
> ~~~

> 时区
>
> ~~~shell
> rm -f /etc/localtime
> sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
> ~~~

> 时间校准 ntp程序
>
> ~~~shell
> systemctl start ntpd
> systemctl enable ntpd
> 手动校准(root)
> ntpdate -u ntp.aliyun.com
> ~~~

# 19.ip地址和主机名

> `127.0.0.1` ： 本地回环ip
>
> host文件
>
> ~~~shell
> hostname  ： 查看主机名
> hostnamectl set-hostname 主机名    ： 修改主机名
> ~~~
>
> 

域名解析：通过主机名找到对应计算机IP地址（即主机名映射），先到本机记录找，再联网去DNS服务器找

# 20. 网络请求和下载

```shell
ping [-c num] ip或主机名   ：  -c 次数
```

~~~shell
网络下载：
wget [-b] url   : -b 后台下载
发起请求：
curl [-O] url   : -O ,可用于下载
~~~

# 21. 端口 

操作系统与外部交互的出入口

- 公认端口 ： 1~1023，用于系统内置或常用知名软件绑定
- 注册端口： 1024~49151，用于松散绑定使用（用户自定义口）
- 动态端口： 49152~65535，用于临时使用（多用于出口）

*查看端口占用*

~~~shell
#map
yum install -y nmap       ： 安装
nmap ip
#netstat
yum install -y net-tools
netstat -anp | grep 端口号
~~~

# 22. 进程管理

~~~shell
ps -ef  : 查看进程信息
ps -ef | grep 关键字    ： 过滤制定关键字进程信息
kill [-9] 进程号    ： 关闭进程，-9 表示强制
~~~

# 23. 主机状态监控

*查看系统资源*

~~~shell
top 
 -p:只显示某个进程
 -d:设置刷新时间
 -c:显示进程的完整命令
 -n:刷新次数
 -b:以非交互非全屏运行，配合-n重定向到文件
 -i:不显示闲置或无用的进程
 -u:查找指定用户启动的进程
~~~

> 交互式按键：`h`帮助，`c`完整命令 ，`f`选择要展示项， `M`根据驻留内存（RES）排序， `P`根据CPU排序， `T`根据时间排序， `E`切换顶部内存单位， `e`切换进程内存单位， `l`切换显示平均负载和启动时间， `i`不显示闲置或无用进程， `t`切换显示CPU状态信息， `m`切换显示内存信息 

*磁盘信息监控*

~~~shell
df [-h]          # -h 内存单位任人性化
~~~

*查看磁盘速率*

~~~shell
iostat num1 num2       # num1 刷新间隔  num2 刷新次数
~~~

*查看网络情况*

~~~shell
sar -n DEV num1 num2   # num1 刷新间隔  num2 刷新次数
~~~

# 24. 环境变量

*查看当前*

~~~shell
env
echo $PATH  # path信息
~~~

*修改环境变量*

~~~shell
#临时修改
export PATH=$PATH:路径
#永久修改
 #针对用户：在 ~/bashrc 中添加 “export PATH=$PATH:路径”
 #针对全部：在 /etc/profile 中添加
 最后执行 source 配置文件路径
~~~

# 25. linux 文件上传和下载

> - finalshell 图形化操作
>
> - ~~~shell
> yum -y install lrzsz
>   rz          # 上传
>   sz 文件     # 下载
>   ~~~
>
> 

# 26. 解压缩

> linux 常用压缩包格式 
> - `tar`   $.tar$        简单整合，无压缩
> - `gzip`  $.gz$ / $.tar.gz$ 

~~~shell
tar [-z -x -v -c -f -C] 参数
  -z：gzip模式      # 一般在开头
  -c: 创建压缩
  -v: 过程
  -x: 解压
  -f：指定压缩/解压的文件 
  -C：指定解压的路径   # 一般在最后
常用
tar -cvf test.tar 1.txt 2.txt 3.txt
tar -zcvf test.tar.gz 1.txt 2.txt 3.txt
tar -xvf test.tar -C /home/Desktop/
tar -zxvf test.gz -C /home/Desktop/
~~~

~~~shell
zip [-r] 参数...
  -r: 包含文件夹
unzip 参数1 [-d 参数2]
  -d：指定解压路径
~~~

# 27.远程登录

windows终端连接到linux

`ssh -p 22 root@centos`

## scp命令



# 28.shell

## 概述及入门

在脚本文件中首行

```
#!/bin/bash
```

执行方式

- `bash test.sh`

- `chmod +x ./test.sh`  #使脚本具有执行权限
  `./test.sh`  #执行脚本
- `source test.sh`   /    `. test.sh`

## 变量

### 系统预定义

> 常用系统变量
>
> `$HOME` `$PWD` `$SHELL` `$USER`
>
> 查看变量值
>
> echo $变量
>
> 显示所有变量 
>
> set

### 自定义变量

> 变量名=变量值，无空格
>
> 撤销变量：unset
>
> 静态变量 readonly

### 特殊变量

> `$n`:`$0`表示脚本名称，`$1`-`$9`为1-9的参数，10以上要用{},如`${10}`
>
> `$#` : 获取输入参数个数，可用于循环
>
> `$*`:获取所有参数，是一个整体
>
> `$@`:获取所有参数，是一个集合，可用于遍历
>
> `$?`: 最后一次执行的命令的返回状态，为0则正常

### 运算符

基本语法

> `$(())` 或`$[]`
>
> ```shell
> $((1+2))
> $[ 1+2 ]
> ```
>
> 命令替换：`$()` 或  ` (反引号)

### 条件判断

基本语法

> - test condition
> - [ condition ]       需要有空格
>
> ```shell
> test $a = hello
> [ $a = hello ]
> ```

条件判断

> - 字符串
>
>   `=`   /    `!=`
>
> - 数值判断
>
>   `-eq` 等于(equal)					`-ne` 不等于(not equal)
>
>   `-lt `小于(less than) 				`-le`小于等于(less equal)
>
>   `-gt` 大于(greater than)   		`-ge` 大于等于(greater equal)
>
> - 文件
>
>   `-r`  有读权限
>
>   `-w`	写权限
>
>   `-x`	执行权限
>
>   `-e` 	文件存在
>
>   `-f`	文件存在且是文件
>
>   `-d`	文件存在且是文件夹
>
>   ```shell
>   [ -w hello.sh ]	# 表示hello.sh是否可写
>   ```
>
> - 多条件判断
>
>   &&  ||  
>
>   ```shell
>   [ 1 -lt 2] && echo "ok" || echo "not ok"
>   ```
>
>   

### 流程控制

- if
>
>   - 单分支
>
>     ```shell
>     if [ 条件判断表达式 ]; then
>     	command
>     fi
>     # 或者
>     if [ 条件判断表达式 ]
>     then
>     	command
>     fi
>     ```
>     
>   - 多分支
>
>     ```shell
>     if condition1
>     then
>         command1
>     elif condition2 
>     then 
>         command2
>     else
>         commandN
>     fi
>     ```
>
>   如果使用 **((...))** 作为判断语句，大于和小于可以直接使用 **>** 和 **<**，如
>
>   ```shell
>   if (( 1 > 2 ))
>   then 
>   	...
>   fi
>   ```
>
>   

- for

  > ```shell
  > for var in item1 item2 ... itemN
  > do
  >     command1
  >     command2
  >     ...
  >     commandN
  > done
  > 
  > ```
  >
  > 

- while

> ```shell
> while condition
> do
>     command
> done
> 
> ```
>
> 无限循环
>
> ```shell
> while true
> do
> 	command
> done
> # 或者
> while :
> do 
> 	command
> done
> # 或者
> for (( ;  ; ))
> ```
>
> 

- until 

```shell
until condition
do
    command
done
```

- case

```shell
case 值 in
模式1)
    command1
    ...
    commandN
    ;;
模式2)
    command1
    ...
    commandN
    ;;
esac

```

_break_ 跳出

_continue_ 跳出当前

### 控制台读取

```bash
read [-p -t] 变量名
```

> `-p` 提示
>
> `-t` 等待时间（s）

```shell
read -t 7 -p "请输入您的名字" name
```

### 函数

系统函数

```bash
basename [string/path] [suffix]
dirname 文件绝对路径
```

> `basename` 用于去除文件名
>
> `dirname` 用于去除路径

自定义函数

```bash
[ function ] add()
{
	num=$[$1+$2]
	echo $num
} 
num=$(add $1 $2)
echo "和："$num
```

同主机不同用户发消息

`mesg`

```bash
[zhang@bogon scripts]$ mesg
is y
[zhang@bogon scripts]$ write lubb pts/1
你好，卢博斌
hbjyhvy
hello lubb
^C[zhang@bogon scripts]$
```

```bash
^C[zhang@bogon scripts]$ who
lubb     pts/1        2023-10-31 20:04 (192.168.88.1)
lubb     pts/3        2023-10-31 20:04 (192.168.88.1)
root     pts/4        2023-10-31 20:07 (192.168.88.1)
[zhang@bogon scripts]$ who am i
root     pts/4        2023-10-31 20:07 (192.168.88.1)
[zhang@bogon scripts]$
```

