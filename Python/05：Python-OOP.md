### Python3 面向对象

------

[TOC]

##### 01：概述

- Python中的类提供了面向对象编程的所有基本功能：类的继承机制允许多个基类，派生类可以覆盖基类中的任何方法，方法中可以调用基类中的同名方法。

##### 02：类定义

- 类实例化后，可以使用其属性，实际上，创建一个类之后，可以通过类名访问其属性。

- 语法：

- ```python
  class ClassName:
      <statement-1>
      .
      .
      .
      <statement-N>
  ```

##### 03：类属性与方法

###### 类的私有属性

- **__private_attrs**：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问

###### 类的方法

- 在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 **self**，且为第一个参数，**self** 代表的是类的实例。
- **self** 的名字并不是规定死的，也可以使用 **this**，但是最好还是按照约定使用 **self**

###### 类的实例方法

- **__private_method**：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用。

###### 类的专有方法

1. **__init__ :** 构造函数，在生成对象时调用
2. **__del__ :** 析构函数，释放对象时使用
3. **__repr__ :** 打印，转换
4. **__setitem__ :** 按照索引赋值
5. **__getitem__:** 按照索引获取值
6. **__len__:** 获得长度
7. **__cmp__:** 比较运算
8. **__call__:** 函数调用
9. **__add__:** 加运算
10. **__sub__:** 减运算
11. **__mul__:** 乘运算
12. **__truediv__:** 除运算
13. **__mod__:** 求余运算
14. **__pow__:** 乘方

###### 运算符重载

- Python同样支持运算符重载，我们可以对类的专有方法进行重载。

```python
#!/usr/bin/python3
 
class Site:
    def __init__(self, name, url):
        self.name = name       # public
        self.__url = url   # private
 
    def who(self):
        print('name  : ', self.name)
        print('url : ', self.__url)
 
    def __foo(self):          # 私有方法
        print('这是私有方法')
 
    def foo(self):            # 公共方法
        print('这是公共方法')
        self.__foo()
 
x = Site('菜鸟教程', 'www.runoob.com')
x.who()        # 正常输出
x.foo()        # 正常输出
x.__foo()      # 报错
```

##### 04：类对象

- 类对象支持两种操作：属性引用和实例化。

- 属性引用使用和 Python 中所有的属性引用一样的标准语法：**obj.name**。

- 类有一个名为 __init__() 的特殊方法（**构造方法**），**该方法在类实例化时会自动调用**， _ _ init _ _() 方法可以有参数，参数通过 _ init_() 传递到类的实例化操作上。

- ```
  def __init__(self):
      self.data = []
  ```

###### self代表类的实例，而非类

- 类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的**第一个参数名称**, 按照惯例它的名称是 self。

- ```python
  #!/usr/bin/python3
   
  # 类定义
  class people:
      # 定义基本属性
      name = ''
      age = 0
      # 定义私有属性,私有属性在类外部无法直接进行访问
      __weight = 0
      # 定义构造方法
      def __init__(self,n,a,w):
          self.name = n
          self.age = a
          self.__weight = w
      def speak(self):
          print("%s 说: 我 %d 岁。" %(self.name,self.age))
   
  # 实例化类
  p = people('runoob',10,30)
  p.speak()
  ```

##### 05：继承

- 语法：BaseClassName（实例中的基类名）必须与派生类定义在一个作用域内

- ```python
  class DerivedClassName(BaseClassName):
      <statement-1>
      .
      .
      .
      <statement-N>
      
  class DerivedClassName(modname.BaseClassName):
  ```

###### 多继承

- 语法：

- 注意：圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

- ```python
  class DerivedClassName(Base1, Base2, Base3):
      <statement-1>
      .
      .
      .
      <statement-N>
  ```

###### 重写

- ```python
  #!/usr/bin/python3
   
  class Parent:        # 定义父类
     def myMethod(self):
        print ('调用父类方法')
   
  class Child(Parent): # 定义子类
     def myMethod(self):
        print ('调用子类方法')
   
  c = Child()          # 子类实例
  c.myMethod()         # 子类调用重写方法
  super(Child,c).myMethod() #用子类对象调用父类已被覆盖的方法
  ```

