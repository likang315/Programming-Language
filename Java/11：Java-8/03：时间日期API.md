### Java 8 时间日期API

------

[TOC]

##### 01：Java 8 之前时间、日期类的API存在很多缺陷

1. Java 的`java.util.Date`和`java.util.Calendar`类易用性差，不支持时区，而且他们都不是线程安全的；
2. SimpleDateFormat 是非线程安全的，当有多个线程调用是会产生意想不到的结果；
3. 对日期的计算方式繁琐，而且容易出错，因为月份是从0开始的，从`Calendar`中获取的月份需要加一才能表示当前月份；
4. 都在java.time下；

##### 02：Instant

- Instant 和 Date 一样，表示一个时间戳，用于描述一个时刻，只不过它较 Date 而言，可以描述更加精确的时刻。并且 Instant 是时区无关的，它表示的是**格林尼治时间**（世界标准时间）；
- Date 最多可以表示毫秒级别的时刻，而 Instant 可以表示纳秒级别的时刻。

###### 方法：

- public static Instant now()；
  - 根据系统当前时间创建一个 Instant 实例，表示当前时刻
- public static Instant ofEpochMilli(long epochMilli)
  - 通过传入一个标准时间的偏移值毫秒数直接构建一个 Instant 实例

```java
// 创建 Instant 实例
Instant instant = Instant.now();
System.out.println(instant);
Instant instant2 = Instant.ofEpochMilli(1597134216043L);
System.out.println(instant2);
```

##### 03：LocalDate 专注于处理日期相关信息

- LocalDate 依然是一个不可变类，它关注时间中年月日部分；
  - 例：2020-01-01

###### 方法：【见名知意】

- public static LocalDate now()
  - 当前系统时间的年月日信息；
- LocalDate parse(CharSequence text)
  - 将"yyyy-MM-dd" 这种个格式的字符串转成LocalDate；
- LocalDate parse(CharSequence text, DateTimeFormatter formatter)
  - 将formatter 转成的字符串在转回来；
- boolean isAfter(ChronoLocalDate other)
  - 判断this date 是否在other之后；
-  boolean isBefore(ChronoLocalDate other) 
  - 判断 this date 是否在other之前；
- boolean isEqual(ChronoLocalDate other)
  - 判断两个date是否相等；
- long toEpochDay()
  - now 到 1970 年的天数；
- public static LocalDate of(int year, int month, int dayOfMonth)
  - 显式指定年月日信息；
- public int getYear()
  - 获取年份值；
- public int getMonthValue()
  - 获取月份的值；
- public int getDayOfMonth()
  - 获取当前日是这个月的第几天；
- public int getDayOfYear()
  - 获取当前日是这一年的第几天；
- public DayOfWeek getDayOfWeek()
  - 返回星期几，SUNDAY；

```java
LocalDate date = LocalDate.now();
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMdd");
String text = date.format(formatter);
LocalDate parsedDate = LocalDate.parse(text, formatter);
```

##### 04：LocalTime 专注于处理时间的

- 专注于时间的处理，它提供小时，分钟，秒，纳秒（10的九次方）的各种处理，**本地时间**。

###### 方法

- public static LocalTime now()
  - 根据系统当前时刻获取其中的时间部分内容
  - 17:35:48.628  【时：分：秒：毫秒】
- public static LocalTime of(int hour, int minute)
  - 显式传入小时和分钟来构建一个实例对象
- public static LocalTime of(int hour, int minute, int second)
  - 通过传入时分秒构造实例
- public int getHour()
- public int getMinute()
- public int getSecond()
- public int getNano()
  - 获取纳秒
- public LocalTime withHour(int hour)
  - 修改当前 LocalTime 实例中的 hour 属性并重新返回一个新的实例

```java
LocalTime localTime = LocalTime.now();
System.out.println(localTime);
System.out.println(localTime.getMinute());
System.out.println(localTime.getSecond());
System.out.println(localTime.getNano() / 1000000 );
```

##### 05：LocalDateTime 

- 是`LocalDate`和`LocalTime`的结合体，与时区无关的，两种创建方式。
- 2018-03-28T16:47:48  ：T 表示分隔符

###### 方法

- public static LocalDateTime now()
- public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second) 
- public LocalDate toLocalDate()
- public LocalTime toLocalTime()

```java
LocalDateTime local1 = LocalDateTime.of(2017, Month.JANUARY, 4, 17, 23, 52);

LocalDate localDate = LocalDate.of(2017, Month.JANUARY, 4);
LocalTime localTime = LocalTime.of(17, 23, 52);
LocalDateTime local2 = localDate.atTime(localTime);
// 时间戳转LocalDateTime
Instant.ofEpochMilli(Long.parseLong(1656906267000L)).atZone(ZoneOffset.ofHours(+8)).toLocalDateTime()
// Instant.now()
```

