### Python 函数

------

[TOC]

##### 01：自定义一个函数

1. 函数代码块以 **def** 关键词开头，后接函数标识符名称和圆括号 **()**。
2. 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。
3. 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
4. 函数内容以冒号 **:** 起始，并且缩进。
5. **return [表达式]** 结束函数，选择性地返回一个值给调用方，不带表达式的 return 相当于返回 None。

###### 语法

- ```python
  def 函数名（参数列表）:
      函数体
      
  #!/usr/bin/python3
  
  def hello() :
      # 打印hello world
      print("Hello World!")
  
  hello()
  ```

##### 02：函数调用

- 可以通过另一个函数调用执行，也可以直接从 Python 命令提示符执行；

##### 03：参数传递

- 在 python 中，**类型属于对象，变量是没有类型的**
- 在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。
  - **不可变类型：**变量赋值 **a=5** 后再赋值 **a=10**，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变 a 的值，相当于新生成了 a。
  - **可变类型：**变量赋值 **la=[1,2,3,4]** 后再赋值 **la[2]=5** 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了

##### 04：参数

###### 必传参数

- 必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

###### 关键字参数

- 函数调用使用关键字参数来确定传入的参数值

- 使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。

- ```python
  #!/usr/bin/python3
   
  #可写函数说明
  def printinfo( name, age ):
     "打印任何传入的字符串"
     print ("名字: ", name)
     print ("年龄: ", age)
     return
   
  #调用printinfo函数
  printinfo( age=50, name="runoob" )
  ```

###### 默认函数

- 调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 age 参数，则使用默认值

- ```python
  #!/usr/bin/python3
   
  #可写函数说明
  def printinfo( name, age = 35 ):
     "打印任何传入的字符串"
     print ("名字: ", name)
     print ("年龄: ", age)
     return
     
  printinfo( name="runoob" )
  ```

###### 不定长参数

- 如果在函数调用时没有指定参数，它就是一个**空元组**。我们也可以不向函数传递未命名的变量。

- 加了两个星号 ***\*** 的参数会以字典的形式导入；

- 

  ```
  def functionname([formal_args,] *var_args_tuple ):
     "函数_文档字符串"
     function_suite
     return [expression]
  ```

- ```python
  #!/usr/bin/python3
   
  # 可写函数说明
  def printinfo( arg1, *vartuple ):
     "打印任何传入的参数"
     print ("输出: ")
     print (arg1)
     for var in vartuple:
        print (var)
     return
   
  # 调用printinfo 函数
  printinfo( 10 )
  printinfo( 70, 60, 50 )
  ```

###### 声明函数时，参数中星号 ***** 可以单独出现

- 如果单独出现星号 ***** 后的参数必须用关键字传入。

- ```
  def f(a,b,*,c):
      return a+b+c
  ```

###### 匿名函数

- Python 使用 **lambda** 来创建匿名函数。所谓匿名，意即不再使用 **def** 语句这样标准的形式定义一个函数。

  - **lambda** 只是一个表达式，函数体比 **def** 简单很多。
  - lambda 的主体是一个表达式，而不是一个代码块。仅仅能在 lambda 表达式中封装有限的逻辑进去。
  - lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
  - 虽然 lambda 函数看起来只能写一行，却不等同于 C 或 C++ 的内联函数，后者的目的是**调用小函数时不占用栈内存从而增加运行效率**。

- 语法

  - ```python
    lambda [arg1 [,arg2,.....argn]]:expression
    
    #!/usr/bin/python3
     
    # 可写函数说明
    sum = lambda arg1, arg2: arg1 + arg2
     
    # 调用sum函数
    print ("相加后的值为 : ", sum( 10, 20 ))
    print ("相加后的值为 : ", sum( 20, 20 ))
    ```

##### 05：return语句

- 用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。

- ```python
  #!/usr/bin/python3
   
  # 可写函数说明
  def sum( arg1, arg2 ):
     # 返回2个参数的和."
     total = arg1 + arg2
     print ("函数内 : ", total)
     return total
   
  # 调用sum函数
  total = sum( 10, 20 )
  print ("函数外 : ", total)
  ```

##### 06：强制位置参数

-  / ：用来指明函数形参必须使用指定位置参数，不能使用关键字参数的形式

- ```python
  def f(a, b, /, c, d, *, e, f):
      print(a, b, c, d, e, f)
      
  f(10, 20, 30, d=40, e=50, f=60)
  ```





