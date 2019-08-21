# 必知必会的linux命令

### 更多命令去这查吧

> https://man.linuxde.net/ 

以下只记录我觉得我经常会用的命令

### 用户权限相关

```shell
useradd user		# 创建用户

useradd -d /home/user user	# 创建用户，并且为用户生成用户目录

userdel user		# 删除用户

userdel -r user	 # 删除用户以及用户目录

passwd user		# 创建用户密码

passwd -d user	# 删除用户密码

passwd root        # 更改root密码

chmod 777 file # 文件权限设为 -rwxrwxrwx（用户/组/其他），可读可写可执行，口诀：读4写2执行1，全部都有777

chown group:user file	# 将 file 的所属权限改为 user

chown -R group:user dir/	# 将dir/下所有文件，所属权限改为user
```

### 目录操作

```shell
mkdir dir/	# 生成目录
rmdir dir/	# 删除空目录
```

### 复制文件

```shell
# 本地文件复制
cp file file1  # 复制文件

cp file dir/  # 复制文件到某个目录下

cp -r dir/ dir1/  # 复制某个目录下的文件到其他目录下

# 远程文件复制
scp user@ip:file dir/   # 将远程机器的文件，复制到本机某个位置

scp -r user@ip:dir/ dir/  # 将远程机器的目录，复制到本机某个位置

rsync -avz src_path user@ip:dest_path # 本地到远端（-a:保留文件符号连接、权限、时间戳  -v打印详细信息 -z 压缩传输）

rsync -avz user@ip:desc_path src_path # 远端到本地

rsync -avzu src_path user@ip:dest_path  # 不覆盖目的地址的已修改文件（-u）
```

### 软连接和硬连接

```shell
ln -s srcFile softFile   # 创建软链接

ln srcFile hardFile      # 创建硬链接
```

**软连接和硬连接的区别**：

软连接：

- 保存了其代表的文件的绝对路径，是另外一种文件，在硬盘上有独立的区块，访问时替换自身路径。
- 删除了源文件之后，打开软链接文件会报错
- 修改软链接文件，源文件也会修改

硬连接：

- 与普通文件没什么不同，inode 都指向同一个文件在硬盘中的区块
- 删除了源文件之后，打开硬链接的文件不会报错
- 修改硬链接文件，源文件也会修改

### **查看和编辑文件**

**cat**

`cat  file`               # 显示文件内容

**more**

`more file`               # 分页显示文件内容

```shell
# 普通模式
Enter               # 向下n行，需要定义。默认为1行
Ctrl+F              # 向下滚动一屏
空格键               # 向下滚动一屏
Ctrl+B              # 返回上一屏
=                   # 输出当前行的行号
V                   # 调用vi编辑器
!命令               # 调用Shell，并执行命令 
q                   # 退出more

# 命令模式
:f                  # 输出文件名和当前行的行号
```

**less**

`less file`          # 分页显示文件内容（ less 比 more 更强大方便，且 less 在查看之前不会加载整个文件。）

```shell
# 命令行参数
-i                  # 忽略搜索时的大小写
-m                  # 显示类似more命令的百分比
-N                  # 显示每行的行号

# 普通模式
/字符串              # 向下搜索“字符串”的功能
?字符串              # 向上搜索“字符串”的功能
n                   # 重复前一个搜索（与 / 或 ? 有关，需要配合 / 或 ? 使用）
N                   # 反向重复前一个搜索（与 / 或 ? 有关，需要配合 / 或 ? 使用）
f                   # 向后翻一页
d                   # 向后翻半页
b                   # 向前翻一页
u                   # 向前滚动半页
y                   # 向前滚动一行
空格键               # 滚动一页    （和more相同）
回车键               # 滚动一行   （和more相同）
[pagedown]          # 向下翻动一页
[pageup]            # 向上翻动一页
q                   # 退出less 命令
```

**vim**

`vim file`                # 编辑文件或查看文件 

> 查看更多 [vim笔记](/page/other/use_vim.md)

```shell
# 命令行参数
-O                  # 分屏显示2个文件的内容（竖屏显示）
-o                  # 分屏显示2个文件的内容（横屏显示）

# 命令模式
:set nu             # 显示行号
:set nonu           # 不显示行号
:q                  # 退出（若文件有编辑改动，退出时会有询问）
:q!                 # 不保存退出
:w                  # 保存
:x                  # 保存并退出（假如文件被修改，才变更文件修改时间）
:wq                 # 保存并退出，并变更文件修改时间（不论文件是否真的有更改）
:wqa                # 全部保存并退出（用于同时打开查看多个文件）
:qa!                # 全部不保存并退出
:行号               # 跳转到相应行
:s/srcStr/newStr    # 将 当前行 的 第一个 srcStr用newStr替换
:s/srcStr/newStr/g  # 将 当前行 的 全部 srcStr用newStr替换
:s#srcStr#newStr    # 用 # 其实和 / 一样，都是用来分割，但是用#的话，字符串里如果有 / ， / 不会作为分隔符
:%s/srcStr/newStr   # 全文的srcStr用newStr替换

# 普通模式
a                   # 光标移动到下一个字符处，并进入编辑模式
i                   # 光标不移动，并进入编辑模式
o                   # 在本行下面另起一行，光标移动到下一行的开头，并进入编辑模式
ngg                 # 跳到n行
dd                  # 删除1行
ndd                 # 删除n行
yy                  # 复制1行
nyy                 # 复制n行
p                   # 粘贴
u                   # 撤销
[pagedown]          # 向下翻动一页
[pageup]            # 向上翻动一页
```

