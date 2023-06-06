### Formatter

------

[TOC]

- 格式化字符串，按照指定格式打印；

##### 01：使用方式

```java
// 构造Formatter对象
Formatter formatter = new Formatter();
// 格式化操作
formatter.format("The result number is %s.", "lisi").toString;
```

##### 02：方法

```java
// 包含格式化说明符的字符串，指定了整体目标格式，通过格式化说明符进行占位并指定相应位置的内容格式；
public Formatter format(String format, Object ... args) {
  	return format(l, format, args);
}
```

##### 03：格式化说明符

| 符号 | 类型               | 示例                                  | 效果         |
| ---- | ------------------ | ------------------------------------- | ------------ |
| d    | 整数型（十进制）   | **formatter.format(“%d”, 1000);**     | 1000         |
| o    | 整数型（八进制）   | formatter.format(“%o”, 1000);         | 1750         |
| x    | 整数型（十六进制） | formatter.format(“%x”, 1000);         | 3e8          |
| f    | 浮点型（十进制）   | **formatter.format(“%f”, 1000.0);**   | 1000.000000  |
| e    | 浮点型（科学计数） | formatter.format(“%e”, 1000.0);       | 1.000000e+03 |
| b    | 布尔型             | formatter.format(“%b”, true);         | true         |
| c    | 字符型             | formatter.format(“%c”, ‘A’);          | A            |
| s    | 字符串型           | **formatter.format(“%s”, “String”);** | String       |
| %    | 字符“%”            | formatter.format(“%d%%”, 100);        | 100%         |