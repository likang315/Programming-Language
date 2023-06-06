### Logback

------

[TOC]

- private static final Logger log = LoggerFactory.getLogger(Main.class);

##### 01：Slf4j：定义了抽象层的日志框架，LogBack：slf4j的原生实现

1. 配置方法：

   1. 尝试在 classpath下查找文件logback.xml;
   2. 如果文件不存在，则查找文件logback-spring.xml；
   3. 如果两个文件都不存在，logback用BasicConfigurator自动对自己进行配置，这会导致记录输出到控制台。

2. 使用方式

   1. 添加jar包：slf4j-api.jar，logback-core.jar，logback-classic.jar

      ```xml
      <dependencies>
          <dependency>
              <groupId>org.slf4j</groupId>
              <artifactId>slf4j-api</artifactId>
              <version>1.7.5</version>
          </dependency>
          <dependency>
              <groupId>ch.qos.logback</groupId>
              <artifactId>logback-core</artifactId>
              <version>${logback.version}</version>
              <scope>runtime</scope>
          </dependency>
          <dependency>
              <groupId>ch.qos.logback</groupId>
              <artifactId>logback-classic</artifactId>
              <version>${logback.version}</version>
              <scope>runtime</scope>
          </dependency>
      </dependencies>
      ```

   2. 配置logback.xml

##### 02：日志级别

- 日志只能打印它以及比它大的级别；

| level （列：由大到小 ） | FATAL(致命) | ERROR（错误） | WARN（警告） | INFO（信息） | DEBUG（调试） | TRACE（跟踪） | ALL   |      |      |
| ----------------------- | ----------- | ------------- | ------------ | ------------ | ------------- | ------------- | ----- | ---- | ---- |
| OFF                     | N           | N             | N            | N            | N             | N             | N     |      |      |
| FATAL                   | **Y**       | N             | N            | N            | N             | N             | N     |      |      |
| ERROR                   | **Y**       | **Y**         | N            | N            | N             | N             | N     |      |      |
| WARN                    | **Y**       | **Y**         | **Y**        | N            | N             | N             | N     |      |      |
| INFO（默认）            | **Y**       | **Y**         | **Y**        | **Y**        | N             | N             | N     |      |      |
| DEBUG                   | **Y**       | **Y**         | **Y**        | **Y**        | **Y**         | N             | N     |      |      |
| TRACE                   | **Y**       | **Y**         | **Y**        | **Y**        | **Y**         | **Y**         | N     |      |      |
| ALL                     | **Y**       | **Y**         | **Y**        | **Y**        | **Y**         | **Y**         | **Y** |      |      |

##### 03：Logback 配置详解

###### 根节点 `<configuration> `

- scan: 
  - 当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
- scanPeriod:
  - 设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。当scan为true时，此属性生效。默认的时间间隔为1分钟。
- debug:
  - 当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。

###### 设置上下文信息  `<contextName>`

- 每个logger都关联到logger上下文，默认上下文名称为“default”。但可以使用 `<contextName>` 设置成其他名字，用于区分不同应用程序的记录。一旦设置，不能修改。
- `<property>` 
  - 用来定义变量值的标签，有两个属性，其中name的值是变量的名称，value的值时变量定义的值。
  - **通过`<property>`定义的值会被插入到logger上下文中。定义变量后，可以使 ${} 来使用变量**。
- `<timestamp>`
  - 有两个属性，其中key，标识此`<timestamp>` 的名字；datePattern：设置将当前时间（解析配置文件的时间）转换为字符串的模式，遵循 java.txt.SimpleDateFormat 的格式；

###### 设置logger，root

- logger

  - 用来**设置某一个包或者具体的某一个类的日志打印级别、以及指定 `<appender>`。**
  - name ：用来**指定受此logger约束的某一个包或者具体的某一个类**。
  - level：用来设置打印级别，大小写无关，还有一个特俗值INHERITED或者同义词NULL，代表强制执行上级的级别。**如果未设置此属性，那么当前logger将会继承上级（root）的级别。**
  - **addtivity**：是否向上级传递 该logger 打印的信息。默认是true。

- root

  - 也是 `<logger>`元素，不过它是根 logger。只有一个level属性，因为已经被命名为"root"
  - `<root>` 可以包含零个或多个 `<appender-ref>`元素，**标识这个appender将会添加到这个logger。**

- ###### 示例

- ```xml
  <configuration>   
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">   
      <encoder>   
        <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>   
      </encoder>   
    </appender>   
    <root level="DEBUG">     
      <appender-ref ref="STDOUT" />   
    </root>
  </configuration>
  ```

###### appender 组件

- 用来定义日志输出格式，日志如何过滤以及日志文件的处理；
- name：指定appender名称；
- class：指定要实例化的appender类的完全限定名称；
  - **ConsoleAppender**：日志输出到控制台，类名 ch.qos.logback.core.ConsoleAppender。
  - **FileAppender**：日志输入到文件，类名ch.qos.logback.core.FileAppender。
  - **RollingFileAppender**：滚动记录文件，FileAppender的子类，**当符合条件（大小、时间），日志进行切分处理**。类名：ch.qos.logback.core.rolling.RollingFileAppender。
    - encoder：用来指定日志的输出格式及编码；
    - file：元素用来配置日志的路径和名称；
    - **filter**：比如用ThresholdFilter（临界值过滤器）来**过滤低于指定级别的日志**，可以控制不同级别的日志打印到不同的文件中；
    - **rollingPolicy**：用于配置滚动策略，支持**TimeBasedRollingPolicy**、SizeAndTimeBasedRollingPolicy、FixedWindowRollingPolicy、SizeBasedTriggeringPolicy；
      - fileNamePattern：指定最终日志的路径及名称；
      - maxHistory：指定日志最多保留天数，时间超过该值会被自动删除；

