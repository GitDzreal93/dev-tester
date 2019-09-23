# Python面试题

## 干货

[github上 5280+star 的面试题](https://github.com/taizilongxu/interview_python#1-python的函数参数传递)

[python面试题 300 道](https://www.cnblogs.com/wupeiqi/p/9078770.html)

[django 后端开发面试题](https://blog.csdn.net/ayocross/article/details/56509840?utm_source=itdadao&utm_medium=referral)

[leetcode](https://leetcode-cn.com/problemset/top/)

## 自己收集的题

### 1、如何去除列表中的重复元素

【题目】给出一个列表：lis = [4, 2, 1, 3, 4, 2, 3, 1, 3, 2, 2, 2]，去除列表中的重复元素。

```python
lis = [4, 2, 1, 3, 4, 2, 3, 1, 3, 2, 2, 2]

# 解法1: 利用set去重，会改变列表顺序
lis1 = list(set(lis))

# 解法2: 
lis2 = []
for i in lis:
  if i not in lis2:
    lis2.append(i)
```

### 2、八大排序算法合集

【题目】给出一个列表： lis = [1, 2, 5, 9, 10, 3,  4,  8, 77, 100]

#### 选择排序

```python
def select_sort(lists):
    '''
    选择排序（升序）【不稳定排序】
    原理：
    1、给定一个列表，经过第一轮比较后，找到最小值，与第一个位置交换；
    2、接着对不包括第一个元素的剩下的元素，找到最小值，与第二个位置交换；
    3、重复该过程，直到进行比较的记录只有一个为止

    以 list = [5, 4, 2, 1, 3] 为例：
    第一次排序后： [1, 4, 2, 5, 3]
    第二次排序后： [1, 2, 4, 5, 3]
    第三次排序后： [1, 2, 3, 5, 4]
    第四次排序后： [1, 2, 3, 4, 5]
		
		时间复杂度：O(n**2)
		空间复杂度：O(1)
		
    :param lists:
    :return: lists
    '''
    count = len(lists)
    for i in range(0, count):
        min = i
        for j in range(i + 1, count):
            if lists[min] > lists[j]:
                min = j
        lists[min], lists[i] = lists[i], lists[min]
    return lists
  

# 调用选择排序
select_sort_list = select_sort(lis)
print(select_sort_list)
```

#### 插入排序

```python
def insert_sort(lists):
    '''
    插入排序（升序）【稳定排序】
    原理：
    1、假设初始时假设第一个记录，自成一个有序序列，其余的记录为无序序列；
    2、从第二个记录开始，按记录大小，依次插入之前的有序序列中；
    3、直到最后一个记录插入到有序序列中为止

    以 list = [5, 4, 2, 1, 3] 为例：
    第一步，插入5之后: [5], 4, 2, 1, 3
    第二步，插入4之后: [4, 5], 2, 1, 3
    第三步，插入2之后: [2, 4, 5], 1, 3
    第四步，插入1之后: [1, 2, 4, 5], 3
    第五步，插入3之后: [1, 2, 3, 4, 5]

    时间复杂度: O(n) ~ O(n**2)  平均: O(n**2)
    空间复杂度: O(1)

    :param lists:
    :return: lists
    '''
    count = len(lists)
    for i in range(1, count):
        key = lists[i]
        j = i - 1
        while j >= 0:
            if lists[j] > key:
                lists[j+1] = lists[j]
                lists[j] = key
            j -= 1
    return lists
  
  
# 调用插入排序
insert_sort_list = insert_sort(lis)
print(insert_sort_list)
```

#### 冒泡排序

```python
def bubble_sort(lists):
'''
    冒泡排序（升序）【稳定排序】
    原理：
    1、从第一个元素开始，开始依次对相邻的两个元素进行比较，当后面的元素大于前面的元素时，交换二者位置；
    2、进行一轮比较之后，最大的元素将在序列尾部（最后一位）；
    3、然后对（n-1）个元素再进行第二轮比较，最大元素将在序列倒数第二位；
    4、重复该过程，直至只剩下最后一个元素为止，最后的元素就是最小值，排在序列首位

    以 list = [5, 4, 2, 1, 3] 为例：
    第一轮排序: [4, 2, 1, 3, 5]
    第二轮排序: [2, 1, 3, 4, 5]
    第三轮排序: [1, 2, 3, 4, 5]

    时间复杂度： O(n) ~ O(n**2)  平均：O(n**2)
    空间复杂度： O(1)

    :param lists:
    :return lists:
    '''
    for i in range(len(lists)-1):
        for j in range(len(lists)-i-1):
            if lists[j] > lists[j+1]:
                lists[j], lists[j+1] = lists[j+1], lists[j]
    return lists
  
# 调用冒泡排序
bubble_sort_list = bubble_sort(lis)
print(bubble_sort_list)
```

#### 归并排序

```python
def merge(left, right):
    i, j = 0, 0
    result = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result


def merge_sort(lists):
    '''
    归并排序（升序）【稳定排序】
    
    原理：
    利用分治法和递归法去解决问题
    1、第一步，划分字表；
    2、第二步，合并半字表
    
    时间复杂度：O(nlogn)
    空间复杂度：O(1)
    
    :param lists: 
    :return lists: 
    '''
    if len(lists) <= 1:
        return lists
    num = len(lists) / 2
    left = merge_sort(lists[:num])
    right = merge_sort(lists[num:])
    return merge(left, right)
  
  
# 调用归并排序
merge_sort_list = merge_sort(lis)
print(merge_sort_list)
```

#### 快速排序

```python
def quick_sort(lists, left, right):
    '''
    快速排序（升序）【不稳定】
    原理：
    分治递归，给定一组序列，把序列分为两部分，前部分的所有元素比后部分的所有元素小，然后再对前后两部分进行快速排序，递归该过程，直到所有记录
    均有序为止。分三步走：
    (1)分解，array[m,...,n] 划分成 array1[m,..,k] 和 array2[k+1,...,n], 其中 array1 所有元素 < array2 所有元素
    (2)递归求解，递归array1, array2
    (3)合并，array[m,...,n] 自动排好序

    :param lists:
    :param left:
    :param right:
    :return lists:
    '''
    if left >= right:
        return lists
    key = lists[left]
    low = left
    high = right
    while left < right:
        while left < right and lists[right] >= key:
            right -= 1
        lists[left] = lists[right]
        while left < right and lists[left] <= key:
            left += 1
        lists[right] = lists[left]
    lists[right] = key
    quick_sort(lists, low, left - 1)
    quick_sort(lists, left + 1, high)
    return lists
 
# 快速排序调用
quick_sort_list = quick_sort(lis, 0, len(lis) - 1)
print(quick_sort_list)
```

#### 希尔排序

```python
def shell_sort(lists):
    '''
    希尔排序（升序）【不稳定排序】
    原理：
    也叫缩小增量排序，将待排序列分成多个子序列，每个子序列元素个数相对较少，对各个子序列分别进行直接插入排序，待整个排序序列"基本有序后"，再对所有元素进行一次直接插入排序

    时间复杂度：O(nlogn) 最差 O(n**s)(1<s<2)
    空间复杂度：O(1)

    :param lists:
    :return lists:
    '''
    count = len(lists)
    step = 2
    group = int(count / step)
    while group > 0:
        for i in range(0, group):
            j = i + group
            while j < count:
                k = j - group
                key = lists[j]
                while k >= 0:
                    if lists[k] > key:
                        lists[k + group] = lists[k]
                        lists[k] = key
                    k -= group
                j += group
        group /= step
    return lists
  
# 希尔排序调用
shell_sort_list = shell_sort(lis)
print(shell_sort_list)
```

#### 堆排序

```python
def adjust_heap(lists, i, size):
    lchild = 2 * i + 1
    rchild = 2 * i + 2
    maxs = i
    if i < size / 2:
        if lchild < size and lists[lchild] > lists[maxs]:
            maxs = lchild
        if rchild < size and lists[rchild] > lists[maxs]:
            maxs = rchild
        if maxs != i:
            lists[maxs], lists[i] = lists[i], lists[maxs]
            adjust_heap(lists, maxs, size)


def build_heap(lists, size):
    for i in range(0, (size / 2))[::-1]:
        adjust_heap(lists, i, size)


def heap_sort(lists):
    '''
   堆排序（升序）【不稳定排序】
   原理：
  （Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。
   步骤：
   1、构建堆
   2、交换堆顶元素和最后一个元素的位置
   
   时间复杂度：O(nlogn)
   
   :param lists:
   :return lists:
   '''
    size = len(lists)
    build_heap(lists, size)
    for i in range(0, size)[::-1]:
        lists[0], lists[i] = lists[i], lists[0]
        adjust_heap(lists, 0, i)
        
# 堆排序调用
heap_sort_list = heap_sort(lis)
print(heap_sort_list)
```

#### 基数排序（桶排）

```python
import math
def radix_sort(lists, radix=10):
    '''
    基数排序（桶排序）（升序）【稳定性排序】
    基数排序（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或bin sort，顾名思义，它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为O (nlog(r)m)，其中r为所采取的基数，而m为堆数，在某些时候，基数排序法的效率高于其它的稳定性排序法。

    时间复杂度：O(nlog(r)m) r是基数，m是堆数

    :param lists:
    :param radix:
    :return lists:
    '''
    k = int(math.ceil(math.log(max(lists), radix)))
    bucket = [[] for i in range(radix)]
    for i in range(1, k+1):
        for j in lists:
            bucket[j/(radix**(i-1)) % (radix**i)].append(j)
        del lists[:]
        for z in bucket:
            lists += z
            del z[:]
    return lists

```

### 3、斐波那契数列的python实现

【题目】 斐波那契数列（Fibonacci sequence），又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、…… 基于python用多种方式，生成费波拉契数列。

```python
# （1）递归法 返回 idx 位的数值，缺点：只能返回某个值
def fib_recursion(idx):
    if idx <= 2:
        return 1
    else:
        return fib_recursion(idx-1) + fib_recursion(idx-2)

# （2）迭代法 返回 idx 位之前的fib数列
def fib_iteration(idx):
    lst = []
    n,a,b = 0,0,1
    while n < idx:
        lst.append(b)
        a,b = b, a+b
        n += 1
    return lst

# （3）生成器法
def fib_generator(idx):
    n,a,b = 0,0,1
    while n < idx:
        yield b
        a,b = b, a+b
        n += 1

if __name__ == '__main__':
    idx = 6
    numb = fib_recursion(idx)
    print(numb)

    lst = fib_iteration(idx)
    print(lst)

    lst1 = fib_generator(idx)
    print(list(lst1))
```

### 4、丑数计算

> 转载自：https://blog.csdn.net/qq_34364995/article/details/80680354
>
> 遵循[ CC 4.0 by-sa ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议

【题目】丑数是只包含质因数2，3，5的正整数（1也是特殊的丑数），请编写程序，找出第n个丑数

*示例*：

```shell
输入：n=10
输出：12
解释：1，2，3，4，5，6，8，9，10，12 是前10个丑数
```

*说明*：1是丑数，n不超过1690

*程序*：

```python
class Solution:
    def nthUglyNumber(self, n):
        """
        原理：
        动态规划思想。
        后面的丑数一定是由前面的丑数乘以2、3或5得到。
        所以第n个丑数一定是由前n-1个数中的某3个丑数（分别记为index2、index3、index5）分别乘以2、3或者5得到的数中的最小数，index2，index3，index5有个特点，即分别乘以2、3、5得到的数一定含有比第n-1个丑数大（可利用反证法：否则第n-1个丑数就是它们当中的一个）最小丑数，即第n个丑数由u[index2]*2、u[index3]*3、u[index5]*5中的最小数得出。让它们分别和第n个丑数比较，若和第n个丑数相等，则更新它们的值。注：一次最少更新一个值（如遇到第n个丑数是6时，index2和index3都要更新）。
        :type n: int
        :rtype: int
        """
        if n <= 0:
            return False
        t1 = 0
        t2 = 0
        t3 = 0
        res = [1]
        while len(res) < n:
            res.append(min(res[t1]*2, res[t2]*3, res[t3]*5))
            if res[-1] == res[t1]*2:
                t1 += 1
            if res[-1] == res[t2]*3:
                t2 += 1
            if res[-1] == res[t3]*5:
                t3 += 1
        return res[-1]
```

### 5、如何不通过循环，输出1到100

【题目】如何不通过循环，输出1到100

```python
def prints(n):
    '''
    可以通过递归替代循环
    :param n: 
    :return: 
    '''
    if (n > 0):
        prints(n-1)
        print(n)

if __name__ == '__main__':
    prints(100)
```

### 6、如何对字符串进行反转

【题目】要求不使用任何系统方法，且时间复杂度最小

```python
def reverse_str(input_str):
    ch=list(input_str)
    lens=len(ch)
    i=0
    j=lens-1
    while i < j:
        tmp= ch[i]
        ch[i] =ch[j]
        ch[j]=tmp
        i+=1
        j-=1
    return ''.join(ch)
  
#tips 假如可以使用系统方法，如何实现？
new_str = old_str[::-1]
```

### 7、如何对单词反转

【题目】把"hello world" 反转成"world hello"

```python
def reverse_word(test_str):
    ch = test_str.split(" ")
    lens = len(ch)
    i = 0
    j = lens - 1
    while i < j:
        tmp = ch[i]
        ch[i] = ch[j]
        ch[j] = tmp
        i += 1
        j -= 1
    return ' '.join(ch)
```

### 8、两个列表如何生成一个对应的字典

【题目】l1=[‘Ben’, ‘Funn’, ‘Mike’, ‘Ronaldo’]，l2= [17, 18, 18, 16]，合并两个列表，生成 d = {‘Ben’: 17, ‘Funn’: 18, ‘Mike’: 18, ‘Ronaldo’: 16}

```python
l1 = ['Ben', 'Funn', 'Mike', 'Ronaldo']
l2 = [17, 18, 18, 16]
d = dict(zip(l1, l2))
# 结果: d =  {'Ben': 17, 'Funn': 18, 'Mike': 18, 'Ronaldo': 16}

# 反之，一个字典如何生成两个列表？
# 解法：用zip(*)函数，zip(*)会生成一个zip对象，即把[(a,b),(c,d),(e,f)]拆分成[(a,c,e)(b,d,f)],（此处只是伪代码帮助理解，实际上可能实现的过程并不是如此）
l1 = list(list(zip(*d.items()))[0])
l2 = list(list(zip(*d.items()))[1])

# 结果：
# l1 = ['Ben', 'Funn', 'Mike', 'Ronaldo']
# l2 = [17, 18, 18, 16]
```

### 9、统计字符串中每个字母出现的次数

【题目】统计字符串s='I love python'中，每个字母出现的次数

```python
s = 'I love python'
# 解法1
from collections import Counter
d = dict(Counter(s))
# 结果：d = {'I': 1, ' ': 2, 'l': 1, 'o': 2, 'v': 1, 'e': 1, 'p': 1, 'y': 1, 't': 1, 'h': 1, 'n': 1}
# 假如说想要去掉空格的统计,只需要 d.pop(' ')即可，结果是：
# {'I': 1, 'l': 1, 'o': 2, 'v': 1, 'e': 1, 'p': 1, 'y': 1, 't': 1, 'h': 1, 'n': 1}

# 解法2
d = {}
for i in s:
    d[i] = s.count(i)

# 结果：d = {'I': 1, ' ': 2, 'l': 1, 'o': 2, 'v': 1, 'e': 1, 'p': 1, 'y': 1, 't': 1, 'h': 1, 'n': 1}

# 解法3
from collections import defaultdict
s = 'I love python'
d = defaultdict(int)
for i in range(len(s)):
    d[s[i]] += 1
# 结果：defaultdict(<class 'int'>, {'I': 1, ' ': 2, 'l': 1, 'o': 2, 'v': 1, 'e': 1, 'p': 1, 'y': 1, 't': 1, 'h': 1, 'n': 1})   
```

### 10、对某英文文章的单词，进行词数统计

【题目】假设有一篇英文文章：English.txt，求出现次数最高的10个单词，并且这10个单词中每个单词出现的频数

```python
# 采用正则表达式进行分词操作，再用collections库的Counter进行词频统计
import re
from collections import Counter

txt = open('English.txt', 'r').read()
#用正则表达式的split对txt整个字符串的单词进行分割(\W+表示特殊字符，差不多和[^a-zA-Z0-9_]等价，而\w+和[a-zA-Z0-9_]等价)，split返回值是数组，用法和str.split差不多
res = re.split('\W+', txt)		
all_counter = Counter(res) 	#用Counter统计出现单词的次数
top10 = all_counter.most_common(10)			#对出现次数最多的10个单词进行统计
print(top10)
# 结果：[('in', 6), ('to', 5), ('subway', 5), ('the', 4), ('a', 3), ('app', 3), ('on', 3), ('city', 3), ('by', 3), ('have', 3)]

# tips：假如是中文文档，可以用jieba分词
```

### 11、字典排序

【题目】有一个学生分数的字典 `d={"stu1":100, "stu2":90, "stu3":40}`，请按分数进行排序（从大到小）

```python
# 思路：用sorted()函数去派去，sorted()函数适用于可迭代对象：sorted(iterable, cmd=None, key=None, reverse=False)
# cmp 是用于比较的函数，比较什么由key 决定
# key 是列表元素的某个属性或函数进行作为关键字，有默认值，迭代集合中的一项
# reverse = True 表示降序， reverse= False 表示升序

# 解法1 化为元祖，根据元祖中分数，去进行排序，缺点:key和value的值会颠倒
tup = zip(all_stu_dict.values(), all_stu_dict.keys())
rank = sorted(tup, reverse=True)

# 解法2 直接用sorted，并且制定规则来排序
rank = sorted(all_stu_dict.items(), key=lambda x:x[1], reverse=True)
```

### 12、如何快速找到多个字典的公共键

【题目】

第一轮：{‘C罗’:3, ‘格里兹曼’:2, ‘内马尔’:1, ‘博格巴’:1}

第一轮：{‘C罗’:2, ‘格里兹曼’:0, ‘内马尔’:0,’梅西’:2}

第一轮：{‘C罗’:4, ‘内马尔’:2, ‘库蒂尼奥’:2, ‘姆巴佩’:2}

统计每轮比赛都有进球的球员

```python
d1 = {'C罗': 3, '格里兹曼': 2, '内马尔': 1, '博格巴': 1}
d2 = {'C罗': 2, '格里兹曼': 0, '内马尔': 0, '梅西': 2}
d3 = {'C罗': 4, '内马尔': 2, '库蒂尼奥': 2, '姆巴佩': 2}

# 解法1  迭代（效率最低）
shooter_list = []
for shooter in d1:
    if shooter in d2 and shooter in d3:
        shooter_list.append(shooter)

# 解法2 利用map分别取出每轮进球的球员名字，再用reduce对每轮都进球的球员进行取交集
all_shooters = map(dict.keys, [d1, d2, d3])
shooter_list = reduce(lambda a, b: a & b, all_shooters)
```

### 13、python的字典合并

> 主要思路：
>
> - 借助 dict(list(d1.items()) + list(d2.items()) 的方法,注意：list(d1.items()) + list(d2.items())拼成一个新的**列表**
> - 借助字典的 update() 方法：d1.update(d2)
> - 借助字典的dict(d1, **d2)方法
> - 借助字典的常规处理方法（采用迭代的方式）

### 14、python的socket编程

> 【解析】
>
> 服务端要做的工作：
>
> - 创建socket
> - 绑定端口
> - 监听端口
> - 接收消息
> - 发送消息
>
> 客户端要做的工作：
>
> - 创建socket
> - 连接socket
> - 发送消息
> - 接收消息
>

```python
# 客户端
import socket
client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
client.connect(('127.0.0.1', 8000))
while True:
    re_data = input()
    client.send(re_data.encode("utf8"))
    data = client.recv(1024)
    print(data.decode("utf8"))
```

```python
# 服务端
import socket
import threading

server = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
server.bind(('0.0.0.0', 8000))
server.listen()


def handle_sock(sock, addr):
    while True:
        data = sock.recv(1024)
        print(data.decode("utf8"))
        re_data = input()
        sock.send(re_data.encode("utf8"))

#获取从客户端发送的数据
#一次获取1k的数据
while True:
    sock, addr = server.accept()

    #用线程去处理新接收的连接(用户)
    client_thread = threading.Thread(target=handle_sock, args=(sock, addr))
    client_thread.start()
```

### 15、判断一个字符串中的括号是不是成对出现的

【题目】给出字符串 s = “12312{}{111{222}}”，判断字符串s中的 括号 是不是成对出现的

```python
s = "12312{}{111{222}}"

def judge_brackets(target_str):
    left_list = []
    right_list = []
    for i in target_str:
        if i == '{' or i == '(' or i == '[':
            left_list.append(i)
        if (i == '}' or i == ')' or i == ']'):
            if left_list != []:
                left_list.pop()
            else:
                right_list.append(i)

    if left_list != [] or right_list != []:
        return "括号不是成对出现的"

    return "括号是成对出现的"


p = judge_brackets(s)
print(p)
```

### 16、python的赋值、浅拷贝和深拷贝的区别

**赋值** 只是复制了新对象的引用，不会开辟新的内存空间。详细请看例子：

```python
# 不可变对象
a = "abcde"
b = a 
print(id(a), id(b))
# 结果： 2274644971848 2274644971848 两个变量的id是一致的
a = "xyz"
print((id(a), id(b)), (a, b))
# 结果：(1970541080680, 1970540237128) ('xyz', 'abcde')，因为字符串是不可变对象，改变之后要开辟新的内存空间，相当于创建了新的内存区块，并且a引用了新的内存区块，所以id这时候就不相等了。

# 可变对象
a = [1,2,3,4,5]
b = a
print(id(a), id(b))
# 结果：1861111211144 1861111211144
c.append(6)
print((id(a), id(b)), (a, b))
# 结果：(1861111211144, 1861111211144) ([1, 2, 3, 4, 5, 6], [1, 2, 3, 4, 5, 6])，因为列表是可变对象，添加元素之后，并不会开辟新的内存区块，所以这时候两个变量仍然还是指向同一区块，id是相等的
```

**浅拷贝** 创建一个新的组合对象(所以id会不同)，这个新对象与原对象共享内存中的子对象。能够拷贝所有元素的引用，但不拷贝元素的对象。

```python
import copy

a = "abcde"
b = copy.copy(a)
print((id(a), id(b)), (a, b))
# 结果：(1911642247496, 1911642247496) ('abcde', 'abcde')，这里有一个十分重要的点，int、string等不可变对象，假如值特别小的话，python内部有一个优化机制，让其不会开辟新的内存空间，所以这种浅拷贝，id是相等的

a = [1,2,3,4,5]
b = copy.copy(a)
print((id(a), id(b)), (a, b))
# 结果：(2005329935816, 2005329935752) ([1, 2, 3, 4, 5], [1, 2, 3, 4, 5])，不可变对象的浅拷贝，拷贝了所有元素的引用，id是不相同的
a.append(6)
print((id(a), id(b)), (a, b))
# 结果：(2687650790856, 2687650790792) ([1, 2, 3, 4, 5, 6], [1, 2, 3, 4, 5])，因为 int 也是不可变对象，进行浅拷贝之后，新增元素也会开辟新的内存空间，所以两个变量的值和id都是不同的。

a = [1,2,3,4,[5,6,7]]
b = copy.copy(a)
print((id(a), id(b)), (a, b))
# 结果：(2773194341768, 2773194763784) ([1, 2, 3, 4, [5, 6, 7]], [1, 2, 3, 4, [5, 6, 7]])
a[4].append(8)
print((id(a), id(b)), (a, b))
# 结果：(2015617514888, 2015617937032) ([1, 2, 3, 4, [5, 6, 7, 8]], [1, 2, 3, 4, [5, 6, 7, 8]])，拷贝到元素的引用，a[4]指向一个数组(不可变对象)，当这个元素的值改变时，没有开辟新的内存空间，所以这个元素还是指向同一个内存区块。
```

**深拷贝** 创建一个新的组合对象，同时递归地拷贝所有子对象，新的组合对象与原对象没有任何关联。虽然实际上会共享不可变的子对象，但不影响它们的相互独立性

```python
import copy


a = "abcde"
b = copy.deepcopy(a)
print((id(a), id(b)), (a, b))
# 结果：(1684255592776, 1684255592776) ('abcde', 'abcde')，和浅拷贝一样

a = [1,2,3,4,5]
b = copy.deepcopy(a)
print((id(a), id(b)), (a, b))
# 结果：(1684258061768, 1684258061704) ([1, 2, 3, 4, 5], [1, 2, 3, 4, 5])，和浅拷贝差不多
a.append(6)
print((id(a), id(b)), (a, b))
# 结果：(1684258061768, 1684258061704) ([1, 2, 3, 4, 5, 6], [1, 2, 3, 4, 5])，和浅拷贝差不多

a = [1,2,3,4,[5,6,7]]
b = copy.deepcopy(a)
print((id(a), id(b)), (a, b))
# 结果：(1684258485064, 1684258061768) ([1, 2, 3, 4, [5, 6, 7]], [1, 2, 3, 4, [5, 6, 7]])
a[4].append(8)
print((id(a), id(b)), (a, b))
# 结果：(1684258485064, 1684258061768) ([1, 2, 3, 4, [5, 6, 7, 8]], [1, 2, 3, 4, [5, 6, 7]])，因为递归拷贝了子对象，所以这个例子可以看出和浅拷贝的区别所在，因为深拷贝递归的把元素的子元素也拷贝了一份，所以新的组合对象和原组合对象实际上看起来是不再有关联的
```

### 17、返回字符串中第一个不重复的字母和位置

```python
def first_char(str):
    d = {}
    for i in range(len(str)):
        # 累计字符的出现次数
        if str[i] in d:
            d[str[i]] += 1
        # 只出现一次，key对应的value就记1次
        else:
            d[str[i]] = 1
    for i in range(len(str)):
        if d[str[i]] == 1:
            return '第一个不重复的字符串是{},索引是{}'.format(str[i], i)

    return "没有不重复的字符串"


if __name__ == '__main__':
    s = "wwqqoogg"
    res = first_char(s)
    print(res)
```

### 18、求一个数字列表里，相邻两数乘积最高的值，及这两个数分别是多少?

【题目】给出L = [2, 4, 6, 3, 9, 11, -12] , 求相邻两数乘积最大的值，并返回这两个相乘的数。

```python
L = [2, 4, 6, 3, 9, 11, -12]

def multi_max(lis):
    L1 = []
    for i in range(1, len(lis)):
        j = i - 1
        multi = lis[i] * lis[j]
        L1.append((multi, lis[i], lis[j]))
        # max函数是取第一个元素做比较，也就是说按multi最大来排
    return max(L1)

multi, left, right = multi_max(L)
print(multi, left, right)
```

### 19、一行代码实现字典的key和value反转

【题目】`d = {"name":1, "age":2}`，反转字典 d，反转成`{1:"name", 2:"age"}`

```python
d = {"name":1, "age":2}
d1 = {age:name for name,age in d.iterms()}
```

### 20、如何求字符串例的最长回文子串

待施工

### 21、python如何实现单例模式

```python
# 装饰器中@语法糖的意思：@foo <=等价=> foo = decorator(foo)

instances = {}

def singleton(cls):
    def get_instance(*args, **kwargs):
        cls_name = cls.__name__
        if not cls_name in instances:
            instance = cls(*args, **kwargs)
            instances[cls_name] = instance
        return instances[cls_name]
    return get_instance
  
@singleton
class User:
    _instance = None
    
    def __init__(self, name):
        self.name = name
```

### 22、为什么模块称为天然的单例模式？

> 更多：http://funhacks.net/2017/01/17/singleton/
>
> 解释：
>
> 因为模块在第一次导入时，会生成 `.pyc` 文件，当第二次导入时，就会直接加载 `.pyc` 文件，而不会再次执行模块代码。因此，我们只需把相关的函数和数据定义在一个模块中，就可以获得一个单例对象了。

```python
# mysingleton.py
class My_Singleton(object):
    def foo(self):
        pass
my_singleton = My_Singleton()
```

```python
from mysingleton import my_singleton
my_singleton.foo()
```

### 24、Python处理排列组合

> 排列组合数的计算公式：
>
> 排列 
>
> （考虑顺序，比如（1，2）不等于（2，1））
>
> 计算全排列有多少种情况：A23 = 3!/(3-2)! = 6
>
> 组合
>
> （不考虑顺序，比如（1，2）和（2，1）都只算一种）
>
> 计算组合有多少种情况：C23 = 3!/(3-2)!2! = 3

用库：

**计算排列组合各有多少种情况**

```python
from scipy.special import comb, perm

# 排列
perm(3, 2)
# 输出 => 6.0

# 组合
comb(3, 2)
# 输出 => 3.0
```

**计算排列组合，全部结果展开**

```python
from itertools import combinations, permutations

permutations([1, 2, 3], 2)
// <itertools.permutations at 0x7febfd880fc0>
                # 可迭代对象
# 计算排列结果
list(permutations([1, 2, 3], 2))
# 输出 => [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]

# 计算组合结果
list(combinations([1, 2, 3], 2))
# 输出 => [(1, 2), (1, 3), (2, 3)]
```

### 25、将某个数字列表中的元素拼成一个最大的数

例如给定：`lists = [98, 77, 981, 2221, 3322]`，定义一个方法，输出`989817733222221`

```python
# -*- coding:utf-8 -*-

def spile_max(lists):
  '''
  利用冒泡排序去解决，比较相邻两数，比如98和77，可以组成7798和9877，
  9877>7798所以，98在77前面
  '''
    for i in range(len(lists) - 1):
        for j in range(len(lists) - i - 1):
            if int(str(lists[j]) + str(lists[j + 1])) < int(str(lists[j + 1]) + str(lists[j])):
                lists[j], lists[j + 1] = lists[j + 1], lists[j]
    return lists


if __name__ == '__main__':
    lists = [98, 77, 981, 2221, 3322]
    arr = spile_max(lists)
    print(arr)
```

