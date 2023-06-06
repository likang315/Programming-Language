### Java API

------

[TOC]

###### java.lang.Object

##### 01：Object：

- Object 类是所有 Java 中类的顶级父类
- 普通类继承了 Object 类才可以称为一个Java类
- 类中提供了最基本的**9个方法（加上重载是11个方法）**

1. ###### toString(): 输出对象时，默认自动调用，也可手动调用

   - 在打印查看一个对象的内部封装数据（属性）时，默认打印的是
   - 完整限定名@对象哈希值的十六进制表示：com.xzy.obj.Person@82764b
   - 用来默认的解析对象的内部结构，当前的这个默认解析是Object类给我们的一种实现，需重写成我们想要解析的格式

2. ###### boolean equals(Object obj) ：

   - 如若没有重写，则是继承 Objecet 类的，根据源码看，还是 ==，比较的是地址(引用数据类型)，若重写则比较的是内容，判断两个对象是否相等
   - 引用数据类型是否相等我们必须比较其封装的内容是否完全相同，重写之后先比较地址是否相同，若相同直接返回ture，若不相等，则比较内容是否相等，相等返回 ture

```JAVA
public boolean equals(Object obj) {
    return (this == obj)；
}
```

###### 四个特性：

- 自反性：对于任意不为nul的引用值x，x.equals(x)一定是true
- 对称性：对于任意不为null的引用值x和y，当且仅当x.equals(y)是true时，y.equals(x)也是true
- 传递性：对于任意不为null的引用值x，y和z，如果x.equals(y)是true，同时y.equals(z)是true，那么x.equals(z)一定是true
- 一致性：对于任意不为null的引用值x和y，如果用于 equals 比较的对象信息没有被修改的话，多次调用时x.equals(y) 要么一致地返回true，要么一致地返回 false

3. ###### hashCode( ): 哈希值

   - 对象调用该方法时，返回的是将该对象的内部地址转换成的一个整数，在一个程序中没有改变对象中封装的数据，则对象多次调用该方法时产生同一个整数（哈希码）

   - ###### hashCode() 与 equals() 机制 

   - 若两个对象 equals( )相等（重写后），先比较地址，若地址相同肯定内容相同直接返回，若地址不同，再比较内容，内容相同则为true

   - 对于eqauls方法和hashCode方法是这样规定的： 

     1. **如果两个对象的 equals( )相等，那么它们的 hashCode( ) 一定要相等**，确保通过equals(Object obj)方法判断结果为true的两个对象具备相等的hashcode()返回值；
  2. 如果两个对象的hashCode相等，它们的equals() 并不一定相等
   
4. ###### Class<?> getClass() 

   - 返回此 Object 的运行时 class 对象

6. ###### protected  void finalize() 

   - 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法；
   - JVM会为每一个重写该方法的对象创建一个**看门狗（watchdog）**。这是**Finalizer类**的一个实例。而所有的这些**看门狗又会为Finalizer类所引用**。由于存在这么一个引用链，因此整个的这些对象都是存活的；
   - **Finalizer队列**
     - 只有在**GC完成后**，JVM才会意识到除了Finalizer对象已经没有人引用到我们创建的这些实例了，因此它才会把**指向这些对象的Finalizer对象标记成可处理的**。GC内部会把这些**Finalizer对象**放到java.lang.ref.Finalizer.ReferenceQueue这个**特殊的队列**里面。
   - **Finalizer守护线程**
     - Finalizer线程是个**单一职责的守护线程**。这个线程会不停的循环等待java.lang.ref.Finalizer.ReferenceQueue中的新增对象。一旦Finalizer线程发现队列中出现了新的对象，它会弹出该对象，**调用它的finalize()方法，将该引用从Finalizer类中移除**，因此下次GC时，**这个Finalizer实例**以及**它引用的那个对象（重写finalize方法的对象）**就会被回收。
   - **导致OOM**
     - 因为**Finalizer线程和主线程相比它的优先级要低**。这意味着分配给它的CPU时间更少，因此它的处理速度没法赶上新对象创建的速度，所以会导致OOM。

7. ###### protected  native Object clone( ) 

   - 创建并返回此对象的一个副本，如果某个对象已经有一些有效值了，并且需要一个和它一样的对象，clone（）默认是浅拷贝

   - 按照惯例重写的时候一个要将protected修饰符修改为public，这是JDK所推荐的做法

   - 重写的clone() ，必须实现 Cloneable 接口。虽然这个接口并没有什么方法，但是必须实现该标志接口,否则将会在运行期间，抛出：CloneNotSupportedException 异常

   - **浅拷贝和深拷贝的区别是**：
