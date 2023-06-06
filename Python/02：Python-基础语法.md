### Python 基础语法

------

[TOC]

##### 01：Python 基本数据类型概述

- Python 中的**变量不需要声明**。每个变量在使用前都必须赋值，**变量赋值以后该变量才会被创建**。在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

###### 多个变量赋值

- ```python
  a = b = c = 1
  a, b, c = 1, 2, "runoob"
  ```

##### 02：标准数据类型

- Python3 的六个标准数据类型中：
  - **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
  - **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）；

###### Number【数字】

- Python3 支持 **int、float、bool、complex（复数）**。在Python 3里，只有一种整数类型 int，表示为长整型;
- 内置的 type() 函数可以用来查询变量所指的对象类型。
- 此外还可以用 isinstance 来判断：
  - type()不会认为子类是一种父类类型。
  - isinstance()会认为子类是一种父类类型。
- Python3 中，bool 是 int 的子类，True 和 False 可以和数字相加，**True==1、False==0** *会返回* **True**，但可以通过* **is** *来判断类型。
- del语句：删除单个或多个对象
  - del var1[,var2[,var3[....,varN]]]

###### 数值运算

- 数值的除法包含两个运算符：**/** 返回一个浮点数，**//** 返回一个整数。
- 在混合计算时，Python会把整型转换成为浮点数。

###### String 【字符串】

- 详见基础语法

###### List【列表】

- 定义：和Java一致；

- 列表的截取方法：变量[头下标:尾下标]

  - 索引值以 **0** 为开始值，**-1** 为从末尾的开始位置。

- Python 列表截取可以接收第三个参数，参数作用是**截取的步长**，以下实例在索引 1 到索引 4 的位置并设置为步长为 2（间隔一个位置）来截取字符串；

- 如果第三个参数为负数表示逆向读取，以下实例用于翻转字符串：

  - 

- ```python
  #!/usr/bin/python3
  
  list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
  tinylist = [123, 'runoob']
  
  print (list)            # 输出完整列表
  print (list[0])         # 输出列表第一个元素
  print (list[1:3])       # 从第二个开始输出到第三个元素
  print (list[2:])        # 输出从第三个元素开始的所有元素
  print (tinylist * 2)    # 输出两次列表
  print (list + tinylist) # 连接列表
  
  # inputWords[-1::-1] 有三个参数
  # 第一个参数 -1 表示最后一个元素
  # 第二个参数为空，表示移动到列表末尾
  # 第三个参数为步长，-1 表示逆向
  inputWords=inputWords[-1::-1]
  ```

###### Tuple【元组】

- 元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 **()** 里，元素之间用逗号隔开。

- 虽然tuple的元素不可改变，但它可以包含可变的对象，比如list列表。

- **构造包含 0 个或 1 个元素的元组**比较特殊，所以有一些额外的语法规则：

  ```python
  tup1 = ()    # 空元组
  tup2 = (20,) # 一个元素，需要在元素后添加逗号
  ```

###### Set【集合】

- 使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

- ```python
  #!/usr/bin/python3
  
  sites = {'Google', 'Taobao', 'Runoob', 'Facebook', 'Zhihu', 'Baidu'}
  print(sites)   # 输出集合，重复的元素被自动去掉
  # 成员测试
  if 'Runoob' in sites :
      print('Runoob 在集合中')
  else :
      print('Runoob 不在集合中')
  
  # set可以进行集合运算
  a = set('abracadabra')
  b = set('alacazam')
  
  print(a)
  print(a - b)     # a 和 b 的差集
  print(a | b)     # a 和 b 的并集
  print(a & b)     # a 和 b 的交集
  print(a ^ b)     # a 和 b 中不同时存在的元素
  ```

###### Dictionary【字典】

- 字典是一种映射类型，字典用 **{ }** 标识，它是一个无序的 **键(key) : 值(value)** 的集合。
- 构造函数 dict() 可以直接从键值对序列中构建字典；

##### 03：数据类型转换

- Python数据类型转换可以分为：
  - 隐式类型转换 - 自动完成
  - 显式类型转换 - 需要使用类型函数来转换

###### 类型函数

- | 函数                     | 描述                                                |
  | :----------------------- | :-------------------------------------------------- |
  | **[int(x [,base\])]**    | 将x转换为一个整数，base 指转换后的进制              |
  | **[float(x)]**           | 将x转换到一个浮点数                                 |
  | [complex(real [,imag\])] | 创建一个复数                                        |
  | **[str(x)]**             | 将对象 x 转换为字符串                               |
  | [repr(x)                 | 将对象转化为供解释器读取的形式                      |
  | **[eval(str)]**          | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
  | [tuple(s)]               | 将序列 s 转换为一个元组                             |
  | [list(s)]                | 将序列 s 转换为一个列表                             |
  | [set(s)]                 | 转换为可变集合                                      |
  | [dict(d)]                | 创建一个字典。d 必须是一个 (key, value)元组序列。   |
  | [frozenset(s)]           | 转换为不可变集合                                    |
  | **[chr(x)]**             | 将一个整数转换为一个字符                            |
  | [ord(x)]                 | 将一个字符转换为它的整数值                          |
  | [hex(x)]                 | 将一个整数转换为一个十六进制字符串                  |
  | [oct(x)]                 | 将一个整数转换为一个八进制字符串                    |

##### 04：Python 推导式

- Python 推导式是一种独特**的数据处理方式**，可以从一个数据序列构建另一个新的数据序列的结构体。

- Python 支持各种数据结构的推导式：

  - 列表(list)推导式
  - 字典(dict)推导式
  - 集合(set)推导式
  - 元组(tuple)推导式

- ```python
  [表达式 for 变量 in 列表] 
  [out_exp_res for out_exp in input_list]
  
  或者 
  
  [表达式 for 变量 in 列表 if 条件]
  [out_exp_res for out_exp in input_list if condition]
  ```

  - out_exp_res：列表生成元素表达式，可以是有返回值的函数。
  - for out_exp in input_list：迭代 input_list 将 out_exp 传入到 out_exp_res 表达式中。
  - if condition：条件语句，可以过滤列表中不符合条件的值。

##### 05：Python3 解释器

- Linux/Unix的系统上，一般默认的 python 版本为 2.x，我们可以将 python3.x 安装在 **/usr/local/python3** 目录中。安装完成后，我们可以将路径 **/usr/local/python3/bin** 添加到您的 Linux/Unix 操作系统的环境变量中，这样您就可以通过 shell 终端输入命令来启动 Python3 。