**tail**

`tail file`               # 显示文件最后10行

```shell
-n                  # 显示文件最后n行
-f                  # 动态显示文件末尾
```

**head**

`head file`               # 显示文件开头10行（查看日志用得最多）

```shell
-n                  # 显示文件开头n行
```

### 查找文件

```shell
find dir/ -name file	              # 根据文件名查找文件，文件名可使用通配符
find dir/ -user username            # 根据用户名查找文件，查找一个目录下该用户权限的文件
find dir/ -size +100M               # 根据文件大小查找文件，（数据块数，通常一块等于512）

locate              # 定位文件
locate file         # 根据数据库记录查找，有时候新创建的文件查找不到，有些Linux发行版不支持

which command       # 查找并显示给定命令的绝对路径

whereis             # 命令用来定位指令的二进制程序、源代码文件和man手册页等相关文件的路径。
```

### grep

```shell
搜索过滤：
grep                    # 超级强大的查找（过滤）工具
grep str file           # 查找文件下的str字符串
grep -rn str dir/       # 查看某个目录下的所有包含str的行，并显示行号（使用频率超高）
tail -f log | grep str  # 常用 tail 配合 grep ，用于动态查看log时，滤出关键信息，只显示匹配str的行
tail -f log | grep -v str               # 不显示str的行
tail -f log | grep str1 | grep str2     # 同时满足 str1 和 str2 才显示出来
tail -f log | grep -E "str1|str2"       # 满足 str1 或 str2 都会显示出来
tail -f log | grep str --color          # 搜出结果的同时高亮字符串
tail -f log | grep -A 3 str             # 打印匹配行以及后三行
tail -f log | grep -B 3 str             # 打印匹配行以及前三行
tail -f log | grep -C 3 str             # 打印匹配行以及前三行和后三行
tail -f log | grep -i Str               # 忽略大小写
```

### 查看系统相关命令

**查看系统信息**

```shell
# 打印当前系统相关信息
uname -a  # 全部输出

# 查看Linux内核版本
cat /proc/version

# 查看Linux版本
lsb_release -a
```

 **进程管理**

```shell
top                     # 类似windows的资源管理器，可以查看cpu、内存的占用情况
htop                    # 比top要高级好用，但可能需要安装
ps -ef | grep process   # 查看进程的使用情况（非常高频）
```

**查看网络相关**

```shell
# 查看网络相关
ifconfig                # 查看网口配置
netstat -ntpl | grep 端口号  # 查看端口占用情况
lsof -i:端口号           # 检查系统端口占用
hostname -i             # 查看本机ip地址
cat /etc/resolv.conf    # 查看本机dns服务器配置
ping ip(domain)         # 查看网络是否ping通，或也可以通过域名查询ip
telnet ip port          # 查看tcp服务是否通
```

**查看CPU/内存/磁盘空间占用**

```shell
ps auxw|head -1;ps auxw|sort -rn -k3|head -10   # CPU占用最多的前10个进程：  
ps auxw|head -1;ps auxw|sort -rn -k4|head -10   # 内存消耗最多的前10个进程
ps auxw|head -1;ps auxw|sort -rn -k5|head -10   # 虚拟内存使用最多的前10个进程
dstat -g -l -m -s --top-mem     # 动态查看当前占用内存最高的进程
dstat -g -l -m -s --top-cpu     # 动态查看当前占用cpu最高的进程
dstat -g -l -m -s --top-io      # 动态查看当前占用io最高的进程
df -lh                          # 磁盘空间使用率
du -sh                          # 一个目录下的所有文件大小
du -h --max-depth=1		# 查看一个目录下第一层子目录的大小
```

**包管理工具**

```shell
# yum
yum install xxx				    # yum安装程序
yum -y install xxx				    # 默认选y安装程序
yum upgrade xxx           # 更新指定的程序
yum remove xxx				    # yum卸载程序
yum deplist rpm 			      # 查看程序rpm依赖情况
yum list                  # 显示软件包信息
yum list xxx              # 显示指定程序包安装情况
yum search xxx            # 检查软件包的信息
yum localinstall rpm      # 安装本地rpm包

# rpm
rpm -ivh ***.rpm				   # 安装rpm包
rpm -e PACKAGE_NAME	   	   # 删除软件包
rpm -qa | grep xxx 		     # 查找xxx包有没有安装
rpm -qf /usr/sbin/httpd    # 查看某个文件属于哪个软件包，可以是普通文件或可执行文件，跟文件的绝对路径
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm	      # 安装程序包，有就升级，没有就安装
```



​    