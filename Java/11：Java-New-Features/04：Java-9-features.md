### Java 9 特性

------

[TOC]

##### 01：概述

- Java 9 发布于 2017 年 9 月 22 日，带来了很多新特性，其中最主要的变化是已经实现的模块化系统。

##### 02：新特性

- **模块系统**：模块是一个包的容器，Java 9 最大的变化之一是引入了模块系统（Jigsaw 项目）。
- **REPL (JShell)**：交互式编程环境。
- **HTTP 2 客户端**：HTTP/2标准是HTTP协议的最新版本，新的 HTTPClient API 支持 WebSocket 和 HTTP2 流以及服务器推送特性。
- **改进的 Javadoc**：支持在 API 文档中的进行搜索。另外，Javadoc 的输出现在符合兼容 HTML5 标准。
- **多版本兼容 JAR 包**：多版本兼容 JAR 功能能让你创建仅在特定版本的 Java 环境中运行库程序时选择使用的 class 版本。
- **集合工厂方法**：List，Set 和 Map 接口中，新的静态工厂方法可以创建这些集合的不可变实例。
- **私有接口方法**：在接口中使用private私有方法。
- **进程 API**: 改进的 API 来控制和管理操作系统进程。
- **改进的 Stream API**：改进的 Stream API 添加了一些便利的方法，使流处理更容易，并使用收集器编写复杂的查询。
- **改进 try-with-resources**：如果你已经有一个资源是 final 或等效于 final 变量，您可以在 try-with-resources 语句中使用该变量，而无需在 try-with-resources 语句中声明一个新变量。
- **改进的弃用注解 @Deprecated**：注解 @Deprecated 标记 Java API 状态，表示被标记的 API 将会被移除，或者已经破坏。
- **改进钻石操作符(Diamond Operator)** ：匿名类可以使用钻石操作符(Diamond Operator)。
- **多分辨率图像 API**：定义多分辨率图像API，开发者可以很容易的操作和展示不同分辨率的图像了。
- **改进的 CompletableFuture API** ： CompletableFuture 类的异步机制可以在 ProcessHandle.onExit 方法退出时执行操作。
- **轻量级的 JSON API**：内置了一个轻量级的JSON API
- **响应式流（Reactive Streams) API**: Java 9中引入了新的响应式流 API 来支持 Java 9 中的响应式编程。
- **G1 默认垃圾回收器**

##### 03：模块系统

- 模块：一个被命名的，代码和数据的自描述集合。一种新型的程序设计组件；

- 可以简单理解为是 package 的上级容器，是多个 package 的集合，一个 jar  可以有多个module，一个module可以有多个package。从代码结构上看，jar > **module** > package > class, interface。

- 模块通过 **requires 和 exports** 关键字，对自身所依赖（requires）的模块和自身暴露（exports）出去的内容（package）进行了声明。与 maven 相辅相成。

- ```java
  // 模块描述文件
  module modular.service {
      exports com.hanmc.example.modulardemo.service;
      
      requires spring.context;
      requires spring.beans;
  }
  ```

##### 04：JShell

- Shell 是 Java 9 新增的**一个交互式的编程环境工具**。它允许你无需使用类或者方法包装来执行 Java 语句。它与 **Python 的解释器类似**，可以直接 输入表达式并查看其执行结果。

- ```shell
  jshell> /help
  jshell> 3+1
  $1 ==> 4
  
  /exit
  ```

##### 05：Javadoc

- ```shell
  # 使用javadoc 生成文档
  javadoc -d C:/JAVA Tester.java
  ```

##### 06： 新的静态工厂方法，创建集合的不可变实例

- Java 9 中，以下方法被添加到 List，Set 和 Map 接口以及它们的重载对象。

  - static <E> List<E> of(E e1, E e2, E e3);

  - static <E> Set<E>  of(E e1, E e2, E e3);

  - static <K,V> Map<K,V> of(K k1, V v1, K k2, V v2, K k3, V v3);

  - static <K,V> Map<K,V> ofEntries(Map.Entry<? extends K,? extends V>... entries)

    - List 和 Set 接口, of(...) 方法重载了 0 ~ 10 个参数的不同方法 。

  - ```java
    Map<String, Integer> map = Map.ofEntries(
            Map.entry("apple", 1),
            Map.entry("banana", 2),
            Map.entry("orange", 3)
    );
    
    // Java 7 中引入的注解，用于标记方法或构造函数中使用可变长参数（varargs），以确保在编译时不会产生类型安全警告
    @SafeVarargs
    @SuppressWarnings("varargs")
    static <K, V> Map<K, V> ofEntries(Entry<? extends K, ? extends V>... entries) {
        if (entries.length == 0) {
            return ImmutableCollections.emptyMap();
        } else if (entries.length == 1) {
            return new ImmutableCollections.Map1<>(entries[0].getKey(),
                                                   entries[0].getValue());
        } else {
            Object[] kva = new Object[entries.length << 1];
            int a = 0;
            for (Entry<? extends K, ? extends V> entry : entries) {
                // implicit null checks of each array slot
                kva[a++] = entry.getKey();
                kva[a++] = entry.getValue();
            }
            return new ImmutableCollections.MapN<>(kva);
        }
    }
    ```

##### 07：私有接口方法

- Java 9 不仅像 Java 8 一样支持接口默认方法，同时还支持私有方法。

  在 Java 9 中，一个接口中能定义如下几种变量/方法：

  - 常量
  - 抽象方法
  - 默认方法：Java 8
  - 静态方法：Java 8
  - **私有方法：**Java 9
  - **私有静态方法**：提供辅助功能或实现细节，供类内部的其他方法使用；

##### 08：新的进程 API

- Java 9 向 Process API 添加了一个名为 ProcessHandle 的接口来增强 java.lang.Process 类。

- ```java
  ProcessBuilder pb = new ProcessBuilder("notepad.sh");
  Process p = pb.start();
  // 进程当前的快照
  ProcessHandle.Info info = p.info();
  ```

##### 09：新的 Stream API

- Java 9 为 Stream 新增了几个方法：dropWhile、takeWhile、ofNullable，为 iterate 方法新增了一个重载方法。
- 【详情见：Java 8 Stream】

##### 10：改进的 try - with - resource

- 在 `try` 语句中使用资源（`resource`）时，可以**在 `try` 关键字后面直接声明资源，而无需在 `try` 代码块前面显式声明**。这项特性称为“改进后的 try-with-resources”。

- 改进点：如果你已经有一个资源是 final 或等效于 final 变量，您可以在 try-with-resources 语句中使用该变量，而无需在 try-with-resources 语句中声明一个新变量。

- ```java
  try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
      String line;
      while ((line = br.readLine()) != null) {
          System.out.println(line);
      }
  } catch (IOException e) {
      // 异常处理
  }
  ```

##### 11：G1 成为默认垃圾回收器

- 在 Java 8 的时候，默认垃圾回收器是 Parallel Scavenge（新生代）+ Parallel Old（老年代）。到了 Java 9, CMS 垃圾回收器被废弃了，**G1（Garbage-First Garbage Collector）** 成为了默认垃圾回收器。