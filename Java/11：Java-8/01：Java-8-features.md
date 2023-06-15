### Java 8 特性

------

[TOC]

##### 01：函数式接口

###### 定义：函数式接口，只包含一个抽象方法的接口，可以使用 Lambda 表达式或方法引用来创建该接口的实例。

- 这种特性非常脆弱，只要多加一个方法就会编译失败，因此提供了一个**@FunctionalInterface**注解来显示的申明函数接口

- 传入的是一个函数，一段执行逻辑代码

- Java 8 提供了很多函数式接口：Java.util.function包下

  - Consumer< T >：**消费型接口，接收一个参数进行消费，但无需返回结果;**

    ```java
    @FunctionalInterface
    public interface Consumer<T> {
        void accept(T t);
    }
    ```

  - Supplier< T >：**供给型接口，无参数，有返回值**。

    ```java
    @FunctionalInterface
    public interface Supplier<T> {
        T get();
    }
    ```

- Function<T，R>：**函数型接口，R apply(T t)，传入一个参数，返回想要的结果**;

  - T 作为传入的参数，R 作为返回结果

  - ```java
    @FunctionalInterface
    public interface Function<T, R> {
      	// 默认和static方法除外
    R apply(T t);
    }
    ```

  - Predicate<T> ： **断言型接口，传入一个参数，返回一个布尔值**；

    ```java
    @FunctionalInterface
    public interface Predicate<T> {
    	boolean test(T t);
    }
    ```

  - BiConsumer<T, U>：用于Map时的操作，BiXXX 都是用于操作Map的函数式接口

    ```java
    @FunctionalInterface
    public interface BiConsumer<T, U> {
        void accept(T t, U u);
    }
    ```

###### 示例

```java
ArrayList<String> list = new ArrayList<>(Arrays.asList("I", "love", "you", "too"));
// 消费型接口，传入一个参数
list.forEach(new Consumer<String>() {
    @Override
    public void accept(String str) {
        if (str.length() > 3) {
            System.out.println(str);
        }
    }
});

// 使用Lambda表达式迭代, 隐式的使用Consumer接口
list.forEach(str -> {
    if (str.length() > 3)
        System.out.println(str);
});
```

##### 02：接口的默认方法和静态方法

- Java 8增加了两个新的概念在接口声明的时候：默认的、静态方法；
- **default 、static修饰的方法在接口中可以有方法体；**
- static 修饰的接口中定义的方法不能被重写，但可以有方法体；
- 两个方法没什么用，主要是为了兼容以前的旧接口；
- 默认方法和抽象方法的区别是抽象方法必须要被实现，默认方法不是。作为替代方式，接口可以提供一个默认的方法实现，所有这个接口的实现类都会通过继承得到这个方法（如果有需要也可以重写这个方法）

```java
public interface ABTest {
    // 此方法无用，无法调用
    static void ATest () {
        System.out.println("test...");
    }
    // 实现类可以重写，调用
    default void BTest() {
        System.out.println("test default...");
    }
}
```

##### 03：Lambda	

​	允许把函数作为一个方法的参数，传入一个动作；

- （参数）-> 方法体   ：作为某个方法的参数;
- 一个lambda由用**逗号分隔的参数列表、–>、函数体**三部分表示;
- 解决匿名内部类复杂的操作而出现，因为匿名内部类需要依赖接口，实现回调方法；
  - 编写匿名内部类编译时会多一个class文件，而是用lambda编译之后不会；

###### 语法格式

```java
(parameters) -> expression
(parameters) ->{statements;}
```

- 可选类型声明：不需要声明参数类型，编译器会统一识别参数值
  
  - ```java
    Collections.sort(list, (x,y) -> x - y);
    ```
- 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号
  
  - ```
    x -> 2 * x
    ```
- 可选的大括号：如果主体包含了一个语句，就不需要使用大括号
  
  - ```
    (int x, int y) -> x + y
    ```
