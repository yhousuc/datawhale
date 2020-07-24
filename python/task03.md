# 异常处理

## try - except 语句

try 语句按照如下方式工作：

- 首先，执行try子句（在关键字try和关键字except之间的语句）
- 如果没有异常发生，忽略except子句，try子句执行后结束。
- 如果在执行try子句的过程中发生了异常，那么try子句余下的部分将被忽略。如果异常的类型和except之后的名称相符，那么- - 对应的except子句将被执行。最后执行try语句之后的代码。
- 如果一个异常没有与任何的except匹配，那么这个异常将会传递给上层的try中。


```python
try:
    f = open('test.txt')
    print(f.read())
except OSError:
    print("文件打开出错")
```

    文件打开出错
    


```python
try:
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError as error:
    print('打开文件出错\n原因是：' + str(error))

# 打开文件出错
# 原因是：[Errno 2] No such file or directory: 'test.txt'
```

    打开文件出错
    原因是：[Errno 2] No such file or directory: 'test.txt'
    

一个try语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。


```python
try:
    int("abc")
    s = 1 + '1'
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError as error:
    print('打开文件出错\n原因是：' + str(error))
except TypeError as error:
    print('类型出错\n原因是：' + str(error))
except ValueError as error:
    print('数值出错\n原因是：' + str(error))

# 数值出错
# 原因是：invalid literal for int() with base 10: 'abc'
```

    数值出错
    原因是：invalid literal for int() with base 10: 'abc'
    


```python
dict1 = {'a': 1, 'b': 2, 'v': 22}
try:
    x = dict1['y']
except LookupError:
    print('查询错误')
except KeyError:
    print('键错误')
else:
    print(x)

# 查询错误
```

    查询错误
    

try-except-else语句尝试查询不在dict中的键值对，从而引发了异常。这一异常准确地说应属于KeyError，但由于KeyError是LookupError的子类，且将LookupError置于KeyError之前，因此程序优先执行该except代码块。所以，使用多个except代码块时，必须坚持对其规范排序，要从最具针对性的异常到最通用的异常。


```python
dict1 = {'a': 1, 'b': 2, 'v': 22}
try:
    x = dict1['y']
except LookupError:
    print('查询错误')
except KeyError:
    print('键错误')
else:
    print(x)
# 查询错误
```

    查询错误
    


```python
dict1 = {'a': 1, 'b': 2, 'v': 22}
try:
    x = dict1['y']
except KeyError:
    print('键错误')
except LookupError:
    print('查询错误')
else:
    print(x)

# 键错误
```

    键错误
    

【例子】一个 except 子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组。


```python
try:
    s = 1 + '1'
    int("abc")
    f = open('test.txt')
    print(f.read())
    f.close()
except (OSError, TypeError, ValueError) as error:
    print('出错了！\n原因是：' + str(error))
# 出错了！
# 原因是：unsupported operand type(s) for +: 'int' and 'str'
```

    出错了！
    原因是：unsupported operand type(s) for +: 'int' and 'str'
    

## 4. try - catch - finally 语句

- 不管try子句里面有没有发生异常，finally子句都会执行。

- 如果一个异常在try子句里被抛出，而又没有任何的except把它截住，那么这个异常会在finally子句执行后被抛出。


```python
def divide(x, y):
    try:
        result = x / y
        print("result is", result)
    except ZeroDivisionError:
        print("division by zero!")
    finally:
        print("executing finally clause")


divide(2, 1)
# result is 2.0
# executing finally clause
divide(2, 0)
# division by zero!
# executing finally clause
divide("2", "1")
# executing finally clause
# TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

    result is 2.0
    executing finally clause
    division by zero!
    executing finally clause
    executing finally clause
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-8-16805cf48925> in <module>
         15 # division by zero!
         16 # executing finally clause
    ---> 17 divide("2", "1")
         18 # executing finally clause
         19 # TypeError: unsupported operand type(s) for /: 'str' and 'str'
    

    <ipython-input-8-16805cf48925> in divide(x, y)
          1 def divide(x, y):
          2     try:
    ----> 3         result = x / y
          4         print("result is", result)
          5     except ZeroDivisionError:
    

    TypeError: unsupported operand type(s) for /: 'str' and 'str'


### 5. try - except - else 语句

- 如果在try子句执行时没有发生异常，Python将执行else语句后的语句。

- 使用except而不带任何异常类型，这不是一个很好的方式，我们不能通过该程序识别出具体的异常信息，因为它捕获所有的异常。


```python
try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print("Error: 没有找到文件或读取文件失败")
else:
    print("内容写入文件成功")
    fh.close()

# 内容写入文件成功
```

    内容写入文件成功
    

注意：else语句的存在必须以except语句的存在为前提，在没有except语句的try语句中使用else语句，会引发语法错误。

### 6. raise语句
Python 使用raise语句抛出一个指定的异常。


```python
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    
# An exception flew by!
```

    An exception flew by!
    

### 练习题：

1、猜数字游戏

题目描述:

电脑产生一个零到100之间的随机数字，然后让用户来猜，如果用户猜的数字比这个数字大，提示太大，否则提示太小，当用户正好猜中电脑会提示，"恭喜你猜到了这个数是......"。在用户每次猜测之前程序会输出用户是第几次猜测，如果用户输入的根本不是一个数字，程序会告诉用户"输入无效"。

(尝试使用try catch异常处理结构对输入情况进行处理)

获取随机数采用random模块。


```python
import random
count = 1
x = random.randint(0, 100)
while(1):
    try:
        a = int(input("第%d次猜，请输入整形数字：" %count))
        
        if a == x:
            print("恭喜你猜到了这个数是", a)
            break
        elif a > x:
            print("太大")
        else:
            print("太小")
    except TypeError:
        print("输入无效")
    finally:
        count += 1

```

    第1次猜，请输入整形数字：50
    太小
    第2次猜，请输入整形数字：75
    太大
    第3次猜，请输入整形数字：65
    太大
    第4次猜，请输入整形数字：60
    太大
    第5次猜，请输入整形数字：55
    太小
    第6次猜，请输入整形数字：56
    恭喜你猜到了这个数是 56
    