- 在拷贝对象的一些属性时，创不创建新的属性对象，不创建直接将原对象的引用值拷贝给新的对象，则是浅拷贝，反之，创建新的对象为深拷贝
     - 实现深拷贝
       - 如果想要深拷贝一个对象，这个对象必须要实现 Cloneable接口，重写clone方法，并且在 clone 方法内部，把该对象引用的其他对象也要 clone 一份，这就要求这个被引用的对象必须也要实现Cloneable接口并且重写clone方法
       - 反序列化为深拷贝；

###### java.lang.System

##### 02：System 

- 属性：
  - static PrintStream err 
    - “标准”错误输出流
  - static InputStream in 
    - “标准”输入流 
  - static PrintStream out 
    - “标准”输出流 
- 方法：
  - static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 
    -  从指定源数组中复制一个数组，复制从指定的位置开始，到目标数组的指定位置结束
  - static void exit(int status) 
    - 终止当前正在运行的 Java 虚拟机
  - static void gc() 
    - 运行垃圾回收器

###### java.util.Scanner

##### 03：Scanner 类：

​	扫描器，从指定源开始扫描，等待键盘输入，创建对象新建扫描器，开始扫描

```java
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
        // next方式接收字符串
        System.out.println("next方式接收：");
        // 判断是否还有输入，只判断一回
        if（scan.hasNext（）） {
            String str1 = scan.next();
            System.out.println("输入的数据为：" + str1);
        }
        scan.close();
    }
    //死循环了，输入一下输出一下
    Scanner sc = new Scanner(System.in);
    while (sc.hasNext()) {
        String str = sc.nextLine();
        System.out.println(str);
    }
    sc.close();
}
```

###### next() 与 nextLine() 区别

- next()，nextInt().....都一样:
  - 一定要读取到有效字符后才可以开始输入，在未读取到有效字符时，空格，tab，Enter键均舍去
  - 只有输入有效字符后才将其后面输入的空白（三个键）作为结束符
  - next() 不能得到带有空格的字符串

- nextLine()：
  - 以Enter为结束符，可以获得空格，Tab键
  - 读取完之在它的结束符之前自动加\n，给输入的流换了个行，下一次读取时自动从新的一行开始扫描

###### java.lang.Math

##### 04：Math 

（无构造）在这个类中提供大量的关于数学运算的 static 方法	

- static double E ： 比任何其他值都更接近 e（即自然对数的底数）的 double 值
- static double PI ：比任何其他值都更接近 pi（即圆的周长与直径之比，圆周率）的 double 值
- static double abs(double a) 
  - 返回 double 值的绝对值
- static double sqrt(double a) 
  - 返回正确舍入的 double 值的正平方根
- static double cbrt(double a) 
  - 返回 double 值的立方根
- static double exp(double a) 
  -  返回 e 的 double 次幂的值
- static double pow(double a, double b) 
  - 返回第一个参数的第二个参数次幂的值
- static double max(double a, double b) 
  - 返回两个 double 值中较大的一个
- static double min(double a, double b) 
  - 返回两个 double 值中较小的一个
- static double random() 
  - 返回带正号的 double 值，该值大于等于 0.0 且小于 1.0，左闭右开
- static int round(float a) 
  - 返回最接近参数的 int
- static double signum(double d) 
  - 返回参数的符号函数；如果参数为 0，则返回 0；如果参数大于 0，则返回 1.0；如果参数小于 0，则返回 -1.0 

###### java.util.Random

##### 05：Class Random：

​	如果用相同的种子创建两个 Random 实例，则对每个实例进行相同的方法调用序列，它们将生成并返回相同的随机数，种子即构造参数