- 可选的返回关键字：如果主体只有一个表达式返回值则编译器会**自动返回值**，大括号需要指定明表达式返回了一个数值
  
  - ```java
    (String s) -> System.out.print(s)   只打印，不返回任何值
    ```

###### 变量作用域

- lambda 表达式中引用的局部变量，尽量被定义为final的局部变量；

  ```java
  final String separator = ",";
  Arrays.asList("a", "b", "d").forEach((String e) -> System.out.print(e + separator));
  ```

- Lambda 表达式不能修改局部变量；

- Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量；

- Lambda 表达式可能会有返回值，如果只有一行，不需要使用return，它会自动根据上下文识别返回值类型；

  ```java
  Arrays.asList("a", "b", "d").sort((e1, e2) -> e1.compareTo(e2));
  Arrays.asList("a", "b", "d").sort((e1, e2) -> {
    // 加了大括号，等价的  
    int result = e1.compareTo(e2);
    return result;
  });
  ```

##### 04：方法的引用

###### 定义

- 方法引用是用来直接访问类或者实例中已经存在的方法或者构造方法；


###### 使用场景

- 当Lambda表达式中只是执行一个方法调用时；


###### 使用方式

- 使用一对 : :
- 无参构造方法：
  - 语法：Class::new，或者更一般的Class< T >::new
- 静态方法
  - 语法：Class::static_method_name
- 无参实例方法
  - 语法：Class::method
- 实例方法,只接受某个类型的参数
  - 语法：instance::method
- 当 lambda 调用的方法是父类中的方法或者是当前类中的方法时，可以使用 super::实例方法名 或 this::实例方法名

```java
final Car police = Car.create(Car::new);
// follow 只能是car类型
cars.forEach(police::follow);
```

##### 05：重复注解

###### 定义：

​	允许相同注解在声明使用的时候重复使用超过一次，重复注解本身需要被@Repeatable注解，它只是编译器层面的改动；

###### 反射的API提供一个新方法 getAnnotationsByType() 来返回重复注释的类型

```java
public class RepeatingAnnotations {
    @Target( ElementType.TYPE )
    @Retention( RetentionPolicy.RUNTIME )
    public @interface Filters {
        Filter[] value();
    }

    @Target( ElementType.TYPE )
    @Retention( RetentionPolicy.RUNTIME )
    @Repeatable( Filters.class )
    public @interface Filter {
        String value();
    };

    // 重复使用
    @Filter("filter1")
    @Filter("filter2")
    public interface Filterable {
    }

    public static void main(String[] args) {
        // 返回使用的两个注解
        for(Filter filter: Filterable.class.getAnnotationsByType(Filter.class) ) {
            System.out.println(filter.value() );
        }
    }
}
```

###### 注解的扩展

扩展了注解的使用范围，现在注解基本上可以用在任何地方

- ElementType.TYPE_USE：对类型的注解
- ElementType.TYPE_PARAMETERL：对参数的注解

##### 06：参数名字保留在字节码文件里

- 使用反射API，Parameter.getName() 方法和字节码里（使用java编译命令javac的**–parameters**参数）

- ```java
  public static void main(String[] args) throws Exception {
      Method method = ParameterNames.class.getMethod("main", String[].class);
      for (final Parameter parameter: method.getParameters()) {
        System.out.println("Parameter: " + parameter.getName());
      }
  }
  ```

##### 07：并行数组

- Arrays  数组的工具类，提供了许多并行的方式提供效率；

  - static void`  `parallelSetAll(int[] array, IntUnaryOperator generator) 
    - 使用生成器对指定的数组进行填充；
  - static void  parallelSort(int[] a) 
    - 并行排序，升序

  ```java
  Arrays.parallelSetAll(arrayOfLong,index -> ThreadLocalRandom.current().nextInt( 1000000));
  Arrays.parallelSort(arrayOfLong);
  ```

