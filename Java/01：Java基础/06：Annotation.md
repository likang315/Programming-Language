### Annotation

------

[TOC]

##### 00：概述

- 概述：相当于一种标记，在程序中加入注解就等于为程序打上某种标记，标记可以加在包、类、属性、方法，方法的参数以及局部变量上，**一旦加上标记，程序要实例化此类**，在底层实现上，所有自定义的注解都会自动继承java.lang.annotation.**Annotation**接口。

##### 01：注解 JDK1.5 的特性，内部提供了三种注解

- @SuppressWarnings("deprecation")      
  - 压缩警告：给编译器一条指令，告诉它对被标记的代码**元素内部的某些警告保持静默（不起作用）**

- @Deprecated

  - 过期的

- @Retention(value=RUNTIME)

  - 被它所注解的注解保留多久

##### 02：在注解类上使用另一个注解类，那么被使用的注解类就称为元注解

​	判断一个类有没有加注解，只能通过反射测试

##### 03：注解的生命周期(三个阶段)

1. java源文件
2. class文件
3. 内存中的运行阶段（Runtime）

- 自定义注解时使用 **Retention注解指明自定义注解的生命周期**，默认是保留到RetentionPolicy.CLASS阶段 
- 自定义注解的生命周期阶段
  - RetentionPolicy.SOURCE (java源文件阶段)；
  - RetentionPolicy.CLASS  (class文件阶段)；
  - RetentionPolicy.RUNTIME(内存中运行阶段)，基本都配置这个；
    - 例：@Retention(value=RUNTIME)

##### 04：@Target

​	限定了注解使用的类型范围，只能标识到指定程序的元素类型上，**若注解类型声明中不存在Target元注解，默认的是用于任何元素类型**

- ElementType：程序元素类型
  - ANNOTATION_TYPE 
    - 注释类型声明 
  - CONSTRUCTOR 
    - 构造方法声明 
  - FIELD 
    - 字段声明（包括枚举常量） 
  - LOCAL_VARIABLE 
    - 局部变量声明 
  - METHOD 
    - 方法声明 
  - PACKAGE 
    - 包声明 
    - 如果一个注解的@Target是定义为Element.PACKAGE，那么这个注解是使用在package-info.java中的，而不能直接在某个类的package代码上面使用；
  - PARAMETER 
    - 参数声明 
  - TYPE 
    - 类、接口（包括注释类型）或枚举声明 
  - 例：@Target( { ElementType.METHOD, ElementType.TYPE })

##### 05：自定义注解

- 使用自定义注解，其作用一定要利用AOP进行切面编程，可以利用其注解实现调用目标方法之前、之后做一些逻辑业务；
- 需要使用 **@interface** 关键字，配合反射使用；
- 自定义注解时，内部需要使用**注解类型元素**，语法如下：
  - 访问修饰符必须为public，不写默认为public；
  - 该元素的类型只能是基本数据类型、String、Class、枚举类型、注解类型（体现了注解的嵌套效果）、数组；
  - 该元素的名称一般定义为名词，如果注解中只有一个元素，请把名字起为value，方便使用时不用写key；
  - **( )不是定义方法参数的地方，也不能在括号中定义任何参数，仅仅只是一个特殊的语法；**
  - default：代表默认值，如果没有默认值，代表后续使用注解时必须给该类型元素赋值
  - 语法设计原因：**使用的时候操作元素类型像在操作属性，解析的时候操作元素类型像在操作方法**。
- 主要用于登录校验和日志打印；

```java
// 定义属性值
@Target(value = {ElementType.METHOD})
@Retention(value=RUNTIME)
public @interface MyAnnotation {
	String name();
	int[] ints();
  // 类型 属性名() default 默认值 
	String color() default "blue"；
}
// 使用注解
@MyAnnotation(name = "hahaha"，ints = {1,3},)  
// 定义其自定义注解作用
@Around(value="@annotation(自定义注解的全路径类名)")
public Object processFunction(ProceedingJoinPoint point) throws Throwable {
 		Signature si = point.getSignature();
    if (si instanceof MethodSignature) {
         Method method = ((MethodSignature) si).getMethod();
      	 // 获取自定义注解
         MyAnnotation annotation = method.getAnnotation(MyAnnotation.class)；
         String value = annotation != null ? annotation.name()
                    : String.format("%s_%s", method.getClass().getSimpleName(),
                                    method.getName());
      	 Object result = point.proceed();
      	 // 获取自定义注解的属性值
      	 System.out.println(value);
      	 return result;
    else {
      return point.proceed();
    }
}
```

