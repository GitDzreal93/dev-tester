# InterviewPython 学习		

## 干货

[【GitHub】awesome-python](https://github.com/vinta/awesome-python)

[【GitHub】Python初学者](https://github.com/Yixiaohan/codeparkshare)

[【公众号】Python开发者](http://mp.weixin.qq.com/profile?src=3&timestamp=1563673765&ver=1&signature=TQ4Qya7gl94apqLCNkFNSrJGqwLjqxPJKWkefqoDFRYOzGzCs78LYq0MlwNL3B2*vuG8RthiokqOIpvne-*xdQ==)

[【公众号】Python中文社区](http://mp.weixin.qq.com/profile?src=3&timestamp=1563673779&ver=1&signature=CUm6FVMcSbpmGyzfw8gpTmiE6tW5Ke6C1VZ*vz-J9wL74NgQDUSarhwQ5jakjTxzKtSt1fOnqafLn-kNuvQKiA==)

[【官网】django中文文档](https://docs.djangoproject.com/zh-hans/2.2/)

[【官网】django-rest-framework](https://q1mi.github.io/Django-REST-framework-documentation/)

## 自己总结的

### python运行原理

#### python程序的生命周期

（1）源码阶段（.py文件）

（2）编译阶段（PyCodeObject字节码对象）

（3）运行阶段（Python虚拟机运行字节码指令）

python程序的运行依赖python解释器，执行一个`.py`文件，首先是将`.py`文件编译成`PyCodeObject字节码对象`，并存入内存中。接下来，python虚拟机逐条运行字节码指令。当运行完毕后，会生成`.pyc`文件，`.pyc`文件是`PyCodeObject字节码对象`在硬盘中的表现形式。

当下次再运行相同的`.py`文件时，假如源码没有任何改动，则不会再次将文件编译成`PyCodeObject字节码对象`，而是优先将`.pyc`文件载入到内存中去执行相应的字节码指令。

#### .pyc文件包含哪些信息？

一个 pyc 文件包含了三部分信息：Python 的 magic number、pyc 文件创建的时间信息，以及 PyCodeObject 对象。

注意：一个.py文件，只有被当作module时，才会生成`.pyc`文件，也就是假如文件中有 `if __name__ ==  '__main__' :`，将不会为这个`.py`文件生成`.pyc`文件，除非这个文件同时被其他运行的文件所引用（`import`）

#### 如何查看python的字节码指令

```python
import dis

with open('xxx.py','r') as f:
        # print(f.read())
        co = compile(f.read(),'xxx.py','exec')
        print(dis.dis(co))
```

### 数据类型

#### 基本数据类型（8种）

1. 整型（int）
2. 浮点型（float）
3. 字符串（str）
4. 列表（list）
5. 元祖（tuple）
6. 字典（dict）
7. 集合（set）
8. 布尔（bool）

#### **数据类型分类**

| 分类     | python 数据类型    |
| -------- | ------------------ |
| 数值类型 | 整数、浮点、布尔   |
| 序列类型 | 字符串、列表、元祖 |
| 散列类型 | 字典、集合         |

**字节类型表示**：`a=bytes('123') or a=b'123'`

**字节数组**：`bytearray('123')`

**可变序列**：列表`[]`，集合`{}`，字典`{"key":'value'}`

**不可变序列**：字符串`''`，元祖`()`

**有序序列**：列表`[]`，字符串`''`，元祖`()`

**无序散列**：集合`{}`，字典`{"key":'value'}`

------

### 数据类型方法

#### 字符串方法

1. 字符串拼接：
   - `str1 + str2`
   - `"".join([str1, str2])` 数组转字符串的常用方法
   - `"str1:%s,str2:%s"%(str1, str2)`
   - `"{}{}{}".format(str1, str2, str3)`
2.  `str1.replace(m,n,x)` 
   - 字符串替换
   - m：新字符串，n：旧字符串，x：替换个数
3. `str1.index(m)`
   - 查找**m**在**str1**中**位置（索引）**，找不到会抛出异常
4. `str1.find(m)`
   - 查找**m**在**str1**中**位置（索引）**，找不到返回 -1
5. `str1.count(m)`
   - 统计**m**在**str1**中出现的**次数**，一个都没有返回 0
6. `str1.isdigit()`
   - 判断**str1**是否是数字，返回 bool
7. `str1.isalpha()`
   - 判断**str1**是否是字母，返回bool
8. `str1.isupper()`
   - 判断**str1**是否是大写，返回bool
9. `str1.islower()`
   - 判断**str1**是否是小写，返回bool
10. `str1.startswith(m)`
    - 判断**str1**是不是以**m**开头，返回bool
11. `str1.endswith(m)`
    - 判断**str1**是不是**m**结尾，返回bool
12. `str1.upper()`
    - **str1**转化为大写
13. `str1.lower()`
    - **str1**转化为小写
14. `str1.strip()`
    - 去掉**str1**左右空白字符串
    - `.lstrip()`去掉左侧空白
    - `.rstrip()`去掉右侧空白
15. `str1.title()`
    - **str1**标题化
16. `str1.capitalize()`
    - **str1**第一个字母变成大写
17. `str1.split(m, x)`
    - 以**m**为界分割，分割**x**次

#### 列表方法

1. `li.append(m)`
  
   - **m**添加到列表末尾
   
2. `li.insert(x,m)`
  
   - 将**m**插入到列表，**x**是列表元素下标
   
3. `li.extend(list1)`
   - 列表拼接，li 尾部增加 list1 的元素，作用在 li 上
   - 这个方法也是充分体现了鸭子类型，传入的参数不仅是列表，元组、集合等可迭代对象都可以当作参数传入
   - 和`li+list1`区别：`li + list1`是表达式，要用一个变量来接收，如 `list2 = li + list1`
   
4. `li.pop(x)`
   - **x**：被删除元素的索引，返回值：被删除的元素
   - 若不传参数，则从最后开始删除
   
5. `li.remove(m)`
  
   - 删除一个元素**m**，没有返回值
   
6. `li.clear()`
  
   - 清空列表li
   
7. `li.index(m)`
  
   - 查询**m**的下标
   
8. `li.count(m)`
  
   - 统计**m**在**li**中出现的**次数**，一个都没有返回 0
   
9. `li[x] = m`
  
   - 把**li**中下标为**x**的元素的值，设置成**m**
   
10. 深拷贝和浅拷贝
    - `copy.copy(li)` 浅拷贝，只拷贝第一层元素的引用，产生的新的列表与被拷贝的列表互不影响
    - `copy.deepcopy(li)`深拷贝，递归拷贝所有元素，产生新的列表与被拷贝的列表互不影响
    - `li = old_li` 赋值，两个变量都指向同一个内存块，修改**li**会对**old_li**产生影响，同理，修改**old_li**也会对**li**产生影响
    
11. 永久排序
    -  `li.sort(reverse=True/False)`
      - True 倒序
      - False 正序
    - `li.reverse()` 
      - 永久倒序
    
12. 临时排序
    - `sorted(li, reverse=True/False)`
      - True 倒序
      - False 正序
    - `reversed(li) `
      - 临时倒序
    
13. 数组遍历

    ```python
    lists = [1, 2, 3, 4, 5]
    
    print("--------# 只遍历值------------")
    # 只遍历值
    for i in lists:
        print(i)
    
    print("--------# 逆序遍历--1----------")
    
    # 逆序遍历
    for i in lists[::-1]:
        print(i)
    
    print("--------# 逆序遍历--2----------")
    # 逆序遍历
    for i in range(len(lists), 0, -1):
        print(i)
    
    print("--------# 遍历键和值--2----------")
    # 遍历键和值
    for idx, val in enumerate(lists):
        print(idx,':',val)
    
    
    print("--------# 遍历键----------")
    # 只遍历键
    for idx in range(0, len(lists)):
        print(idx)
        
    ```

#### 元组方法

1. `tup.index(m)`
   - 查找**m**在**tup**中下标
2. `tup.count(m)`
   - 统计**m**在**tup**中出现的**次数**，一个都没有返回 0

#### 集合方法

1. `st1 & st2`
   - 求 **st1** 和 **st2** 的交集
2. `st1 | st2`
   - 求 **st1** 和 **st2** 的并集
3. `st1 - st2`
   - 求 **st1** 和 **st2** 的差集
4.  `st.add(m)`
   - 向集合**st**里面添加一个元素**m**
5. `st.pop()`
   - 随机删除集合里的元素
6. `st.remove(m)`
   - 指定删除集合里的元素**m**，若集合没有元素**m**，也不会报错
7. `st1.isdisjoint(st2)`
   - 判断**st1**与**st2**是否存在交集
8. `st1.issubset(st2)`
   - 判断**st1**是否是**st2**的子集
9. `st1.issuperset(st2)`
   - 判断**st1**是否是**st2**的父集
10. `st1.update(m)`
    - 向集合里面添加元素**m**，**m**可以为**字符串**、**列表**、**元组**、**集合**、**字典**（字典只存key）

#### 字典方法

1. `d = dict.fromkeys(m, v)`
   - 生成一个字典**d**，键是**可迭代对象m**中的元素，值是默认值**v**
2. `d.setdefault(k, v)`
   - 查询字典**d**中有没有**k**这个键，有则返回**k**对应的值；无则添加，`d[k]=v`
3. `d.clear()`
   - 清空字典
4. `d.pop(k)`
   - 删除以**k**为键的键值对
5. `d.popitem()`
   - 删除最后一个键值对
6. `d.update(new_d)`
   - 把new_d的键值对合并到d中
7. `d[k]=v`
   - 添加或修改键值对
8. `d.get(k, v)`
   - 查询**d**中是否**k**这个键值对，若有，返回**k**的值；若没有，返回默认值**v**

#### 判断类型的方法

1. `type(变量)`
2. `isinstance(变量，类型)`

------

### 运算符

*优先级从上至下，由高到低*

| 运算符                                                 | 说明                                             |
| ------------------------------------------------------ | ------------------------------------------------ |
| `**` 、`^`、  ` !`                                     | 指数、按位翻转、非                               |
| `*`、  `/`、  `%`、  ` //`                             | 乘、除、取模、整除                               |
| `+`、  ` -`                                            | 加、减                                           |
| `>>`、  `<<`                                           | 左移、右移                                       |
| `==`、 `>=`、 `<=`、 `>`、 `<`、 `!=`                  | 是否相等、大于等于、小于等于、大于、小于、不等于 |
| `=`、 `+=`、 `-=`、 `*=`、 `/=`、 `%=`、 `**=`、 `//=` | 赋值（其中`a+=b`，相当于`a=a+b`，其他的同理)     |
| `is`、 `is not`                                        | 判断`id`是否相同                                 |
| `in`、 `not in`                                        | 判断成员是否存在                                 |
| `and`、 `or`、 `not`                                   | 与、或、非                                       |

------

### 流程控制

#### 判断

if-else

```python
if 条件:
    语句
else:
	  语句
```

if-elif-else

```python
if 条件1：
		语句
elif 条件2:
    语句
elif 条件3:
    语句
else:
    语句
```

类似三目运算符

```python
# 如果满足条件，则执行语句A，否则执行语句B，是if-else的简写
语句A if 条件 else 语句B
```

#### 循环

while

```python
while 条件:
    语句
```

while-else

```python
while 条件:
    语句
else:
    循环结束后执行的语句
```

for-in

```python
for i in 可迭代对象:
    语句
```

跳出循环

```python
# break 跳出整个循环体
for i in [1, 2, 3, 4, 5]:
    # 当i>3时，跳出循环，不再往下迭代
    if i>3:
        break
    print(i)
    
# [输出] : 1, 2, 3


# continue 跳出本次循环
for i in [1, 2, 3, 4, 5]:
    # 当i=3时，跳出本次循环，进入下次迭代
    if i=3:
        continue
    print(i)
    
# [输出] : 1，2，4，5
```

------

### 函数

#### 定义函数

```python
# 无参无返回值
def foo():
    语句

# 有参无返回值
def foo(x, y):
    z = x + y

# 无参有返回值
def foo():
    x = 1
    return x + 1

# 有参有返回值
def foo(x, y):
    return x + y
```

#### 空函数

```python
def emptyfunc():
    pass
```

#### 多个返回值

return多个值，其实是返回一个元组

```python
def foo():
    x, y, z=1, 2, 3
    return x,y,z

# 返回一个元组
tup = foo()
# 元组拆包
x1, y1, z1 = foo()
```

#### 函数的参数

1. 必选参数
   - `def foo(x, y, z):pass`
2. 默认参数
   - `def foo(x=1):pass`
   - 假如不传`x`参数的话，`x`默认是等于1
3. 可变参数（`*`）
   - 不定长传参
     - 定义：`def foo(*args):pass`
     - 调用：`foo(1,2,3,4,5)`
   - 元组和列表的压包
     - 定义：`def foo(*args):pass`
     - 调用：`foo(*(1,2,3,4,5))` or `foo(*[1,2,3,4,5])`
4. 关键字参数（`**`)
   - 定义：`def foo(**kwargs):pass`，其中`kwargs`是一个字典
   - 调用：
     - `foo(k1=v1, k2=v2)`  参数传入键值对
     - `foo(**d)` 参数传入字典**d**
5. 命名关键字参数（`*`）
   - 定义：
     - `def foo(a, b, *, k1, k2, k3):pass`  a, b是普通参数，k1, k2, k3 是命名关键字参数
     - 命名关键字参数需要一个特殊分隔符`*`，后面的参数被视为命名关键字参数
     - 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了
   - 调用
     - `foo(a, b, k1=v1, k2=v2, k3=v3)`
6. 参数组合
   - 定义：`def fun(parameter,*args,keyparameter,**kwargs):pass`
   - 参数定义顺序：必选参数，默认参数，可变参数，命名关键字参数，关键字参数

#### 函数的递归

```python
# 典型案例：斐波那契数列
def fib_recursion(idx):
    if idx <= 2:
        return 1
    else:
        return fib_recursion(idx-1) + fib_recursion(idx-2)
```

注意：

1. 必须设置函数终止条件
2. 实用递归优点是逻辑简单清晰，缺点是过深调用导致栈溢出

#### 函数作用域

1. 外部不能访问函数内部变量
2. 函数内部能够访问外部变量
3. 函数内部不能修改外部变量，强行修改需要加 `global 外部变量 = 新值`
4. 函数内部和函数外部变量名可以相同，但需要注意访问优先级

```python
# 例子一，内部访问同名变量，不要在内部变量定义之前去访问
g = 1

def foo():
    print(g)  # 报错 UnboundLocalError: local variable 'g' referenced before assignment
    g = 2
    print(g)  # 2

foo()
print(g)  # 1

# -------------------- #
# 例子二，采用global关键字，会把内部变量g声明成全局变量，从而在函数内部可以改变全局变量
g = 1

def foo():
    global g
    print(g) # 1
    g = 2
    print(g) # 2

foo()
print(g) # 2
```

#### 函数式编程

##### 高阶函数

`map`

用法：`map(函数名，列表/元组/集合/字符串)`

说明：把传入的函数依次作用于每个元素，处理完后返回的是生成器类型，需要用list生成数据

```python
li = [1, 2, 3, 4, 5]
def add1(x):
    return x + 1
add_li = list(map(add1, li)) # [2, 3, 4, 5, 6]
```

`filter`

用法：`filter(函数名, 列表/元组/集合/字符串)`

说明：filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素，处理完后返回的是生成器类型，需要用list生成数据

```python
li = [1, 2, 3, 4, 5, 6]
def even_num(n):
    if n%2 == 0:
        return n
      
even_li = list(filter(even_num, li)) # [2,4,6]
```

`reduce()`

用法：`reduce(函数名，列表/元组/集合/字符串)`

说明：reduce()用于对参数序列中元素进行累积。python3 中，reduce已经被从全局名字空间里移除了，它被放置在functools模块里

```python
from functools import reduce
li = [1, 2, 3, 4, 5]
m = reduce(lambda x, y : x+y, li)   # m=15
```

#### 返回函数

```python
def outer_foo(*args):
    def inner_foo():
        for i in args:
            print(i)
    return inner_foo

f = outer_foo(1, 2, 3, 4, 5)
f()  # 1 2 3 4 5
```

函数可以被当作返回值返回

#### 函数的闭包

```python
# 典型案例1: 
# 外部函数只是返回了函数名的列表，但并没有调用。等到内部函数被调用的时候，外部函数已经迭代完毕，最终的i变成3，所以f1()、f2()、f3()相当于执行了三次f(),返回3*3=9
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
print(f1())   # 9
print(f2())   # 9
print(f3())   # 9

# 典型案例2:
# 外部函数返回的是内部函数名的调用，所以结果和案例1不相同
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
f1,f2,f3=count()
print(f1())  # 1
print(f2())  # 4
print(f3())  # 9
```

#### 匿名函数

语法：`lambda 形参:含形参的表达式`

```python
f = lambda x:x+1
print(f(1)) # 2
```

lambda 返回的是函数名（函数地址）

lambda 常常与map、filter、reduce、sorted等联合使用

#### 装饰器

作用：增强函数的功能，其实装饰器可以装饰函数，也可以装饰类

定义装饰器

```python
def decorator(func):
	def wrapper(*args,**kargs): # 可以自定义传入的参数
		print(func.__name__)
		return func(*args,**kargs) # 返回传入的方法名参数的调用
	return wrapper  # 返回内层函数函数名
```

实用装饰器

```python
# 使用方法1
f = decorator(函数名)  # 装饰器不传入参数时
f = (decorator(参数))(函数名)  # 装饰器传入参数时
f()  # 执行被装饰过的函数

# 使用方法2
@decorator # 已定义的装饰器
def f():  # 自定义函数
    pass
f() # 执行被装饰过的函数
```

自身不传入参数的装饰器

```python
def login(func):
    def wrapper(*args,**kargs):
        print('函数名:%s'% func.__name__)
        return func(*args,**kargs)
    return wrapper
    
@login
def f():
    print('inside decorator!')

f()

# 输出:
# 函数名:f
# 函数本身:inside decorator!
```

自身传入参数的装饰器

```python
def login(text):
    def decorator(func):
        def wrapper(*args,**kargs):
            print('%s----%s'%(text, func.__name__))
            return func(*args,**kargs)
        return wrapper
    return decorator

@login('this is a parameter of decorator')  # 等价于 ==> (login(text))(f) ==> 返回 wrapper
def f():
    print('2019-06-13')

f() # 等价于 ==> (login(text))(f)() ==> 调用 wrapper() 并返回 f()

# 输出:
# this is a parameter of decorator----f
# 2019-06-13
```

内置装饰器

`@property`  ：把类内方法当成属性来使用，必须要有返回值，相当于getter；假如没有定义@func.setter修饰的方法的话，就是只读属性。

```python
# property 例子
class Car:

    def __init__(self, name, price):
        self._name = name
        self._price = price

    @property
    def car_name(self):
        return self._name

    # car_name可以读写的属性
    @car_name.setter
    def car_name(self, value):
        self._name = value

    # car_price是只读属性
    @property
    def car_price(self):
        return str(self._price) + '万'


benz = Car('benz', 30)
print(benz.car_name)   # benz
benz.car_name = "baojun"
print(benz.car_name)   # baojun
print(benz.car_price)  # 30万

```

`@staticmethod` ：静态方法，不需要表示自身对象的self和自身类的cls参数，就跟使用函数一样。

`@classmethod`  ：类方法，不需要self参数，但第一个参数需要是表示自身类的cls参数。

```python
class Demo(object):
    text = "三种方法的比较"

    def instance_method(self):
        print("调用实例方法")


    @classmethod
    def class_method(cls):
        print("调用类方法")
        print("在类方法中 访问类属性 text: {}".format(cls.text))
        print("在类方法中 调用实例方法 instance_method: {}".format(cls().instance_method()))

    @staticmethod
    def static_method():
        print("调用静态方法")
        print("在静态方法中 访问类属性 text: {}".format(Demo.text))
        print("在静态方法中 调用实例方法 instance_method: {}".format(Demo().instance_method()))


if __name__ == "__main__":
    # 实例化对象
    d = Demo()

    # 对象可以访问 实例方法、类方法、静态方法
    # 通过对象访问text属性
    print(d.text)
    # 通过对象调用实例方法
    d.instance_method()
    # 通过对象调用类方法
    d.class_method()
    # 通过对象调用静态方法
    d.static_method()

    # 类可以访问类方法、静态方法
    # 通过类访问text属性
    print(Demo.text)
    # 通过类调用类方法
    Demo.class_method()
    # 通过类调用静态方法
    Demo.static_method()
```

`@staticmethod`和`@classmethod`的区别和使用场景：

在上述例子中，我们可以看出，

在**定义**静态类方法和类方法时，`@staticmethod`装饰的**静态方法**里面，想要访问类属性或调用实例方法，必须需要把类名写上；而`@classmethod`装饰的**类方法**里面，会传一个`cls`参数，代表本类，这样就能够避免手写类名的硬编码。

在**调用**静态方法和类方法时，实际上写法都差不多，一般都是通过`类名.静态方法()`或`类名.类方法()`。也可以用实例化对象去调用静态方法和类方法，但为了和实例方法区分，最好还是用类去调用静态方法和类方法。

所以，在定义类的时候，**假如不需要用到与类相关的属性或方法时，就用静态方法**`@staticmethod`；**假如需要用到与类相关的属性或方法，然后又想表明这个方法是整个类通用的，而不是对象特异的，就可以使用类方法**`@classmethod`。

#### 内置函数

##### 常用函数

`len()`求长度

`min()`求最小值

`max()`求最大值

`sorted()`排序

`sum()`求和

`dir()`查看对象所有属性和方法

`type()`查看对象属于哪个类

##### 进制转换函数

`bin()`转换为二进制

`oct()`转换为八进制

`hex()`转换为十六进制

`ord()`字符转ASCII码

`chr()`ASCII码转字符

##### 高级内置函数

`enumerate()` 转化为元组标号，通常用于遍历输出列表元素下标

```python
# enumerate 的例子
li = [1, 2, 3, 4, 5]
for idx, ele in enumerate(li):
    print(idx, ':', ele)
```

`eval(str)`只能运行一行字符串代码，也可以用于字符串转字典

```python
# eval 的例子
d = eval("{'a':1,'b':2}")
print(type(d))
print(d)
```

`exec(str)`执行字符串编译过的字符串，可以运行多行字符串代码

`filter(函数名,可迭代对象)`过滤器

`map(函数名,可迭代对象)`对每个可迭代对象的每个元素处理

`zip(可迭代对象,可迭代对象)`将对象逐一配对

```python
# zip 的例子
# 压缩
l1 = ['a', 'b', 'c', 'd', 'e']
l2 = [1, 2, 3, 4, 5]
l = list(zip(l1,l2)) # [('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)]
d = dict(zip(l1,l2)) # {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 解压
tup = (('a',1),('b',2),('c',3),('d',4))
l1,l2 = zip(*tup)
# l1 ('a', 'b', 'c', 'd')
# l2 (1, 2, 3, 4)
```

------

### 高级特性

#### 切片

切片适用于字符串`''`、列表`[]`和元组`()`

```python
li = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(li[0]) # 取下标为0的元素，1
print(li[::]) # 取全部元素，[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(li[::2]) # 步长为2取值，[1, 3, 5, 7, 9]
print(li[1:3]) # 左开右闭，[2, 3]
print(li[1:]) # 下标大于等于1的元素，[2, 3, 4, 5, 6, 7, 8, 9, 10]
print(li[:5]) # 下标小于5的元素，[1, 2, 3, 4, 5]
print(li[::-1]) # 列表逆序，[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

#### 迭代

可迭代对象：可以直接作用于for循环的对象统称为可迭代对象(`Iterable`)

判断对象是否可迭代

```python
from collections import Iterable


print(isinstanc(对象，Iterable))
# True为可迭代
# False为不可迭代
```

可迭代对象：字符串，列表，元组，字典，集合

遍历可迭代对象（字典除外）：

```python
for i in 可迭代对象:
    语句
```

对字典遍历

```python
# d 是一个字典

# 遍历 keys
for i in d:
    语句
# 或者
for i in d.keys():
    语句
    
# 遍历 values
for i in d.values():
    语句

# 同时遍历 key 和 value
for k,v in d.items():
    语句
    
```

#### 生成式

**列表生成式**：`[返回的参数 for循环 条件判断]`

```python
li = [i for i in range(11) if i%2==0]
print(li) # [0, 2, 4, 6, 8, 10]
```

**集合生成式**：`{返回的参数 for循环 条件判断}`

```python
s = {i for i in range(1,10) if i%2==0}
print(s)  # {8, 2, 4, 6}
```

**字典生成式**：`{key:value for循环 条件判断}`

```python
li=['a','b','c','d','e','f','g','h']
d={i:j for i,j in enumerate(li) if i%2==0}
print(d)  # {0: 'a', 2: 'c', 4: 'e', 6: 'g'}
```

**生成器**：`(返回的参数 for循环 条件判断)`

```python
gen = [i for i in range(11) if i%2==0]
print(gen) # [0, 2, 4, 6, 8, 10]
```

#### 生成器

生成器：为了节省内存空间，提高程序速度，这种一边循环一边计算的机制，称为生成器(`Generator`)。

`next()`取值

```python
li=[1, 2, 3, 4, 5]
g=(x for x in li)
print(g)
print(next(g))
print(next(g))
print(next(g))
print(next(g))
print(next(g))

# [输出]:
# <generator object <genexpr> at 0x0000018F628397C8>
# 1
# 2
# 3
# 4
# 5
```

for循环遍历取值

```python
li=[1, 2, 3, 4, 5]
g=(x for x in li)
print(g)
for i in g:
    print(i)
    
# [输出]:
# <generator object <genexpr> at 0x0000018F628397C8>
# 1
# 2
# 3
# 4
# 5
```

生成器函数

用yield返回的函数，都可以看作是生成器

```python
def fun():
    yield 1
    yield 2
    yield 3
      
print(next(f))   # 1
print(next(f))   # 2
print(next(f))   # 3
print(next(f))   # 抛出异常：StopIteration
```

用for循环遍历生成器

```python
f = fun()
for i in f:
  print（i）

# [输出]：
# 1
# 2
# 3
```

#### 迭代器

迭代器：可以被`next()`函数调用并不断返回下一个值的对象称为迭代器(`Iterator`)

实用`iter(可迭代对象)`可以把可迭代对象转化为迭代器

```python
it = iter([1, 2, 3, 4, 5])
next(it) # 1
next(it) # 2
next(it) # 3
```

注意：

1. 凡是可作用于for循环的对象都是Iterable类型
2. 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列
3. 集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象
4. Python的for循环本质上就是通过不断调用next()函数实现的

------

### 类

#### 类的定义

```python
class Teacher:
    pass
```

#### 控制对象的生成过程`__new__`

`__new__`是用来控制对象的生成过程的，在对象生成之间调用，如果`__new__`方法不返回对象，则不会调用`__init__`

```python
class User:
    def __new__(cls, *args, **kwargs):
        print("in new")
        return super().__new__(cls)
    def __init__(self, name):
        print("in init")
        self.name = name
```

#### 初始化实例属性`__init__`

`__init__`是用来完善对象的，在对象生成之后调用

```python
class Teacher:
  
  def __init__(self, name, age, subject):
      self.name = name
      self.age = age
      self.subject = subject
```

#### 实例化对象

```python
t = Teacher('Dzreal', 25, 'PE')
```

#### 实例属性和类属性

```python
class Teacher:
    
    age = 20
    
    def __init__(self, name, age, subject):
          self.name = name
          self.age = age
          self.subject = subject
          
t = Teacher('Dzreal', 25, 'PE')
print(t.age)    # 25
del t.age       # 删除实例属性后，自动调用类属性
print(t.age)    # 20
```

注意：

1. 实例属性的优先级高于类属性
2. 类没有实例属性时会调用类属性

#### 访问限定

伪私有属性 `_attrname`：可以访问，但不要随意访问

私有属性 `__attrname`：一般不可访问，强行访问的话，可以通过 `实例._类名__attrname`来访问

```python
class Teacher:

    def __init__(self, name, age, subject):
        self.__name = name
        self.age = age
        self._subject = subject

    def get_fake_private(self):
        print(self._subject)

    def get_private(self):
        print(self.__name)


t = Teacher('Dzreal', 25, 'PE')

# 通过实例方法访问私有属性
t.get_fake_private()  # PE
t.get_private()  # Dzreal

# 直接访问私有属性
print(t._subject)  # PE
print(t._Teacher__name) # Dzreal
print(t.__name)    # 报错 AttributeError: 'Teacher' object has no attribute '__name'
```

#### 定制属性访问

`hasattr(m,'n')` m:实例 n:类里面的属性 确定属性是否存在

`getattr(m,'n')` m:实例 n:类里面的属性 得到指定类属性的值

`setattr(m,'n',x)` m:实例 n:类里面的属性 x:设置属性的值 有则改无则添加

`delattr(m,'n')` m:实例 n:类里面的属性 删除一个类属性

#### 继承

##### 单继承

```python
class A:
    def foo(self):
        print("father foo")

class B(A):
    pass

b = B()
b.foo()  # father foo
```

##### 多继承

python的继承采用 C3算法+DFS算法（深度优先算法）实现

```python
# 案例一、普通多继承
class A:
    def foo(self):
        print(A)

class B:
    def foo(self):
        print(B)

class C(A):
    pass

class D(B):
    pass

# 继承顺序似乎和C、D的顺序有关，要是改成 class E(D, C), 继承顺序又会不一样
class E(C, D):
    pass

e = E()
e.foo() 
# A

# mro() 或 __mro__ 可以查看继承顺序
print(E.mro()) 
# [<class '__main__.E'>, <class '__main__.C'>, <class '__main__.A'>, <class '__main__.D'>, <class '__main__.B'>, <class 'object'>]
# 即继承顺序是：E->C->A->D->B

# 案例二、菱形结构继承
class A:
    pass

class B(A):
    pass

class C(A):
    pass

class D(C,B):
    pass

print(D.mro())
# [<class '__main__.D'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>]
# 即继承顺序是：D->C->B->A
# 说明python的继承不完全是深度优先算法实现，而是采用 C3算法+DFS算法（深度优先）
```

##### super

```python
class A:
    def __init__(self):
        print("A")

class B(A):
    def __init__(self):
        print("B")
        super().__init__()
        
b = B()

# [输出]：
# B
# A
```

#### 多态

```python
class Animal:
    def run(self):
        raise AttributeError('子类必须实现这个方法')
 
class People(Animal):
    def run(self):
        print('人正在走')
 
class Pig(Animal):
    def run(self):
        print('猪正在走')

peo=People()
pig=Pig()

peo.run()  # 人正在走
pig.run()  # 猪正在走
```

注意：

1. `super().__init__()`不是调用父类的构造函数，而是调用mro下一个类的构造函数
2. 假如父类方法`foo`被子类重写，`super().foo()`可以调用父类方法
3. 假如父类方法`foo`没有被子类重写，`self.foo()`可以调用父类方法

##### 鸭子类型

python的内置协议（魔法函数）就是一个很典型的鸭子类型的例子：

如果一个对象实现了`__getitem__`方法，那python的解释器就会把它当做一个`collection`，就可以在这个对象上使用切片，获取子项等方法。

如果一个对象实现了`__iter__`和`next`方法，python就会认为它是一个`iterator`，就可以在这个对象上通过循环来获取各个子项。

```python
# 鸭子类型案例： 
# 下面的例子，之所以能实现鸭子类型，是因为python的变量可以指向任意类型。而在java中，是无法实现的，java中需要指明类型或父类类型，才能实现多态。
class Pig:
    def say(self):
        print("i am a pig")

class Dog:
    def say(self):
        print("i am a dog")
        
class Duck:
    def say(self):
        print("i am a duck")

animal_list = [Pig, Dog, Duck]
for animal in animal_list:
    animal().say()

# [输出]:
# i am a pig
# i am a dog
# i am a duck
```

------

### 魔法函数

python的魔法函数定义之后，不要去调用它，python解释器自己会去调用，定义魔法函数的作用是在于利用python的鸭子类型的特性，给类添加一些特性。

#### 字符串表示

`__repr__`：在命令行交互模式下会用到，某个类中重写了`__repr__`方法，就不用调用`print`函数，只需要输入类名，在控制台下就会调用`__repr__`，输出`__repr__`定义的内容

`__str__`：  调用`print(类名)`，实际上会调用类的`__str__`方法

#### 集合、序列相关

`__len__`：获取元素个数

`__getitem__`

`__setitem__`

`__delitem__`

`__contains__`

#### 迭代相关

`__iter__`

`__next__`

#### 可调用

`__call__`

#### with上下文管理器

`__enter__`：开始的时候调用

`__exit__`：结束的时候调用

#### 数值转换

`__abs__`

`__bool__`

`__int__`

`__float__`

`__hash__`

`__index__`

#### 元类相关

`__new__`

`__init__`

#### 属性相关

`__getattr__`、`__setattr__`

`__getattribute__`、`__setattribute__`

`__dir__`

#### 属性描述符

`__get__`

`__set__`

`__delete__`

####  协程

`__await__`

`__aiter__`

`__anext__`

`__aenter__`

`__aexit__`

------

### IO编程

#### 文件操作

**打开文件**

```python
# 打开文件
# 模式：
# r 读
# w 写
# a 追加写
# r+ 读写(先读后写)
# w+ 写读(先写后读)
# a+ 追加写读(先写后读)
fp = open(文件路径，模式，encoding='utf-8')

# 关闭文件
fp.close() 

# 上下文管理器打开文件（会自动close文件）
with open(文件路径，模式，encoding='utf-8') as fp:
    fp.read()
```

`fp.seek(m)`：移动文件指针，当前位置向后移动m个字节

`fp.tell(m)`：查看文件指针

`fp.flush(m)`：刷新缓冲区和`fp.close()`类似

**文件读取**

`fp.read(m)`：文件全文读取成一个大字符串，m：代表读取m个字节。

`fp.readline()`：按行读取文件，默认只返回第一行，查看全文用`while` 循环遍历文件

```python
with open('test.txt','r',encoding='utf-8') as fp:
    line = fp.readline()
    while line:
        line = fp.readline()
        print(line)
```

`fp.readlines()`：把文件按行存成大数组，一般用for循环遍历每行

**文件写入**

`fp.write(内容)` 文件中写入内容

`fp.writelines(list)` 文件中写入列表类型内容

`fp.truncate()` 文件内容清空

#### Json操作

需要用到 json 库（`import json`）

`json.loads()`：把 json 格式的字符串转换成 python 对象

`json.dumps()`：把 python 对象转换成 json 格式的字符串

`json.load(open(文件路径))`：读取文件中 json 形式的字符串元素，转换成python对象

`json.dump(open(文件路径))`：把 python 对象转换成 json 格式的字符串并存入文件中

**jsonpath**

| Xpath | JSONPath | 描述                                                         |
| ----- | -------- | ------------------------------------------------------------ |
| /     | $        | 跟节点                                                       |
| .     | @        | 现行节点                                                     |
| /     | . or []  | 取子节点                                                     |
| ..    | ..       | 就是不管位置，选择所有符合条件的条件                         |
| *     | *        | 匹配所有元素节点                                             |
| []    | []       | 迭代器标示(可以在里面做简单的迭代操作，如数组下标，根据内容选值等) |
| &#124 | [,]      | 支持迭代器中做多选                                           |
| []    | ?()      | 支持过滤操作                                                 |
| n/a   | ()       | 支持表达式计算                                               |
| ()    | n/a      | 分组，JsonPath不支持                                         |

```python
import json
import jsonpath


json_str = '[{"id":1,"category":{"id":1,"category":"常用改图工具","sn":"cierlezk","weight":60,"cat_is_show":true,"add_time":"2019-07-13T11:41:00","webconfig":1},"name":"在线修改图片尺寸","desc":null,"url":"https://www.gaitubao.com/","logo":null,"ws_is_show":true,"add_time":"2019-07-13T11:42:00"},{"id":2,"category":{"id":1,"category":"常用改图工具","sn":"cierlezk","weight":60,"cat_is_show":true,"add_time":"2019-07-13T11:41:00","webconfig":1},"name":"图片加水印","desc":null,"url":"https://www.gaitubao.com/shuiyin/","logo":null,"ws_is_show":true,"add_time":"2019-07-13T11:46:00"},{"id":3,"category":{"id":1,"category":"常用改图工具","sn":"cierlezk","weight":60,"cat_is_show":true,"add_time":"2019-07-13T11:41:00","webconfig":1},"name":"在线压缩图片","desc":null,"url":"https://img.top/","logo":null,"ws_is_show":true,"add_time":"2019-07-13T11:46:00"}]'
# json 字符串转成 python 对象
py_obj = json.loads(json_str)
name = jsonpath.jsonpath(py_obj, '$..name')
url = jsonpath.jsonpath(py_obj, '$..url')
print(name)  # ['在线修改图片尺寸', '图片加水印', '在线压缩图片']
print(url)  # ['https://www.gaitubao.com/', 'https://www.gaitubao.com/shuiyin/', 'https://img.top/']
```

#### 网络编程

##### http请求

发送http请求用`requests`库（`import requests`）

**发送请求**

`r = requests.get(url='网址url', params={"k":"v"})` ：发送 get 请求

`r = requests.post(url='网址url', data={"k":"v"})` ：发送 post 请求

`r = requests.post(url='网址url', data={"k":"v"})` ：发送 put 请求

`r = requests.post(url='网址url', data={"k":"v"})` ：发送 patch 请求

`r = requests.post(url='网址url', **kwargs)` ：发送 delete 请求

`r = requests.head(url='网址url')` ：发送 head 请求

**响应处理**

```python
import requests


url = 'http://www.baidu.com'
r = requests.get(url)

print(r.text)  # 获取网页html源码
print(r.json())  # 若返回值是json，把json转成python对象
print(r.status_code) # 获取http响应码
print(dict(r.cookies))  # 获取cookies
print(dict(r.headers))  # 获取headers

try:
    r = requests.get(url)
    r.raise_for_status()
    print(r.text)
except:
    print("http请求出现异常")
```

##### socket编程

**服务端**要做的工作：

1. 创建socket
2. 绑定端口
3. 监听端口
4. 接收消息
5. 发送消息

**客户端**要做的工作：

1. 创建socket
2. 连接socket
3. 发送消息
4. 接收消息

**客户端代码**

```python
# 客户端
import socket


if __name__ == '__main__':
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.connect(('127.0.0.1', 8000))
    while True:
        re_data = input()
        client.send(re_data.encode("utf8"))
        data = client.recv(1024)
        print(data.decode("utf8"))
```

**服务端代码**

```python
# 服务端
import socket
import threading


# 处理客户端的请求
def handle_sock(sock, addr):
    while True:
        data = sock.recv(1024)
        print(data.decode("utf8"))
        re_data = input()
        sock.send(re_data.encode("utf8"))
        
if __name__ == '__main__':
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind(('0.0.0.0', 8000))
    server.listen()

    #获取从客户端发送的数据
    #一次获取1k的数据
    while True:
        sock, addr = server.accept()

        #用线程去处理新接收的连接(用户)
        client_thread = threading.Thread(target=handle_sock, args=(sock, addr))
        client_thread.start()
```

------

### 多线程、多进程、线程池和进程池编程

#### 多线程

多线程是操作系统能够调度的最小单元

需要用到`threading`库（`import threading`）

**实例化** `对象=Thread(target=函数名,args=(函数参数,))`

**设置线程名** `对象.setName(‘线程名’)`

**获取线程名** `对象.getName()`

**获取当前线程名** `threading.current_thread().name`

**开启线程** `对象.start()`

**线程守护** `对象.setDaemon(True)` 主线程结束它的子线程就结束

**线程阻塞** `对象.join()` 当子线程结束后主线程才结束

**线程锁** 解决cpu资源竞争问题

**实例化线程锁** `对象=threading.Lock()`

**上锁** `对象.acquire()`

**解锁** `对象.release()`

**可重入锁** `对象=threading.RLock()` 在同一个线程（函数）里面，锁里面还可以加锁，只要锁acquire的次数和release的次数一样就行

**线程间通讯**

1. 共享变量（不是线程安全，需要加锁）
2. Queue（`from queue import Queue`，Queue是线程安全的）

**线程同步**

Q：为什么要线程同步？

A：保证同一时刻，只有加锁的代码段处于执行状态，不会因为cpu资源竞争，不同代码段同时操作某个数据，导致数据错乱，这样保证了线程安全

Q：为什么会出现死锁？

A：（1）锁没有释放，然后又申请了加锁。（2）互相等待，资源互相竞争

Q：怎么解决死锁？

A：（1）申请锁要记得释放锁（2）用可重入锁`RLock`

*注意*：

1. 用锁会影响性能
2. 锁有可能会引起死锁

线程间同步的方式：

1. `Lock`  锁

2. `RLock` 可重入锁 

3. `condition` 条件变量，用于复杂的线程间同步。

   - 需要注意线程启动顺序

   - 可以用`with语句`获得`condition`
   - 重要方法：`condition.notify()`和`condition.wait()`，切换conditon时用到

4. `Semaphore` 信号量，用于控制进入数量的锁（控制线程并发数量），`semaphore.acquire()`减少一把锁，`semaphore.release()`加回来一把锁，锁的数量等于0，就等待

##### 基于函数创建线程

```python
import threading


def mytask(name):
    print("{} task start".format(name))
    time.sleep(2)
    print("{} task stop".format(name))
  
if __name__ == '__main__':
    thread1 = threading.Thread(target="mytask", args=('task1',))
    thread2 = threading.Thread(target="mytask", args=('task2',))
    thread1.start()
    thread2.start()
    thread1.join()
    thread2.join()
    print("线程全部执行完毕")
```

##### 基于类创建线程

```python
class MyTask(threading.Thread):
    
    def __init__(self, name):
        super().__init__(name=name)
  
    # 必须要重载run方法
    def run(self):
        print("{} task start".format(name))
        time.sleep(2)
        print("{} task stop".format(name))
    
if __name__ == '__main__':
    thread1 = MyTask(task1)
    thread2 = MyTask(task2)
    thread1.start()
    thread2.start()
    thread1.setDaemon(True)  # 主线程结束了，子线程也会结束
    thread2.setDaemon()
    print("线程全部执行完毕")
```

#### 线程池和进程池

使用`concurrent.futures`库（`from concurrent.futures import ThreadPoolExecutor,`）

`ThreadPoolExecutor(max_workers)`**线程池**

```python
import time
from concurrent.futures import ThreadPoolExecutor, as_completed，wait

def mytask(name):
    print("{} task start".format(name))
    time.sleep(2)
    print("{} task stop".format(name))
    return time.time()

if __name__ == '__main__':
    all_tasks = []
    executor = ThreadPoolExecutor(max_workers=3)
    task1 = executor.submit(mytask, ('task1'))
    task2 = executor.submit(mytask, ('task2'))
    all_tasks.append(task1)
    all_tasks.append(task2)
    
    # 判断线程有没有执行完成，done()方法是非阻塞的方法
    print(task1.done()) 
    
    # 可以获取线程执行结果，result()方法是阻塞的方法
    print(task1.result())
    
    # 取消执行线程，只能在线程执行前调用，线程开始或线程结束，无法调用 cancel()
    task2.result()  # false
    
    # as_completed 是生成器，返回已经完成的future对象
    for future in as_completed(all_tasks):
        data = future.result()
        print("completed time is {}".format(data))
        
    # 用 executor.map(函数，iterable)更加能精简代码，map返回的是future.result()
    name_list = ['task3', 'task4', 'task5']
    for data in executor.map(mytask, name_list):
        print(data)
        
    # wait(future序列)，等待future序列都运行完了，才往下执行
    future_list = [executor.submit(mytask, (name)) for name in name_list]
    wait(future_list)
    print("线程全部执行完毕")
```

`ProcessPoolExecutor(max_workers)`**进程池**

进程池用法和线程池差不多

#### 多进程

**多进程和多线程的使用场景**

- 耗CPU的操作，用**多进程**
- 耗IO的操作，用**多线程**

多进程用`multiprocessing`模块

```python
from multiprocessing import Process, Pool

def fib(n):
    if n < 2:
        return 1
    return fib(n-1) + fib(n-2)

if __name__ == '__main__':
    # 进程
    process = Process(target=fib, args=(10,))
    process.start()
    process.join()
    
    # 进程池 multiprocessing的进程池
    pool = multiprocessing.Pool(multiprocessing.cpu_count())
    # 异步启动进程池，返回值是类似future的对象
    result = pool.apply_async()
    # 不再接收新的任务（在pool.join()之前，一定要有pool.close()）
    pool.close()
    # 等待所有进程执行完
    pool.join()
    # 返回进程执行返回值
    result.get()
```

**进程间通信**

1. Queue： 用 `multiprocessing.Queue` ，注意：这个Queue不能用在multiprocessing.Pool中
2. Pipe： pipe只能适用于两个进程
3. Manager： 提供进程间通信的多种数据类型，其中`Manager().Queue()`可以用在 multiprocessing.Pool中

------

### 协程

**协程是什么**：可以暂停的函数，且可以向暂停的函数传入值

**协程的目的**：

1. 采用同步的方式去编写异步的代码
2. 使用单线程去切换任务

**协程和多线程的差别**：

1. 线程是由操作系统切换的；协程是单线程切换，而且可由程序猿自己去调度任务
2. 不需要锁，并发性更高

**生成器协程**

