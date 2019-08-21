# awk 的使用

### 简介

awk 主要用于统计和查看文本

### 语法格式

`awk [options] ‘command’ file`

### 常用参数（options）

- `-f` 调用脚本
- `-F` 指定**‘分隔符’**，指定多个分隔符(指定:和#作为分隔符): -F ‘[:#]’
- `-v` 定义变量

**常用特殊变量**

- `$0` 表示整个当前行
- `$1` 每行第一个字段
- `NF` 字段数量变量
- `NR` 行号
- `FILENAME` 文件名
- `OFS` 制表符

**特殊要点**

- `~` 匹配
- `!~` 不匹配
- `==` 等于，必须全等，精确比较
- `!=` 不等于，精确比较
- `&&` 逻辑与
- `||` 逻辑或

**常用命令：**

- `print` 打印字符

- 条件判断语句

  ```shell
  if(表达式){语句}else if(表达式){语句}else(表达式)(语句)
  ```
  
- 循环语句

  ```shell
  while(表达式){语句}
  for(变量 in 数组){语句}
  for(变量;条件;表达式){语句}
  do{语句}while(条件)
  ```

**程序结构**

- `BEGIN` 初始化代码块，在对每一行进行处理之前，初始化代码，主要是引用全局变量，设置FS分隔符，可以在绘制输出的表头时使用
- `//` 匹配代码块，可以是字符串或正则表达式
- `{}` 命令代码块，包含一条或多条命令
- `；` 多条命令使用分号分隔
- `END` 结尾代码块，在对每一行进行处理之后再执行的代码块，主要是进行最终计算或输出结尾摘要信息

用法：

```shell
awk -F '分隔符' 'BEGIN{程序开始时要做的命令}/pattern/{逐行匹配的时候要做的命令}END{程序执行完毕需要做的命令}' file
```

### 例子

以 `/etc/passwd` 文件为例：

passwd 文件说明：

用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell

##### 查看/etc/passwd下的用户名和口令，并添加前缀

```shell
awk -F':' '{print "user:"$1 "\t" "passwd:"$2}' /etc/passwd

# 输出：
# user:root       passwd:x
# user:daemon     passwd:x
# user:bin        passwd:x
# user:sys        passwd:x
# ...
```

##### 查看/etc/passwd下的用户名和口令，并添加表头

```shell
awk -F ':' 'BEGIN{print "user""\t""passwd"}{print $1 "\t" $2}END{print "---filename:---"FILENAME}' /etc/passwd

# 输出：
# user    passwd
# root    x
# daemon  x
# bin     x
# sys     x
# sync    x
# ...
# ---filename:---/etc/passwd
```

##### 查看/etc/passwd下的用户名和口令，并在每行开头打印出行号

```shell
awk -F ':' '{print NR " " "user:"$1 " " "passwd:"$2}' /etc/passwd

# 输出：
# 1 user:root passwd:x
# 2 user:daemon passwd:x
# 3 user:bin passwd:x
# 4 user:sys passwd:x
# 5 user:sync passwd:x
# 6 user:games passwd:x
# ...
```

##### 统计/etc/passwd文件的行数和字段数

```shell
awk -F ':' 'END{print "行数:"NR "\t" "字段数:"NF}' /etc/passwd
# 输出：
# 行数:37 字段数:7
```

##### 统计"bin"在/etc/passwd出现次数

```shell
awk -v dzreal=DZREAL -F ':' 'BEGIN{print "user" "\t" "operator"}{print $1 "\t" dzreal}' /etc/passwd

# 输出：
# user    operator
# root    DZREAL
# daemon  DZREAL
# bin     DZREAL
# sys     DZREAL
# sync    DZREAL
# ...
```

##### 显示第5行和第6行

```shell
awk -F: 'NR==5 || NR==6{print}'  /etc/passwd 

# 输出：
# sync:x:4:65534:sync:/bin:/bin/sync
# games:x:5:60:games:/usr/games:/usr/sbin/nologin
```

##### 打印第一列，并且匹配指定内容才显示

```shell
awk -F: '$1~/mail/{print $0}' /etc/passwd

# 输出：
# mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
```

##### 显示第一列不匹配的部分

```shell
awk -F: '$1!~/mail/{print $1}' /etc/passwd   

# 输出：
# root
# daemon
# bin
# sys
# sync
# ...
```

##### 输出前5个字段并使用"|"分隔

```python
awk -F: '{print $1,$2,$3,$4,$5}' OFS='|' /etc/passwd   

# 输出：
# root|x|0|0|root
# daemon|x|1|1|daemon
# bin|x|2|2|bin
# sys|x|3|3|sys
# ...
```

##### 计算当前目录下，普通文件的大小，使用KB作为单位，int是取整的意思

```shell
ls -l|awk 'BEGIN{sum=0} !/^d/{sum+=$5} END{print "total size is:",int(sum/1024),"KB"}' 

# 输出：
# total size is: 9908 KB
```

##### 统计当前目录下不同用户的普通文件的总数是多少？

```shell
ls -al|awk 'NR!=1 && !/^d/{sum[$3]++} END{for (i in sum) printf "%-6s %-5s %-3s \n",i," ",sum[i]}'

# 输出：
# root         3  
```

##### 统计Nginx的access.log，统计url以及url出现的次数

```shell
tail -1000 access.log | awk -F ' ' '!/"-"/{sum[$11]++}END{for(i in sum){print "url:"i,"count:"sum[i]}}' OFS="\t"

# 输出：
# url:"http://123.205.30.222//wuwu11.php" count:2
# url:"http://123.205.30.222//hm.php"     count:1
# url:"http://123.205.30.222"     count:1
# url:"http://123.205.30.222:80/invoker/readonly" count:1
# url:"http://123.205.30.222/l.php"       count:7
# url:"http://123.205.30.222//xx.php"     count:2
```

