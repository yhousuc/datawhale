# 条件循环结构

### 1. if 语句

### 2. if-else 语句

### 3. if-elif-else 语句

### 4. assert 断言


```python
a = ""
assert(len(a))
print("xxxx")
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-2-8dd4efec1c9f> in <module>
          1 a = ""
    ----> 2 assert(len(a))
          3 print("xxxx")
    

    AssertionError: 


会报相应的（AssertionError）


```python
a = "123"
assert(len(a))
print("xxxx")
```

    xxxx
    

# 循环语句

### 5. while 循环语句


```python
a = 5
while(a):
    print("hello world")
    a -= 1
```

    hello world
    hello world
    hello world
    hello world
    hello world
    

### 6. while-else 循环


```python
a = 5
while(a):
    print("hello world")
    a -= 1
else:
    print("hello datawhale")
```

    hello world
    hello world
    hello world
    hello world
    hello world
    hello datawhale
    

### 7. for循环


```python
for x in "i am yhousuc":
    print(x)
```

    i
     
    a
    m
     
    y
    h
    o
    u
    s
    u
    c
    

### 8. for-else循环


```python
nums = [1,2,3,4,5]
for num in nums:
    print(num)
else:
    print("list loop over")
```

    1
    2
    3
    4
    5
    list loop over
    

### 9.range 函数

range(start, end, step), 其中start为起始数(默认为0)，end为终止数（不包含），step为步长，默认为1


```python
for i in range(5):
    print(i)
```

    0
    1
    2
    3
    4
    


```python
for i in range(1, 5):
    print(i)
```

    1
    2
    3
    4
    


```python
for i in range(1,5,2):
    print(i)
```

    1
    3
    

### 10. enumerate()函数， 枚举函数
enumerate(sequence, \[start=0])


```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
lst = list(enumerate(seasons))
print(lst)
# [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
lst = list(enumerate(seasons, start=1))  # 下标从 1 开始
print(lst)
# [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

    [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
    [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
    

### 11. break 语句


```python
a = 5
for i in range(a):
    if(i == 3):
        break
    print("xxxxxxxx")
```

    xxxxxxxx
    xxxxxxxx
    xxxxxxxx
    

### 11. continue 语句


```python
for i in range(10):
    if i % 2 != 0:
        print(i)
        continue
    i += 2
    print(i)

# 2
# 1
# 4
# 3
# 6
# 5
# 8
# 7
# 10
# 9
```

    2
    1
    4
    3
    6
    5
    8
    7
    10
    9
    

### 12. continue 语句

### 13. pass语句


