### 数值计算API

------

[TOC]

##### 01：概述

- 计算机是二进制的，浮点数没有办法是用**二进制进行精确表示。CPU表示浮点数由两个部分组成：指数和尾数，这样的表示方法一般都会失去一定的精确度**，有些浮点数运算也会产生一定的误差。
- 计算时精度丢失，会导致计算结果不一致，所以涉及到数值计算时，需要使用特殊类处理；
- double 0.1 ：很好玩~

##### 02：BigDecimal

- BigDecimal 由任意精度的**非标度值 和 32 位的整数标度 (scale) 组成**;
- 如果为零或正数，则标度是小数点后的位数
- 如果为负数，则将该数的非标度值乘以 10 的负 scale 次幂，因此，BigDecimal 表示的数值是 (unscaledValue × 10-scale)
- 舍入模式：设置舍入模式，确定精度；
- BigDecimal都是**不可变的（immutable）的**，在进行每一步运算时，都会产生一个新的对象，所以在**做加减乘除运算时千万要保存操作后的值**；

###### 构造方法

- BigDecimal(double val)
  - 参数类型为double的构造方法的结果有一定的不可预知性，不建议使用；
- BigDecimal(String val)
  - 建议使用String的；
  - **当double必须用作BigDecimal的源时，**使用BigDecimal的静态方法valueOf；
- public static BigDecimal valueOf(long val) ；

###### 方法

- public BigDecimal add(BigDecimal augend)
  - 加
- public BigDecimal subtract(BigDecimal subtrahend)
  - 减
- public BigDecimal multiply(BigDecimal multiplicand)
  - 乘
- public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)
  - 除，需要指定保留结果的位数，和舍入模式；

###### 舍入模式

- ROUND_UP ：向远离零的方向舍入。舍弃非零部分，并将非零舍弃部分相邻的一位数字加一。
- ROUND_DOWN ：向接近零的方向舍入。舍弃非零部分，同时不会非零舍弃部分相邻的一位数字加一，采取截取行为。
- **ROUND_HALF_UP** ：向“最接近”的数字舍入，如果与两个相邻数字的距离相等，则为向上舍入的舍入模式，“四舍五入”。
- **ROUND_HALF_DOWN** ：向“最接近”的数字舍入，如果与两个相邻数字的距离相等，则为向下舍入的舍入模式，“五舍六入”。

###### 比大小

- public i**nt compareTo(BigDecimal val)**
  - 大于返回1， 小于返回 -1， 等于返回0；
  - 会比较整数位，和小数位；

##### 03：Class BigInteger：大整数

###### 构造方法：

- BigInteger(int numBits, Random rnd) 
  - 构造一个随机生成的 BigInteger，它是在 0 到 (2^numBits - 1)（包括）范围内均匀分布的
- BigInteger(String val) 
  - 将 BigInteger 的十进制字符串表示形式转换为 BigInteger

###### 方法：

- BigInteger add(BigInteger val) 
  - 加
- BigInteger subtract(BigInteger val) 
  - 减
- BigInteger multiply(BigInteger val) 
  - 乘
- BigInteger divide(BigInteger val) 
  - 除
- int compareTo(BigInteger val) 
  - 比较