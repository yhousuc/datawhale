# 列表
## 简单数据类型
整型<class 'int'>
浮点型<class 'float'>
布尔型<class 'bool'>

## 容器数据类型
列表<class 'list'>
元组<class 'tuple'>
字典<class 'dict'>
集合<class 'set'>
字符串<class 'str'>

##向列表添加数据
```python
x.append('tuesday')
```
- 此元素如果是一个 list，那么这个 list 将作为一个整体进行追加，注意append()和extend()的区别
- list.extend(seq) 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
```python
x = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
x.extend(['Thursday', 'Sunday'])
print(x)  
# ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Thursday', 'Sunday']

print(len(x))  # 7
```
- 严格来说 append 是追加，把一个东西整体添加在列表后，而 extend 是扩展，把一个东西里的所有元素添加在列表后。

- list.insert(index, obj) 在编号 index 位置插入 obj。
```python
x = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
x.insert(2, 'Sunday')
print(x)
# ['Monday', 'Tuesday', 'Sunday', 'Wednesday', 'Thursday', 'Friday']

print(len(x))  # 6
```
## 删除
list.remove(obj) 移除列表中某个值的第一个匹配项
list.pop([index=-1]) 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值
```python
x = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y = x.pop()
print(y)  # Friday

y = x.pop(0)
print(y)  # Monday

y = x.pop(-2)
print(y)  # Wednesday
print(x)  # ['Tuesday', 'Thursday']
```
- remove 和 pop 都可以删除元素，前者是指定具体要删除的元素，后者是指定一个索引。
- 如果知道要删除的元素在列表中的位置，可使用del语句。
## 索引
```python
x = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
print(x[3:])  # ['Thursday', 'Friday']
print(x[-3:])  # ['Wednesday', 'Thursday', 'Friday']

3 “：”复制所有元素
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
print(week[:])  
# ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
```

## 常用操作符
- 等号操作符：==
- 连接操作符 +
- 重复操作符 *
- 成员关系操作符 in、not in
- 「等号 ==」，只有成员、成员位置都相同时才返回True。

列表拼接有两种方式，用「加号 +」和「乘号 *」，前者首尾拼接，后者复制拼接。
```python
list1 = [123, 456]
list2 = [456, 123]
list3 = [123, 456]

print(list1 == list2)  # False
print(list1 == list3)  # True

list4 = list1 + list2  # extend()
print(list4)  # [123, 456, 456, 123]

list5 = list3 * 3
print(list5)  # [123, 456, 123, 456, 123, 456]

list3 *= 3
print(list3)  # [123, 456, 123, 456, 123, 456]

print(123 in list3)  # True
print(456 not in list3)  # False
```
## 其它
- list.count(obj) 统计某个元素在列表中出现的次数
- list.index(x[, start[, end]]) 从列表中找出某个值第一个匹配项的索引位置 
- list.reverse() 反向列表中元素
- list.sort(key=None, reverse=False) 对原列表进行排序
reverse -- 排序规则，reverse = True 降序， reverse = False 升序（默认）。
```python
x = [123, 456, 789, 213]
x.sort()
print(x)
# [123, 213, 456, 789]

x.sort(reverse=True)
print(x)
# [789, 456, 213, 123]


# 获取列表的第二个元素
def takeSecond(elem):
    return elem[1]


x = [(2, 2), (3, 4), (4, 1), (1, 3)]
x.sort(key=takeSecond)
print(x)
# [(4, 1), (2, 2), (1, 3), (3, 4)]

x.sort(key=lambda a: a[0])
print(x)
# [(1, 3), (2, 2), (3, 4), (4, 1)]
```