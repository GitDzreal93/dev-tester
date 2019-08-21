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
    快速排序（升序）
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

### 8、如何求字符串例的最长回文子串

【题目】回文子串：一个字符串从左到右遍历与从右到左遍历得到的序列是相同的，例如“abcba”是回文子串，但是