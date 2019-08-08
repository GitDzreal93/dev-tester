# Mac 高效开发

### **前言**

我这里列举的，可能就是我自己用得最为高频的，未必是全的。市面上也有很多优秀的教程或文章去介绍mac的使用技巧，如有更多需要，大家可以自行查阅。

### 常用快捷键

#### **Ctrl**

1. `Ctrl + A`：移动到行首
2. `Ctrl + E`：移动到行尾
3. `Ctrl + K`：删除到行尾
4. `Ctrl + U`：删除到行头

#### **Delete**

1. `→ + Delete` 向右删除一个字母
2. `Delete` 向左删除一个字母

#### **Option**

1. `Option + ←`：光标向左移动一个单词
2. `Option + →`：光标向右移动一个单词
3. `Option + Delete`：删除一个单词

#### **Shift**

在某个位置点击光标并**按住 shift 键不松开**，再去另一个位置点击一次，就可以**选中两次点击位置之间的文本内容**。



### item2 奇淫技巧

#### **选择文本**（选中即为复制）

1. 双击：选中单词
2. 三击：选中整行
3. 四击：智能选择

`Command + Option` 还可以选中矩形范围内的文本。

#### **标签页和分屏**

`Command + T` : 新建一个 Tab

`Command + 序号`:  切换Tab

`Command + [` : 切换到左侧标签页

 `Command + ]`：切换到右侧标签页

`Command + D`：水平切分，切分出一左一右两个 Pane

`Command + Shift + D`：垂直切分，切分出一上一下两个Pane

`Command + Ctrl + 方向键`：调整Pane的大小

`Command + Option + 方向键`:  切换Pane

`^old^new`: 将上一条命令中，匹配了old的部分替换成new（适用于上一条命令输错了，又无需去重新输入修改）

#### 免密登陆远程服务器

以下两种方法任选其一

##### 方式一：

使用`ssh-copy-id`工具来实现免密登陆

**安装**：`brew install ssh-copy-id`

**使用**：`ssh-copy-id 账号:密码@IP:端口`

**原理**：把自己的公钥（默认使用 `~/.ssh/id_rsa.pub` 这个文件中的内容）复制到目标服务器的 `~/.ssh/authorized_keys` 文件内。

**添加别名**：

编辑 `~/.ssh/config` 文件，添加以下内容：

```shell
Host remotehost
    HostName 123.206.3.4
    User root
    Port 22
    IdentityFile ~/.ssh/id_rsa
```

**登陆远程服务器**：

```shell
# 使用别名前
ssh root@1.2.3.4

# 使用别名后
ssh remotehost
```

##### 方式二：

使用命令自动登陆远程服务器

**设置**：

![](http://ww1.sinaimg.cn/large/69b577d4gy1g5pudkp48nj21f60vogt7.jpg)

**脚本配置**：

```shell
#!/usr/bin/expect -f
set host your_host
set user your_account
set password your_password
#set timeout -1

spawn ssh $user@$host
expect "*assword:*"
send "$password\r"
interact
expect eof
```

**登陆**：

![](http://ww1.sinaimg.cn/large/69b577d4gy1g5puvcopypj20h809u7cx.jpg)

##### SSH全局配置

```shell
Host *
    ForwardAgent yes
    ServerAliveInterval 10
    ServerAliveCountMax 10000
    TCPKeepAlive no
    ControlMaster auto
    ControlPath ~/.ssh/%h-%p-%r
    ControlPersist 4h
    Compression yes
```

ServerAliveInterval 和 ServerAliveCountMax：客户端定期向服务端发送心跳包



### Mac 必装软件

- [Alfred](https://www.waitsun.com/alfred-3-7-931b.html) 效率神器
- [Homebrew](https://my.oschina.net/Rayn/blog/2876725) 安装mac软件包
- [Parallels](https://www.waitsun.com/parallels-desktop-13-3-1-43365.html) 最强虚拟机，可以跑windows程序