###### 交互式编程

- 在终端中输入"Python"命令来启动**Python解释器**；
- 当键入一个多行结构时，续行是必须的。

###### 脚本式编程

- 将代码拷贝至 **script.py**文件中：

  ```python
  python3 script.py
  ```

- 在Linux/Unix系统中，你可以在脚本顶部添加以下命令让Python脚本可以像SHELL脚本一样可直接执行

  - ```python
    #! /usr/bin/env python3
    
    # 然后修改脚本权限，使其有执行权限，命令：chmod +x hello.py
    ./hello.py
    ```

##### 06：Python3 注释

- Python 中单行注释以 **#** 开头，例如：

- 多行注释用三个单引号 **'''** 或者三个双引号 **"""** 将注释括起来

  - ```python
    # 这是一个注释 print("Hello, World!")
    #!/usr/bin/python3 
    '''
    这是多行注释，用三个单引号
    这是多行注释，用三个单引号 
    这是多行注释，用三个单引号
    '''
    print("Hello, World!")
    
    !/usr/bin/python3 
    """
    这是多行注释，用三个双引号
    这是多行注释，用三个双引号 
    这是多行注释，用三个双引号
    """
    print("Hello, World!")
    ```


##### 07：Python 3 的运算符

###### 算术运算符

- | 运算符 |                                                              |                                                              |
  | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | /      | 除 - x 除以 y                                                | b / a 输出结果 2.1                                           |
  | **     | 幂 - 返回x的y次幂                                            | a**b 为10的21次方                                            |
  | //     | 取整除 - 向下取接近商的整数                                  |                                                              |
  | **=    | 幂赋值运算符                                                 | c **= a 等效于 c = c ** a                                    |
  | //=    | 取整除赋值运算符                                             | c //= a 等效于 c = c // a                                    |
  | :=     | 海象运算符，可在表达式内部为变量赋值。**Python3.8 版本新增运算符**。 | 在这个示例中，赋值表达式可以避免调用 len() 两次:`if (n := len(a)) > 10:    print(f"List is too long ({n} elements, expected <= 10)")` |

###### 逻辑运算符

- | 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
  | :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
  | and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 x 的值，否则返回 y 的计算值。 | (a and b) 返回 20。     |
  | or     | x or y     | 布尔"或" - 如果 x 是 True，它返回 x 的值，否则它返回 y 的计算值。 | (a or b) 返回 10。      |
  | not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |

###### Python成员运算符

- | 运算符 | 描述                                                    | 实例                                              |
  | :----- | :------------------------------------------------------ | :------------------------------------------------ |
  | in     | 如果在指定的序列中找到值返回 True，否则返回 False。     | x 在 y 序列中 , 如果 x 在 y 序列中返回 True。     |
  | not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。 | x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。 |

###### Python身份运算符

- | 运算符 | 描述                                        | 实例                                                         |
  | :----- | :------------------------------------------ | :----------------------------------------------------------- |
  | is     | is 是判断两个标识符是不是引用自一个对象     | **x is y**, 类似 **id(x) == id(y)** , 如果引用的是同一个对象则返回 True，否则返回 False |
  | is not | is not 是判断两个标识符是不是引用自不同对象 | **x is not y** ， 类似 **id(x) != id(y)**。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

##### 08：Python 条件控制

- IF 语句

  - ```python
    if condition_1:
        statement_block_1
    elif condition_2:
        statement_block_2
    else:
        statement_block_3
    ```

  - 每个条件后面要使用冒号 **:**，表示接下来是满足条件后要执行的语句块。

  - 使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块。

  - 在Python中没有switch – case语句。

##### 09：Python 循环结构

- while

- while 循环使用 else 语句

  - ```python
    while 判断条件(condition)：
        执行语句(statements)……
        
    while <expr>:
        <statement(s)>
    else:
        <additional_statement(s)>
    
    ```

- For 循环

  - ```python
    for <variable> in <sequence>:
        <statements>
    else:
        <statements>
    ```

- range()函数

  - ```python
    # 如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列
    for i in range(5):
         print(i)
            
    # 使用range指定区间的值：
    for i in range(5,9) :
     
    # 定数字开始并指定不同的增量（步长）
    for i in range(0, 10, 3) :
    ```

- break 和 continue 语句及循环中的 else 子句

  - break 和 continue  和Java 一致；
  - 循环语句可以有 else 子句，它在穷尽列表(以for循环)或条件变为 false (以while循环)导致循环终止时被执行，但循环被 break 终止时不执行；

- pass 语句

  - Python pass是空语句，是为了保持程序结构的完整性，pass 不做任何事情，一般用做占位语；

##### 10：Python 迭代器和生成器

###### 迭代器

- ```python
  list=[1,2,3,4]
  >>> it = iter(list)    # 创建迭代器对象
  >>> print (next(it))   # 输出迭代器的下一个元素
  1
  >>> print (next(it))
  2
  # 使用常规for语句进行遍历
  list=[1,2,3,4]
  it = iter(list)    # 创建迭代器对象
  for x in it:
      print (x, end=" ")
  ```

- StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况，在 __next__() 方法中我们可以设置在完成指定循环次数后触发 StopIteration 异常来结束迭代。

###### 生成器

- 在 Python 中，使用了 yield 的函数被称为生成器（generator）。
- 调用一个生成器函数，返回的是一个迭代器对象。

##### 11：Python3 模块【Java 包】

-  Python 提供了一个办法，把这些定义存放在文件中，为一些脚本或者交互式的解释器实例使用，**这个文件被称为模块**。

- 模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。

- ```python
  #!/usr/bin/python3
  # 文件名: using_sys.py
   
  import sys
   
  print('命令行参数如下:')
  for i in sys.argv:
     print(i)
   
  print('\n\nPython 路径为：', sys.path, '\n')
  ```

  1. import sys 引入 python 标准库中的 sys.py 模块；这是引入某一模块的方法。
  2. sys.argv 是一个包含命令行参数的列表。
  3. sys.path 包含了一个 **Python 解释器自动查找所需模块的路径的列表**;

##### 12：impot

- 要使用 **Python 源文件**，只需在另一个源文件里执行 import 语句；

- ```python
  import module1[, module2[,... moduleN]
  ```

- **搜索路径：**是一个解释器会先进行搜索的所有目录的列表。如想要导入模块 support，需要把命令放在脚本的顶端

  - ```python
    #!/usr/bin/python3
    # Filename: test.py
     
    # 导入模块
    import sys
     
    # 现在可以调用模块里包含的函数了
    sys.path
    ```

##### 13：from ... import *

- 把一个模块的所有内容全都导入到当前的命名空间，但是那些由单一下划线（_）开头的名字不在此例；

- ```python
  from modename import *
  ```

##### 14：_ name _ 属性

- 一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想**在模块被引入时，模块中的某一程序块不执行**，我们可以用_ name _属性来使该程序块仅在该模块自身运行时执行。

- 每个模块都有一个__name__属性，当其值是'__main__'时，表明该模块自身在运行，否则是被引入。

- ```python
  #!/usr/bin/python3
  # Filename: using_name.py
  
  if __name__ == '__main__':
     print('程序自身在运行')
  else:
     print('我来自另一模块')
     
  $ python using_name.py
  程序自身在运行
  $ python
  >>> import using_name
  我来自另一模块
  >>>
  ```

##### 15：dir() 函数

- 内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回:
- 如果没有给定参数，那么 dir() 函数会罗列出当前定义的所有名称；

##### 16：包【Java的包】

- 包是一种管理 Python 模块命名空间的形式，采用"点模块名称"。
- 比如一个模块的名称是 A.B， 那么他表示一个包 A中的子模块 B 。
- **from package import item** 这种形式的时候，对应的 item 既可以是包里面的子模块（子包），或者包里面定义的其他名称，比如函数，类或者变量。import 语法会首先把 item 当作一个包定义的名称，如果没找到，再试图按照一个模块去导入。如果还没找到，抛出一个 **:exc:ImportError** 异常。
- 如果使用形如 **import item.subitem.subsubitem** 这种导入形式，除了最后一项，都必须是包，而最后一项则可以是模块或者是包，但是不可以是类，函数或者变量的名字。

###### 示例

```python
import sound.effects.echo
# 这将会导入子模块:sound.effects.echo。 他必须使用全名去访问:
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)


# 这同样会导入子模块: echo，并且他不需要那些冗长的前缀，所以他可以这样使用:
from sound.effects import echo
echo.echofilter(input, output, delay=0.7, atten=4)
```

##### 17：从一个包中导入 *

- **from sound.effects import ***

- 导入语句遵循如下规则：如果**包定义文件`__init.py__`** 存在一个叫做 **__all__** 的列表变量，那么在使用 **from package import \*** 的时候就把这个列表中的所有名字作为包内容导入。

  - ```python
    __all__ = ["echo", "surround", "reverse"]
    
    from . import echo
    from .. import formats
    ```

- 无论是隐式的还是显式的相对导入都是从当前模块开始的。

- 主模块的名字永远是"__main__"，一个Python应用程序的主模块，应当总是使用绝对路径引用。

- 包还提供一个额外的属性__path__。这是一个目录列表。

##### 18：Python3 输入和输出

- 表达式语句、print() 函数和使用文件对象的 write() 方法，标准输出文件可以用 sys.stdout 引用；
  - **str()：** 函数返回一个用户易读的表达形式。
  - **repr()：** 产生一个解释器易读的表达形式。
    - 可选项 **:** 和格式标识符可以跟着字段名。 这就允许对值进行更好的格式化。 下面的例子将 Pi 保留到小数 点后三位
    - 在 **:** 后传入一个整数, 可以保证该域至少有这么多的宽度

###### 示例

```python
import math
>>> print('常量 PI 的值近似为 {0:.3f}。'.format(math.pi))
常量 PI 的值近似为 3.142。

>>> table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
>>> for name, number in table.items():
...     print('{0:10} ==> {1:10d}'.format(name, number))
...
Google     ==>          1

table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
>>> print('Runoob: {0[Runoob]:d}; Google: {0[Google]:d}; Taobao: {0[Taobao]:d}'.format(table))
Runoob: 2; Google: 1; Taobao: 3
```

###### 旧式字符串格式化

- **%** 操作符也可以实现字符串格式化。 它将**左边的参数**作为类似 sprintf() 式的格式化字符串, **而将右边的代入, 然后返回格式化后的字符串**。

- ```python
  >>> import math
  >>> print('常量 PI 的值近似为：%5.3f。' % math.pi)
  常量 PI 的值近似为：3.142。
  ```

##### 19：读取键盘输入

-  input() 函数接受一个标准输入数据，返回为 string 类型。

- ```python
  a = input("input:")
  input:123                  # 输入整数
  
  #!/usr/bin/python
  #输入三角形的三边长 输入多个值
  a,b,c = (input("请输入三角形三边的长：").split())
  a= int(a)
  b= int(b)
  c= int(c)
  ```

##### 20：pickle 模块

- python的pickle模块实现了基本的**数据序列化和反序列化**。

- ```python
  pickle.dump(obj, file, [,protocol])
  x = pickle.load(file)
  ```

##### 21：File 方法

###### open() 方法

- Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。
- open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。
- open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
  - 参数说明:
    - file: 必需，文件路径（相对或者绝对路径）。
    - mode: 可选，文件打开模式
    - buffering: 设置缓冲
    - encoding: 一般使用utf8
    - errors: 报错级别
    - newline: 区分换行符
    - closefd: 传入的file参数类型
    - opener: 设置自定义开启器，开启器的返回值必须是一个打开的文件描述符。

###### file 对象

- | 序号 | 方法及描述                                                   |
  | :--- | :----------------------------------------------------------- |
  | 1    | f**ile.close()：**关闭文件。关闭后文件不能再进行读写操作。   |
  | 2    | **file.flush()：**刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。 |
  | 3    | file.fileno()：返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。 |
  | 4    | file.isatty()：如果文件连接到一个终端设备返回 True，否则返回 False。 |
  | 5    | file.next()：**Python 3 中的 File 对象不支持 next() 方法。**返回文件下一行。 |
  | 6    | **file.read([size\])：**从文件读取指定的字节数，如果未给定或为负则读取所有。 |
  | 7    | file.readline([size\])：读取整行，包括 "\n" 字符。           |
  | 8    | file.readlines([sizeint\])]：读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区。 |
  | 9    | **file.seek(offset[, whence\])：**移动文件读取指针到指定位置 |
  | 10   | file.tell()：返回文件当前位置。                              |
  | 11   | file.truncate([size\])：从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 windows 系统下的换行代表2个字符大小。 |
  | 12   | **file.write(str)：**将字符串写入文件，返回的是写入的字符长度。 |
  | 13   | file.writelines(sequence)]：向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。 |

##### 22：OS 文件/目录方法

- **os** 模块提供了非常丰富的方法用来**处理文件和目录**。

- | 序号 | 方法及描述                                           |
  | :--: | :--------------------------------------------------- |
  |  1   | os.access(path, mode)： 检验权限模式                 |
  |  2   | os.chdir(path)： 改变当前工作目录                    |
  |  3   | os.chflags(path, flags)： 设置路径的标记为数字标记。 |
  |  4   | os.chmod(path, mode)：更改权限                       |
  |  5   | os.chown(path, uid, gid)：更改文件所有者             |
  |  6   | os.chroot(path)：改变当前进程的根目录                |
  |  7   | os.close(fd)： 关闭文件描述符 fd                     |
  |  8   | os.path 模块：获取文件的属性信息；                   |

##### 23：Python3 错误和异常

- Python 有两种错误很容易辨认：**语法错误和异常**。
- Python assert（断言）用于判断一个表达式，在表达式条件为 false 的时候触发异常。

##### 24：异常处理

###### try/except

- 异常捕捉可以使用 **try/except** 语句。

- 一个except子句可以**同时处理多个异常**，这些异常将被放在一个括号里成为一个元组；

- **最后一个except子句可以忽略异常的名称，它将被当作通配符使用**。你可以使用这种方法打印一个错误信息，然后再次把异常抛出。

  - ```python
    while True:
        try:
            x = int(input("请输入一个数字: "))
            break
        except ValueError:
            print("您输入的不是数字，请再次尝试输入！")
    ```

###### try/except ... else

- **try/except** 语句还有一个可选的 **else** 子句，如果使用这个子句，那么必须放在所有的 except 子句之后。
- else 子句将在 try 子句**没有发生任何异常的时候执行**。

###### try-finally 语句

- try-finally 语句**无论是否发生异常都将执行**最后的代码。

- ```python
  try:
      runoob()
  except AssertionError as error:
      print(error)
  else:
      try:
          with open('file.log') as file:
              read_data = file.read()
      except FileNotFoundError as fnf_error:
          print(fnf_error)
  finally:
      print('这句话，无论异常是否发生都会执行。')
  ```

###### 抛出异常

- Python 使用 raise 语句抛出一个指定的异常。

- raise 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 Exception 的子类）。

- ```python
  raise [Exception [, args [, traceback]]]
  ```

###### 用户自定义异常

- 可以通过创建一个新的异常类来拥有自己的异常。异常类继承自 Exception 类，可以直接继承，或者间接继承；

###### 预定义清理行为

- 一些对象定义了**标准的清理行为**，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行。

- **关键词 with 语句：**可以保证诸如文件之类的对象在使用完之后一定会正确的执行他的清理方法:

  - ```python
    with open("myfile.txt") as f:
        for line in f:
            print(line, end="")
    ```

##### 25：python3 命名空间

- 命名空间(Namespace)是从名称到对象的映射，大部分的命名空间都是通过 Python 字典来实现的。
- 命名空间提供了在项目中避免名字冲突的一种方法

###### 三种命名空间

- **内置名称（built-in names**）Python内置的名称，比如函数名 abs、char 和异常名称 BaseException、Exception 等。
- **全局名称（global names）**模块中定义的名称，记录了模块的变量，包括函数、类、导入的模块、模块级的变量和常量。
- **局部名称（local names）**函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）

###### 命名空间查找顺序

- 假设我们要使用变量 runoob，则 Python 的查找顺序为：**局部的命名空间去 -> 全局命名空间 -> 内置命名空间**。如果找不到变量 runoob，它将放弃查找并引发一个 NameError 异常；

###### 命名空间的生命周期

- 命名空间的生命周期取决于对象的作用域，如果对象执行完成，则该命名空间的生命周期就结束。

##### 26：作用域

- 作用域就是一个 Python 程序可以直接访问命名空间的正文区域；
- 变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称

###### 四种作用域

- **L（Local）**：最内层，包含局部变量，比如一个函数/方法内部。
- **E（Enclosing）**：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类） A 里面又包含了一个函数 B ，那么对于 B 中的名称来说 A 中的作用域就为 nonlocal。
- **G（Global）**：当前脚本的最外层，比如当前模块的全局变量。
- **B（Built-in）**： 包含了内建的变量/关键字等，最后被搜索。

###### 规则顺序

- 规则顺序： **L –> E –> G –> B**。
- 在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。
- 内置作用域是通过一个名为 **builtin 的标准模块**来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须**导入这个文件才能够使用它。**

###### 全局变量和局部变量【同Java】

- 定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

- ```python
  #!/usr/bin/python3
   
  total = 0 # 这是一个全局变量
  # 可写函数说明
  def sum( arg1, arg2 ):
      #返回2个参数的和."
      total = arg1 + arg2 # total在这里是局部变量.
      print ("函数内是局部变量 : ", total)
      return total
   
  #调用sum函数
  sum( 10, 20 )
  print ("函数外是全局变量 : ", total) # 30,0
  ```

###### global 和 nonlocal关键字

- 当内部作用域想修改外部作用域的变量时，就要用到 global 和 nonlocal 关键字了。

- global：修改全局变量 num；

- 如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字；

- ```python
  #!/usr/bin/python3
   
  num = 1
  def fun1():
      global num  # 需要使用 global 关键字声明
      print(num) 
      num = 123
      print(num)
  fun1()
  print(num)
  ```

##### 27：Python3 标准库

###### 操作系统接口

- **os模块**提供了不少与操作系统相关联的函数。
- 针对日常的文件和目录管理任务，**shutil 模块**提供了一个易于使用的高级接口。

###### 文件通配符

- **glob模块**提供了一个函数用于从目录通配符搜索中生成文件列表。

###### 命令行参数

- 通用工具脚本经常调用命令行参数。这些命令行参数以链表形式存储于 **sys 模块的 argv 变量**。

###### 错误输出重定向和程序终止

- sys 还有 stdin，stdout 和 stderr 属性，即使在 stdout 被重定向时，后者也可以用于显示警告和错误信息。

###### 字符串正则匹配

- **re模块**为高级字符串处理提供了正则表达式工具。

###### 数学

- **math模块**为浮点运算提供了对底层C函数库的访问。
- **random模块**提供了生成随机数的工具。

###### 访问 互联网

- 有几个模块用于访问互联网以及处理网络通信协议。其中最简单的两个是用于处理从 urls 接收的数据的 **urllib.request** 以及用于**发送电子邮件的 smtplib**；

###### 日期和时间

- **datetime模块**为日期和时间处理同时提供了简单和复杂的方法。

###### 数据压缩

- 以下模块直接支持通用的数据打包和压缩格式：**zlib，gzip，bz2，zipfile，以及 tarfile**。

###### 性能度量

- Python 提供了一个度量工具，**timeit模块**。

###### 测试模块

- **doctest模块**：提供了一个工具，扫描模块并根据程序中内嵌的文档字符串执行测试；
- **unittest模块：**不像 doctest模块那么容易使用，不过它可以在一个独立的文件里提供一个更全面的测试集；