- 构造方法：
  ​			public Random(）   -------- 创建一个新的随机数生成器
  ​			public Random(long d)------- 使用单个 long 种子创建一个新的随机数生成器
- 方法：
  - int nextInt()
    - 返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值
  - int nextInt(int n) 
    - 返回一个伪随机数，它是取自此随机数生成器序列的、在 0（包括）和指定值（不包括）之间均匀分布的 int 值，左闭右开
- 注意：伪随机数如果种子数相同，那么在相同的随机次数下，产生的随机数是相同的，其有一个nextDouble()产生含0.几的随机小数，其也是Math类的Random()底层实现

###### java.util.Date

#####  07：Class Date：获取当前的瞬时时间精确到毫秒 

- 使用 Calendar 类实现日期和时间字段之间转换，使用 DateFormat 类来格式化和解析日期字符串

- 时间戳：是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数；

- 方法：	
  ​		 boolean after(Date when) 
  ​				  测试此日期是否在指定日期（when）之后 
  ​		 boolean before(Date when) 
  ​				  测试此日期是否在指定日期(when)之前
  ​		 int compareTo(Date anotherDate)
  ​				  比较两个日期的顺序
  ​		 boolean equals(Object obj) 
  ​    				  比较两个日期的相等性
  
- toString
  
  - ```java
    public String toString() {
       // "EEE MMM dd HH:mm:ss zzz yyyy";
       // Mon Oct 11 20:57:49 CST 2021
    }
    ```
  
- 默认：时间戳，Jackson 默认序列化成时间戳，可以设置响应格式；

###### java.text.DateFormat:  日期/时间格式化子类的抽象类

#####  Calss DateFormat 

- static DateFormat getDateInstance() ：获取日期格式器，该格式器具有默认语言环境的默认格式化风格 
- static DateFormat getTimeInstance() ：获取时间格式器，该格式器具有默认语言环境的默认格式化风格 

###### java.text.SimpleDateFormat

##### Class SimpleDateFormat：与语言环境有关的方式来格式化和解析日期的具体类，线程不安全的类

- 构造方法：
  - SimpleDateFormat ()：用默认的模式和默认语言环境的日期格式符号构造 SimpleDateFormat
  - SimpleDateFormat (String pattern)：用给定的模式和默认语言环境的日期格式符号构造 
    - new SimpleDateFormat ("yyyy/MM/dd hh:mm:ss");
- 方法：
  - void applyPattern(String pattern) ： 将给定模式字符串应用于此日期格式 
  - String  format(String.valueOf(date) ：将一个 Date 格式化为日期/时间字符串

###### java.util.Calendar

#####  08：Class Calendar 类：

```java
public abstract class Calendar implements Serializable, Cloneable, Comparable<Calendar> {
    // Calendar's Year: 2012
    System.out.println("Calendar's Year: " + cal.get(Calendar.YEAR));
    // 获取周几，返回数值从星期天开始
    System.out.println(calendar.get(Calendar.DAY_OF_WEEK));
    // 这月的第几天
    System.out.println(calendar.get(Calendar.DATE));
    // 哪一年
    System.out.println(calendar.get(Calendar.YEAR));
    // 获取时刻
    System.out.println(calendar.get(Calendar.HOUR_OF_DAY));

    // 获取星期对应数值
    public static int getWeekNumber(Date date) {
        Calendar c = Calendar.getInstance();
        c.setTime(date);
        /*星期日:Calendar.SUNDAY=1
         *星期一:Calendar.MONDAY=2
         *星期二:Calendar.TUESDAY=3
         *星期三:Calendar.WEDNESDAY=4
         *星期四:Calendar.THURSDAY=5
         *星期五:Calendar.FRIDAY=6
         *星期六:Calendar.SATURDAY=7 */
        int number = c.get(Calendar.DAY_OF_WEEK) - 1;
        if (number == 0) {
            number = 7;
        }
        return number;
    }
}

// 获取后续第n天日期
public static Date getNextNumberDay(Date date, int n) {
    Calendar cal = Calendar.getInstance();
    cal.setTime(date);
    // 时间域，可以修改为年、月、日、时
    cal.add(Calendar.DATE, n);
    return cal.getTime();
}
```

- 抽象类不能实例化，提供了一个类方法 getInstance（），以获得此类型的实例；
  - 根据TimeZone时区类型和Locale地区类型，创建Calendar；
  - 如果没有某个对应的参数，那么程序会自动获取机器的时区或者地区；
- Date getTime()
  - 获取当前日历的时间；
- void setTime (Date date) 
  - 使用给定的 Date 设置此 Calendar 的时间；
- get(int field)
  - 获取时间域的值 ，field为Calendar.YEAR等时间域；
- Date add(int field, int amount)；
  - 获取当前时间域的后续指定的amount的时间域； 
- void setFirstDayOfWeek(int value)
  - 设置一周的第一天
- getFirstDayOfWeek()
  - 获取一周的第一天

##### 09：MessageFormat：用来格式化一个消息，通常是一个字符串

```java
public class MessageFormat extends Format {
  public static String format(String pattern, Object ... arguments) {
        MessageFormat temp = new MessageFormat(pattern);
        return temp.format(arguments);
  }
}
// 格式化字符串，常用于打印日志
String s2 = MessageFormat.format("oh, {0} is 'a' pig {1}", "lisi","!");
System.out.println(s2);
```
