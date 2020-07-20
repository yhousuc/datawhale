# Task01: 变量、运算符、数据类型及位运算
>by 雁过留声央日红   Datawhale

## 目录   
  - [1.变量](#1. 变量)
  - [2.运算符](#2. 运算符)
  - [3.基本数据类型](#3. 基本数据类型)
  - [4.练习](#4. 练习题)
  - [5. LeetCode.136 只出现一次的数字](#5. LeetCode.136 只出现一次的数字)

## 1. 变量
  - Python 中的变量是不需要声明的（区别于C++、JAVA的关键字类型的声明方式）
  - 变量在使用前都必须赋值，赋值后才会创建


```python
# python 环境
!python -V
```

    Python 3.6.10 :: Anaconda, Inc.



```python
number = 100        # 整数型变量
distance = 100.12   # 浮点型变量
name = "yhousuc"    # 字符串型变量

number, distance, name
```


    (100, 100.12, 'yhousuc')



**多个变量赋值**

 - 赋相同的值


```python
name1 = name2 = name3 = "LiBai"
name1, name2, name3
```


    ('LiBai', 'LiBai', 'LiBai')



 - 赋不同的值


```python
a,b,c = 1, 2, 3
a,b,c
```


    (1, 2, 3)



 ## 2. 运算符

#### <span id="jump2.1">2.1. 算数运算符</span>

<img src="C:\Users\13398\Desktop\1.png" alt="1" style="zoom:67%;" />

#### <span id="jump2.2">2.2. 比较运算符</span>

<img src="C:\Users\13398\Desktop\比较运算符.png" alt="比较运算符" style="zoom:67%;" />

#### <span id="jump2.3">2.3. 赋值运算符</span>

<img src="C:\Users\13398\Desktop\赋值运算符.png" alt="赋值运算符" style="zoom:67%;" />

#### <span id="jump2.4">2.4. 比较运算符</span>

<img src="C:\Users\13398\Desktop\比较运算符.png" alt="比较运算符" style="zoom:67%;" />

#### <span id="jump2.5">2.5. 逻辑运算符</span>

<img src="C:\Users\13398\Desktop\逻辑运算符.png" alt="逻辑运算符" style="zoom:67%;" />




```python
x = (10 and 20)
y = (10 or 20)
z = (-1 or 20)

x, y, z, not x, not(not x) 
```


    (20, 10, -1, False, True)



#### 2.6. 位运算符

<img src="C:\Users\13398\Desktop\位运算符.png" alt="位运算符" style="zoom:67%;" />

<br>
Python 中的整数本来就是以二进制形式存在的，下面的小例子可以说明：


```python
a = int('0b1010', 2)    # a = 10
b = int('0b1101', 2)    # b = 13
c = int('0b1000', 2)    # c = 8
d = int('0b1111', 2)    # d = 15
e = int('0b0111', 2)    # e = 7

a, b , a&b == c, a|b == d, a^b == e
```


    (10, 13, True, True, True)




```python
a = 2
a << 1, a >> 1 # 左移1位等价于2倍， 右移1位等于除以2
```


    (4, 1)



#### 2.7. 成员运算符

<img src="C:\Users\13398\Desktop\成员运算符.png" alt="成员运算符" style="zoom:67%;" />


```python
a = 10
b = 20
list = [1, 2, 3, 4, 5 ];
 
if ( a in list ):
   print ("1 - 变量 a 在给定的列表中 list 中")
else:
   print ("1 - 变量 a 不在给定的列表中 list 中")
 
if ( b not in list ):
   print ("2 - 变量 b 不在给定的列表中 list 中")
else:
   print ("2 - 变量 b 在给定的列表中 list 中")
 
# 修改变量 a 的值
a = 2
if ( a in list ):
   print ("3 - 变量 a 在给定的列表中 list 中")
else:
   print ("3 - 变量 a 不在给定的列表中 list 中")
```

    1 - 变量 a 不在给定的列表中 list 中
    2 - 变量 b 不在给定的列表中 list 中
    3 - 变量 a 在给定的列表中 list 中

#### 2.8. 身份运算符

<img src="C:\Users\13398\Desktop\身份运算符.png" alt="身份运算符" style="zoom:67%;" />

>Note: id() 函数用于获取对象内存地址


```python
a = 20 
b = 20
c = 30

if(a == b and id(a) == id(b) and a is not c):
    print("C'est La Vie")
```

    C'est La Vie

#### 2.9. 运算符优先级

<img src="C:\Users\13398\Desktop\运算符优先级.png" alt="运算符优先级" style="zoom:67%;" />

- and 拥有更高优先级


```python
x = True
y = False
z = False
 
if x or y and z:
    print("yes")
else:
    print("no")
```

    yes


## 3. 基本数据类型
Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

**Python3 的六个标准数据类型中：**

- 不可变数据（3 个）：Number（数字）、String（字符串）、Tuple（元组）；
- 可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）。

#### 3.1. 数字

 - Python3 支持 **int、float、bool、complex**
 - Python3 没有 **long**
 - `type() | isinstance()`两种函数可以用来查看变量类型


```python
a, b, c = 20, 5.5, "yhousuc"
type(a), type(b), type(c)
```


    (int, float, str)




```python
a, b, c = 20, 5.5, "yhousuc"
isinstance(a, int), isinstance(b, float), isinstance(c, str)
```


    (True, True, True)



`Type()` 与 `isinstance()`之间的区别<br>
- `type` 不会认为子类是一种父类类型
- `isinstance` 认为子类是一种父类类型


```python
class A:
    pass
class B(A):
    pass

a, b = A(), B()

type(a) == A, type(b) == A, isinstance(a, A), isinstance(b, A)
```


    (True, False, True, True)



- `del` 删除对象


```python
a = 3
del a
a
```


    ---------------------------------------------------------------------------
    
    NameError                                 Traceback (most recent call last)
    
    <ipython-input-14-4c6f95158088> in <module>
          1 a = 3
          2 del a
    ----> 3 a


    NameError: name 'a' is not defined


- 复数表示(两种)
 - a + bj
 - complex(a, b)
 - 复数的实部和虚部都是浮点型


```python
x = 1 + 2j
y = complex(1, 2)
x, y
```

#### 3.2. 字符串

Python中的字符串用单引号 ' 或双引号 " 括起来，同时使用反斜杠 \ 转义特殊字符。
- 字符串可以切片操作
- 索引值以 0 为开始值，-1 为从末尾的开始位置。
- 加号 + 是字符串的连接符， 星号 * 表示复制当前字符串，与之结合的数字为复制的次数。
- Python 使用反斜杠 \ 转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 r，表示原始字符串

#### 3.3. 列表


- 列表可以完成大多数集合类的数据结构实现。列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）。

- 列表是写在方括号 [] 之间、用逗号分隔开的元素列表。

- 和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。

- 列表可以切片
- 列表元素可以改变

#### 3.4. 元组

- 元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 () 里，元素之间用逗号隔开。

- 元组中的元素类型也可以不相同：

#### 3.5. 字典
- 字典（dictionary）是Python中另一个非常有用的内置数据类型。

- 列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。

- 字典是一种映射类型，字典用 { } 标识，它是一个**无序**的 **键(key) : 值(value)** 的集合。

- 键(key)必须使用不可变类型。

- 在同一个字典中，键(key)必须是唯一的。


```python
# 输出字典key
dict = {}
dict['a'] = 123
dict['b'] = 213
dict.keys()
```

#### 3.6. 集合

- 集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。

- 基本功能是进行成员关系测试和**删除重复元素**。

- 可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。



```python
# 去重功能
set1 = set()
set1.add(1)
set1.add(1)
set1
```

#### 3.7 数据类型转换

![数据类型转换](C:\Users\13398\Desktop\数据类型转换.png)



## 4. 练习题


- 怎样对python中的代码进行注释？
 - \#  '''多行注释内容 '''  """ 多行注释内容 """
- python有哪些运算符，这些运算符的优先级是怎样的？
 - 单算移关于， 异或逻条赋
- python 中 is, is not 与 ==, != 的区别是什么？
 - is 等价于 id(), == 、!= 是值之间的比较， 身份运算符 与 关系运算符
- python 中包含哪些数据类型？这些数据类型之间如何转换？
 - 数字、列表、元组、字典、字符串
 - 转换见上表


```python
a = 1
b = 1
a is b, a is not b
```


    (True, False)



## 5. LeetCode.136 只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4


```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res = 0
        for x in nums:
            res = res ^ x
        return res
```
