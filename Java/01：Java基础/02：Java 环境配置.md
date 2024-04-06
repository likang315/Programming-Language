### 认识 Java 语言

------

##### 01：java语言发展历程

- 1995年 由Sun公司提出，2010年 被Oracle公司收购
- java之父：James Gosling

##### Java版本：

- javaME：微型版-------塞班手机（诺基亚）
- javaSE：标准版
- javaEE：企业版
  - javaSE是java的基础，主要用于桌面应用程序的开发，javaEE主要用于开发企业级分布式的网络程序，javaME主要用于嵌入式系统开发

##### 03：Java 语言的特点

- 面向对象
- 单继承
- 跨平台：它摆脱了硬件平台的束缚，实现了“一次编写，到处运行”的理想；
- **JVM（Java Virtual Machine）**：相对安全的内存管理和访问机制;
  - Classic VM：世界上第一款商用Java虚拟机；
  - JDK 3 时，HotSpot VM成为默认虚拟机；(**武林盟主**)

- **JDK（Java Development Kit）**：Java程序设计语言、Java虚拟机、Java类库这三部分统称为JDK;
- **JRE（Java Runtime Environment）**：把 Java 类库 API 中的Java SE API子集和Java虚拟机；

##### 04：配置环境变量：

- step1:安装jdk 
- step2:配置path
- step3:配置classpath
- 环境变量的配置：把jdk安装目录下的bin目录，复制，在计算机的属性中找到环境变量，在系统变量中找到path，点击编辑，将光标移动到开始为置粘贴，完了之后一级一级点击确定
  - JAVA_HOME :找到jdk开发工具包  ---------用来辅助配置path的
  - CLASSPATH：告诉编译器字节码文件所在的位置 -----//当前目录下
  - .;C:\Program Files\Java\jdk1.8.0_171\;C:\Program Files\Java\jdk1.8.0_171\jre\bin
  - 注意：
    - java -version ，java，javac来测是否配置成功
  - path 是用来搜索所执行的可执行文件路径的，如果执行的可执行文件不在当前目录下，那就会依次搜索path中设置的路径，而CLASSPATH是用来告诉编辑器在那里寻找Java编译过程中所需的包和类所以其路径中配置的是lib目录下的tools.jar;

##### 05：Java 编译器

- 将 Java **源代码编译成 Java 字节码**，是 Java 开发工具包（JDK）的一部分，它可以在命令行或集成开发环境（IDE）中使用。

##### 06：Java 解释器

- 是 Java虚拟机（JVM）的一部分，将**Java字节码解释成机器码**以在计算机上执行程序。

##### 07：Java 语言的执行过程

1. 编译器将 Java 代码编译成 Java 字节码；
2. 将字节码文件（class文件）加载到内存；
3. 对每个字节码都实例化一个 java.lang.Class 的对象
4. 进行连接，初始化；
5. 实始化静态成员变量，静态代码块，放入方法区；
6. 执行入口方法main；
   - java程序的运行机制 ： 源文件（.java) -> 编译器(javac)生成 ->字节码文件（.class）-> 解释器（java）-> 机器码
   - javac a.java   -----编译
   - java a         -----解释执行

###### 编写 java 程序时需要注意：

- .java 源文件名称要和该文件中public修饰的类名名称完全一样，在一个源文件当中可以存在多个类		