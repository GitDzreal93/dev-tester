# Shell使用基础

### 什么时候使用shell？

> 1、Shell 适用于开发小工具和包装脚本
>
> 2、充当浇水语言的角色，仅仅调用其他程序，或是极少的数据处理
>
> 3、如果需要使用hash、嵌套array，建议用其他编程语言实现
>
> 4、对性能要求较高的场景，建议用其他语言实现

## 变量

### 定义变量

```shell
# a 和 1 中间不要留有空格，例如：不能写成 a = 1
a=1
echo $a
```

### 引用变量

```shell
a=1

# 写法一：
echo $a

# 写法二（推荐）：
echo ${a}

# 用${}的好处是字符串可以拼接
echo ${a}b  # => 输出 1b
```

### 引号

```shell
# 尽量使用双引号，双引号可以输出变量和转义，但是单引号完全是字面量
a=1
echo "${a}" # => 输出 1
echo '${a}' # => 输出 ${a}
```

### 环境变量

```shell
# 环境变量没有要求必须大写，但是建议写成大写

export A=1
# 或
declare -x B=2

# 也可以把普通变量导出为环境变量
C=3
export C
```

### 定义常量

```shell
# 常量一般写在文件开头
readonly CONST1
# 或
declare -r CONST2
```

### 默认全局变量

```shell
# 查看当前所在的目录
$PWD

# 上一次所在的目录
$OLDPWD

# PATH 环境变量
$PATH
```

### 特殊变量

```shell
# 脚本名称
$0

# 脚本参数，$1是第一个参数...
$1
$2
$3

# 参数个数
$#

# 上一个命令的结果，0是正常结束，非0是有异常
$?
```

## 基本语法

### 条件判断

shell 的判断可以用 `[]`，也可以用 `[[]]`，但建议用`[[]]`

```shell
if [[ $1 -eq "1" ]];then
    echo "ok"
elif [[ $1 -eq "2" ]];then
    echo "2333"
else
    echo "no"
```

判断可分为数字比较、字符串比较和文件判断

### 数字比较

判断相等：`=`、`==`、`-eq` （建议用`==`或者`-eq`，语义比较明确）

判断不等：`!=`、`-ne`

判断大于：`>`（需要双括号 `(())`）、`-gt`（`[[]]`即可）

判断小于：`<`（需要双括号）、`-lt`（`[[]]`即可）

```shell
nb=4

(( $nb > 4 )) && echo "yes" || echo "no"
# 或
[[ $nb -gt 4 ]] && echo "yes" || echo "no"
```

判断大于等于：`-ge`

判断小于等于：`-le`

### 字符串判断

判断相等：`=`、`==`、`-eq`

判断不等：`!=`、`-ne`

判断大于：`>`（需要双括号 `(())`）、`-gt`（`[[]]`即可）

判断小于：`<`（需要双括号）、`-lt`（`[[]]`即可）

判断大于等于：`-ge`

判断小于等于：`-le`

判断字符串不为null（判断长度不为0）：`-n`

判断字符串为null（判断长度为0）：`-z`

```shell
str="" 
# 未定义和长度为零的字符串都算空字符串
[[ -z $str ]] && echo "yes" || echo "not" # 输出 yes
[[ -n $str ]] && echo "yes" || echo "not" # 输出 not
```

### 文件判断

判断是否存在（不限制类型）：`-e`

判断**文件**是否存在：`-f`

判断**文件夹**是否存在：`-d`

### 逻辑运算符

取反：`!`

或：`||`

与：`&&`

```shell
a=1
[[ ! $a == 1 || 1=1 ]] && echo "yes" || echo "no" # 输出 => yes
[[ ! $a == 1 && 1=1 ]] && echo "yes" || echo "no" # 输出 => no
```

### case语句

```shell
# | 相当于 or，
# ;; 相当于break，必须有
# *) 相当于 default

case $1 in
 s|start)
        echo "start"
        ;;
 stop)
        echo "stop"
        ;;
 restart)
        echo "restart"
        ;;
 *)
        echo "Usage:[start|stop|reload]"
        exit 1
        ;;
esac
exit 0
```

### 循环

for 循环

```shell
# 写法一
# 输出当前文件夹下的所有文件
for f in `ls`;do
    echo $f
done

# 写法二
# 输出1-100的奇数
for i in {1..100..2};do
    echo $i
done

# 写法三
# 计算从1加到100
sum=0
for(( i=1;i<100;i++ ));do
		echo $(( sum+=i ))
done
```

while 循环

```shell
nb=0
while [[ $nb -lt 10 ]];do
    echo $(( nb+=1  ))
done
```

### 函数

**注意：函数的返回值只能是数字**

```shell
# 函数定义
function foo() {
    echo 'output'
}

# 函数调用
foo

# 函数参数
function sum(){
    echo $1
    echo $2
    res=$1+$2
    if [[ $res==3 ]];then
        return 0
    else:
        return -1
    fi
}

# 接收函数返回值用 $? ，函数的返回值只能是数字！
sum 1 2
echo $?  # 输出 => 0
```

### shell错误处理

`set -e`：报错时退出

`set -u`：检测未定义变量

`set -x`：调试执行，输出shell脚本的原始内容，编写jenkins的脚本常用

```shell
set -x
echo "hello world"

# 输出：
# + echo "hello world"  # +号 后面是源文件内容 输出格式由 PS4 的环境变量决定
# hello world
```