##### 04：异步输出日志

- 日志输出到文件是同步输出的，即每次输出都会直接写IO到磁盘文件。logback提供了日志异步输出的AsyncAppender。处理方式也很简单，添加一个基于异步写日志的appender，并指向原配置的appender即可；

- ```xml
  <!-- 异步输出 -->
  <appender name="ASYNCDEBUG" class="ch.qos.logback.classic.AsyncAppender">
      <!-- 默认如果队列的80%已满，则会丢弃TRACT、DEBUG、INFO级别的日志，若要保留全部日志，设置为0 -->
      <discardingThreshold>0</discardingThreshold>
      <!-- 缓冲队列的大小，默认值为256 -->
      <queueSize>256</queueSize>
      <!-- 关联需要异步的appender，最多只能添加一个 -->
      <appender-ref ref="needAsyncAppender"/>
      <includeCallerData>true</includeCallerData>
  </appender>
  <!-- 异步输出关联到root -->
  <root level="DEBUG">
      <appender-ref ref="STDOUT"/>
      <appender-ref ref="ASYNCDEBUG" />
  </root>
  ```

##### 05：使用AOP统一处理Web请求日志

1. 添加AOP依赖

2. 编写切面类

   ```java
   @Aspect
   @Component
   public class WebLogAspect {
       private Logger logger = Logger.getLogger(getClass());
       // 申明切点
       @Pointcut("execution(com.xupt.controller..*.*(..))")
       public void webLog() {
       }
   
       @Before("webLog()")
       public void doBefore(JoinPoint joinPoint) throws Throwable {
           // 接收到请求，记录请求内容
           ServletRequestAttributes attributes = 
               (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
           HttpServletRequest request = attributes.getRequest();
           // 记录下请求内容
           logger.info("---------------request----------------");
           logger.info("URL : " + request.getRequestURL().toString());
           logger.info("IP : " + request.getRemoteAddr());
       }
   
       @AfterReturning(returning = "resp", pointcut = "webLog()")
       public void doAfterReturning(Object ret) throws Throwable {
           logger.info("---------------response----------------");
           // 处理完请求，返回内容
           logger.info("RESPONSE : " + resp);
       }
   }
   ```

##### 06：logback.xml 文件

- 配置：一个**INFO及以上级别的日志输出**，可追踪相应的生产日志；一个**单独ERROR级别的日志输出**，方便快速检查出异常日志。
- 多个logger 的顺序由上到下依次加载；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- slf4j日志配置文件 -->
<configuration debug="true" scan="true" scanPeriod="30 seconds">
    <!-- 设置日志输出根目录 -->
    <property name="app.name" value="zeus"/>
    <property name="log.dir" value="${catalina.base}/logs"/>
    <property name="encoding" value="UTF-8"/>
    <!--打印日志的格式-->
    <property name="pattern"
              value="%d{yyyy-MM-dd HH:mm:ss.SSS}[%level][%thread] |  %C#%M:%L  |  %msg%n"/>
    <!--归档时间最小单位，保留时间-->
    <property name="log.maxHistory" value="7"/>
    <property name="log.maxSize" value="40GB" />

    <!--console-->
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${pattern}</pattern>
            <charset>${encoding}</charset>
        </encoder>
    </appender>
    <!-- 只打印 INFO 级别以上的信息-->
    <appender name="info_appender" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<!--高于此级别的日志才打印-->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
        <file>${log.dir}/info.log</file>
        <encoder>
            <pattern>${pattern}</pattern>
            <charset>${encoding}</charset>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <param name="fileNamePattern" value="${log.dir}/info-%d{yyyy-MM-dd_HH}.log"/>
            <param name="maxHistory" value="${log.maxHistory}"/>
            <param name="totalSizeCap" value="${log.maxSize}"/>
        </rollingPolicy>
    </appender>
    <!-- 只打印 ERROR 级别以上的信息 -->
    <appender name="error_appender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <File>${log.dir}/error.log</File>
        <encoder>
            <pattern>${pattern}</pattern>
            <charset>${encoding}</charset>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <param name="fileNamePattern" value="${log.dir}/error-%d{yyyy-MM-dd_HH}.log"/>
            <param name="maxHistory" value="${log.maxHistory}"/>
            <param name="totalSizeCap" value="${log.maxSize}"/>
        </rollingPolicy>
    </appender>

    <appender name="dbAccessAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.dir}/dbsource.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <param name="fileNamePattern" value="${log.dir}/dbsource-%d{yyyy-MM-dd}.log"/>
            <param name="maxHistory" value="${log.maxHistory}"/>
        </rollingPolicy>
        <encoder>
            <pattern>${pattern}</pattern>
        </encoder>
    </appender>
    <appender name="redisAccessAppender"
              class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.dir}/redis.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <param name="fileNamePattern" value="${log.dir}/redis-%d{yyyy-MM-dd}.log"/>
            <param name="maxHistory" value="${log.maxHistory}"/>
        </rollingPolicy>
        <encoder>
            <pattern>${log.pattern}</pattern>
        </encoder>
    </appender>

    <!--自定义logger-->
    <logger name="com.atlantis.zeus.base.db" level="warn" additivity="false">
        <appender-ref ref="dbAccessAppender" />
    </logger>
    
    <logger name="com.xupt.redis.storage" level="warn" additivity="false">
        <appender-ref ref="redisAccessAppender" />
    </logger>
    
    <logger name="com.atlantis" additivity="false">
        <appender-ref ref="info_appender" />
        <appender-ref ref="error_appender" />
    </logger>
    
    <root level="INFO">
        <appender-ref ref="console"/>
    </root>
</configuration>
```

