### 正则表达式、URL

------

[TOC]

##### 01：正则表达式 API (Regular Expression)

- 概念：一种描述字符串模式的语言；
- 正则表达式定义了**字符串的模式**，可以用来搜索、编辑或处理文本

###### String 中正则表达式的应用

- boolean matches(String regex) 
  - 判断此字符串是否匹配给定的正则表达式
- String[] split(String regex) 
  - 根据给定正则表达式的匹配拆分此字符串 
- String replaceAll(String regex, String replacement) 
  - 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串 

###### java.util.regex

##### 02：Pattern 

- 正则表达式的编译表示形式，先编译，然后得到匹配器，再匹配；


```java
public final class Pattern
```

- 字符串的正则表达式必须**首先被编译为此类的实例，然后可将得到的模式用于创建 Matcher（匹配器**），依照正则表达式，该对象可以与任意字符序列匹配
- static Pattern compile(String regex) 
  - 将给定的正则表达式编译到模式中
- static Matcher matcher(CharSequence input) 
  - 创建匹配编译后模式的匹配器，但并不比较是否匹配

###### java.util.regex.Matcher

##### 03：Matcher：

- 通过解释 Pattern 对 characterSequence 执行匹配操作的引擎，调用Pattern.matcher( ) 方法从模式创建匹配器
- Pattern pattern() 
  - 返回由此匹配器解释的模式

- boolean find() 
  - 尝试查找与该模式匹配的输入序列的下一个子序列

- ```java
  public class Main {
      String pattern = "(\\w+)(\\d+)(.*?)";
      // 提前编译为Pattern的实例，创建 Pattern 对象
      Pattern r = Pattern.compile(pattern);
  
      public static void main( String[] args ){
          // 按指定模式在字符串查找
          String line = "This order was placed for QT3000! OK?";
          // 现在创建 matcher 对象
          Matcher m = r.matcher(line);
          if (m.find( )) {
              System.out.println("Found value: " + m.group(0) );
              System.out.println("Found value: " + m.group(1) );
              System.out.println("Found value: " + m.group(2) );
              System.out.println("Found value: " + m.group(3) ); 
          } else {
              System.out.println("NO MATCH");
          }
      }
  ```

###### Java.net.URL

##### 04：URL

​	类 URL 代表一个统一资源定位符，它是指向互联网中“唯一的资源”

###### 构造方法：

- URL(String spec) 
  - 根据 String 表示形式创建 URL 对象

###### 方法：

- URLConnection openConnection() 
  - 用于建立一个到 URL 所引用的资源的通信链接，URLConnection 对象，会发送一次GET请求；

###### java.net.URLConnection

##### 05：URLConnection  

- 应用程序和 URL 之间的通信链接，此类的实例可用于读取和写入此 URL 引用的资源
- InputStream getInputStream() 
  - 返回从此打开的连接读取的输入流
    	