###### 序列化

- ```java
  // 自己的序列化方式
  @Override
  public String toString() {
      return date.toString() + 'T' + time.toString();
  }
  // 使用Jackson工具，会序列化成数组
  {
      "nano": 0,
      "year": 2021,
      "monthValue": 7,
      "dayOfMonth": 21,
      "hour": 16,
      "minute": 33,
      "second": 59,
      "month": "JULY",
      "dayOfWeek": "WEDNESDAY",
      "dayOfYear": 202,
      "chronology": {
          "id": "ISO",
          "calendarType": "iso8601"
      }
  }
  ```

- 解决方式：ObjectMapper 配置JavaTimeModule；

##### 06：ZonedDateTime

-  可以被理解为 LocalDateTime 的外层封装，它的内部存储了一个 LocalDateTime 的实例，还定义了 ZoneId 和 ZoneOffset 来描述时区；
- **日期+时区** 就已经唯一确定了某个时刻，就相当于我在存储某个时刻的时候，我说明了这是某某时区的某某时间，即便你换了一个地区，你也不至于把这个时间按自己当前的时区进行解析并直接使用了吧。

###### 方法

- public static ZonedDateTime now()
  - 系统将以默认时区（取当地时区）计算并存储日期时间信息
- public static ZonedDateTime now(ZoneId zone)
  - 指定时区
- public static ZonedDateTime of(LocalDate date, LocalTime time, ZoneId zone)
  - 指定日期时间和时区

```java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
System.out.println(zonedDateTime);

LocalDateTime localDateTime = LocalDateTime.now();
// 关联了时区的日期时间就很能够解决，换时区导致程序中时间错乱的问题
ZoneId zoneId = ZoneId.of("America/Los_Angeles");
ZonedDateTime zonedDateTime1 = ZonedDateTime.of(localDateTime, zoneId);
System.out.println(zonedDateTime1);

Instant instant = Instant.now();
ZoneId zoneId1 = ZoneId.of("GMT");
ZonedDateTime zonedDateTime2 = ZonedDateTime.ofInstant(instant, zoneId1);
System.out.println(zonedDateTime2);
```

##### 07：格式化日期时间

- **DateTimeFormatter**
- 格式化主要有两种情况
  - 将日期时间格式化成字符串
  - 将格式化的字符串装换成日期时间对象

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(formatter.format(localDateTime));
// str 的格式要符合formatter的格式
String str = "2020-08-11 19:45:16";
DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime localDateTime2 = LocalDateTime.parse(str, formatter2);
System.out.println(localDateTime2);
```

##### 08：时间差

- java.time 提供了两个日期时间之间的差值的计算方法
- **Period**：处理两个日期之间的差值
- **Duration**：处理两个时间之间的差值

```java
LocalDate date = LocalDate.of(2020,7,11);
// 2020.8.12, 可能为负数
LocalDate date1 = LocalDate.now();
// between(LocalDate startDateInclusive, LocalDate endDateExclusive)
Period period = Period.between(date, date1);
System.out.println(
  period.getYears() + "年" + period.getMonths() + "月" + period.getDays() + "天");

LocalDateTime from = LocalDateTime.of(2017, Month.JANUARY, 5, 10, 7, 0);
LocalDateTime to = LocalDateTime.of(2017, Month.FEBRUARY, 5, 10, 7, 0);    
// 表示从 2017-01-05 10:07:00 到 2017-02-05 10:07:00 这段时间
Duration duration = Duration.between(from, to);     

long days = duration.toDays();              // 这段时间的总天数
long hours = duration.toHours();            // 这段时间的小时数
long minutes = duration.toMinutes();        // 这段时间的分钟数
long seconds = duration.getSeconds();       // 这段时间的秒数
long milliSeconds = duration.toMillis();    // 这段时间的毫秒数
long nanoSeconds = duration.toNanos();      // 这段时间的纳秒数
```

##### 09：时区

- **ZoneId** 对象可以通过`ZoneId.of()`方法创建，也可以通过`ZoneId.systemDefault()`获取系统默认时区;
- ZonedDateTime 对象由两部分构成，LocalDateTime`和`ZoneId，其中2017-01-05T15:26:56.147`部分为`LocalDateTime，+08:00[Asia/Shanghai]部分为ZoneId；

```java
ZoneId shanghaiZoneId = ZoneId.of("Asia/Shanghai");
ZoneId systemZoneId = ZoneId.systemDefault();
```



