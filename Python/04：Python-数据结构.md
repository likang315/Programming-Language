### Python-数据结构

------

[TOC]

##### 01：列表

###### 函数

| 方法              | 描述                                                         |
| :---------------- | :----------------------------------------------------------- |
| list.append(x)    | 把一个元素添加到列表的结尾，相当于 a[len(a):] = [x]。        |
| list.extend(L)    | 通过添加指定列表的所有元素来扩充列表，相当于 a[len(a):] = L。 |
| list.insert(i, x) | 在指定位置插入一个元素。第一个参数是准备插入到其前面的那个元素的索引，例如 a.insert(0, x) 会插入到整个列表之前，而 a.insert(len(a), x) 相当于 a.append(x) 。 |
| list.remove(x)    | 删除列表中值为 x 的第一个元素。如果没有这样的元素，就会返回一个错误。 |
| list.pop([i])     | 从列表的指定位置移除元素，并将其返回。如果没有指定索引，a.pop()返回最后一个元素。元素随即从列表中被移除。（方法中 i 两边的方括号表示这个参数是可选的，而不是要求你输入一对方括号，你会经常在 Python 库参考手册中遇到这样的标记。） |
| list.clear()      | 移除列表中的所有项，等于del a[:]。                           |
| list.index(x)     | 返回列表中第一个值为 x 的元素的索引。如果没有匹配的元素就会返回一个错误。 |
| list.count(x)     | 返回 x 在列表中出现的次数。                                  |
| list.sort()       | 对列表中的元素进行排序。                                     |
| list.reverse()    | 倒排列表中的元素。                                           |
| list.copy()       | 返回列表的浅复制，等于a[:]。                                 |

###### 列表推导式

- 每个列表推导式都在 for 之后跟一个表达式，然后有零到多个 for 或 if 子句。返回结果是一个**根据表达从其后的 for 和 if 上下文环境中**生成出来的列表。

- ```python
  vec = [2, 4, 6]
  >>> [3*x for x in vec]
  [6, 12, 18]
  [3*x for x in vec if x > 3]
  [12, 18]
  ```

###### 嵌套列表解析

- ```python
  matrix = [
  ...     [1, 2, 3, 4],
  ...     [5, 6, 7, 8],
  ...     [9, 10, 11, 12],
  ... ]
  
  [[row[i] for row in matrix] for i in range(4)]
  [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
  ```

###### del语句

-  使用 del 语句可以从一个列表中根据索引来删除一个元素，而不是值来删除元素，

- 也可以用 del 删除实体变量；

- 用 del 语句从列表中删除一个切割，或清空整个列表

- ```python
  >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
  >>> del a[0]
  >>> a
  [1, 66.25, 333, 333, 1234.5]
  >>> del a[2:4]
  >>> a
  [1, 66.25, 1234.5]
  >>> del a[:]
  >>> a
  []
  
  del a
  ```

##### 02：元组和序列

- 元组由若干逗号分隔的值组成；
- 元组在输出时总是有括号的，以便于正确表达嵌套结构。在输入时可能有或没有括号， 不过括号通常是必须的

##### 03：集合

- 用大括号({})创建集合

- 集合也支持推导式

- ```python
  a = {x for x in 'abracadabra' if x not in 'abc'}
  >>> a
  {'r', 'd'
  ```

##### 04：字典

- 一对大括号创建一个空的字典：{}。

- 构造函数 dict() 直接从键值对元组列表中构建字典；

- 字典推导可以用来创建任意键和值的表达式词典；

- 如果关键字只是简单的字符串，使用关键字参数指定键值对有时候更方便；

- ```python
  tel = {'jack': 4098, 'sape': 4139}
  >>> tel['guido'] = 4127
  
  dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
  {'sape': 4139, 'jack': 4098, 'guido': 4127}
  
  >>> {x: x**2 for x in (2, 4, 6)}
  {2: 4, 4: 16, 6: 36}
  
  >>> dict(sape=4139, guido=4127, jack=4098)
  {'sape': 4139, 'jack': 4098, 'guido': 4127}
  ```

##### 05：遍历技巧

- 在字典中遍历时，关键字和对应的值可以使用 items() 方法同时解读出来

  - ```python
    knights = {'gallahad': 'the pure', 'robin': 'the brave'}
    >>> for k, v in knights.items():
    ...     print(k, v)
    ```

- 在序列中列表时，索引位置和对应值可以使用 enumerate() 函数同时得到：

  - ```python
    for i, v in enumerate(['tic', 'tac', 'toe']):
    ...     print(i, v)
    ```

- 同时遍历两个或更多的序列，可以使用 zip() 组合：

  - ```python
    questions = ['name', 'quest', 'favorite color']
    >>> answers = ['lancelot', 'the holy grail', 'blue']
    >>> for q, a in zip(questions, answers):
    ...     print('What is your {0}?  It is {1}.'.format(q, a))
    ```

- 要反向遍历一个序列，首先指定这个序列，然后调用 reversed() 函数：

  - ```python
    >>> for i in reversed(range(1, 10, 2)):
    ...     print(i)
    ```

- 要按顺序遍历一个序列，使用 sorted() 函数返回一个已排序的序列，并不修改原值：

  - ```python
    >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
    >>> for f in sorted(set(basket)):
    ...     print(f)
    ```



