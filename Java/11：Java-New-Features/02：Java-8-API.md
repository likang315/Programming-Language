### Java-8 API

------

[TOC]

##### 01：Optional< T >

​	一个容器，可以保存T，或者仅仅用于保存null，用于解决空指针异常，当出现null时，选用备选值；

###### 方法

- static <T> Optional<T>  ofNullable(T value)； 返回一个Optional对象，value可以为空

  ```java
  // 使用教程
  Optional.ofNullable(null).orElseGet(() -> ("fds"));
  ```

- optional.orElse(T other)；

  - 如果Optional为null，则返回other，如果不为null，则执行other，但是返回原对象

- optional.orElseGet(Supplier<? extends T> other)；

  - 如果Optional为null，则执行other并返回，一种回退机制，若不为null，other流不会执行；

- optional.orElseThrow(Supplier<? extends X> exceptionSupplier)

  - 如果Optional为null，则执行exceptionSupplier，并抛出异常，不会使用备选值

- map(Function<? super T, ? extends U> mapper)

  - 转换功能，然后将返回的值包装在 Optional 中。这就使对返回值进行链试调用的操作成为可能

- filter(Predicate<? super T> predicate)

  - 接受一个 Predicate 参数，返回测试结果为 true 的值会留下，如果测试结果为 false，会返回一个空的 Optional
  
- ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)

  - 如果一个 Optional 包含值，则对其包含的值调用函数 action，即 action.accept(value)，如果 Optional 不包含值，那么 ifPresentOrElse 便会调用 emptyAction，即 emptyAction.run()。

  - ```java
    Optional<Integer> optional = Optional.of(1);
    optional.ifPresentOrElse( x -> System.out.println("Value: " + x),() -> 
                             System.out.println("Not Present."));
    ```

- or(Supplier<? extends Optional<? extends T>> supplier)

  - 如果值存在，返回 Optional 指定的值，否则返回一个预设的值。

  - ```java
    Optional<String> optional1 = Optional.of("Mahesh");
    optional1 = optional1.or(() -> Optional.of("Not Present"););
    ```


```java
public final class Optional<T> {
  private static final Optional<?> EMPTY = new Optional<>();
  private final T value;
  public static<T> Optional<T> empty() {
    @SuppressWarnings("unchecked")
    Optional<T> t = (Optional<T>) EMPTY;
    return t;
  }
  public static <T> Optional<T> ofNullable(T value) {
    return value == null ? empty() : of(value);
  }
  
  private Optional(T value) {
        this.value = Objects.requireNonNull(value);
  }
  public static <T> Optional<T> of(T value) {
        return new Optional<>(value);
  }
	// 不为null时，获取值
  public T get() {
    if (value == null) {
      throw new NoSuchElementException("No value present");
    }
    return value;
  }
  
  public boolean isPresent() {
    return value != null;
  }
  // 供给型接口，函数体直接作为返回值
  public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier)
    throws X {
        if (value != null) {
            return value;
        } else {
            throw exceptionSupplier.get();
        }
    }
  
  public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent())
      return empty();
    else {
      return Optional.ofNullable(mapper.apply(value));
    }
  }

  public Optional<T> filter(Predicate<? super T> predicate) {
    Objects.requireNonNull(predicate);
    if (!isPresent())
      return this;
    else
      return predicate.test(value) ? this : empty();
  }
}
```

##### 02：Objects

```java
public final class Objects {
    /**
   	* 私有构造函数无法New对象
   	*/
    private Objects() {
        throw new AssertionError("No java.util.Objects instances for you!");
    }

    /**
    * 比较两个参数值相等，且不为null
    */
    public static boolean equals(Object a, Object b) {
        return (a == b) || (a != null && a.equals(b));
    }

   /**
    * 深度比较，增加比较数组的每个值的方法
    */
   public static boolean deepEquals(Object a, Object b) {
        if (a == b)
            return true;
        else if (a == null || b == null)
            return false;
        else
            return Arrays.deepEquals0(a, b);
    }

    /**
     * 非空参数的哈希码，空参数的哈希码为0
     */
    public static int hashCode(Object o) {
        return o != null ? o.hashCode() : 0;
    }

  	/**
  	* 调用非空参数的toString和空参数的“null”的结果
    */
    public static String toString(Object o, String nullDefault) {
        return (o != null) ? o.toString() : nullDefault;
    }
    
    /**
     * 判断是否为null
     */
    public static boolean isNull(Object obj) {
        return obj == null;
    }
}
```

##### 03：Stream

- 将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等；

- ```java
  public interface Stream<T> extends BaseStream<T,Stream<T>>
  +--------------------+       +------+   +------+   +---+   +-------+
  | stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
  +--------------------+       +------+   +------+   +---+   +-------+
  ```

1. 生成流

   - 集合接口提供两个方法生成流
     - **stream()** − 为集合创建串行流；
     - **parallelStream()** − 为集合创建并行流；

2. Stream操作被分为**中间操作**和**终点操作**

   - 中间操作：返回一个新的Stream，这个操作是有延迟的；
   - 终点操作：比如说forEach或者sum会遍历Stream从而产生最终结果或附带结果。终点操作执行完之后，Stream管道就被消费完了，不再可用。在几乎所有的情况下，**终点操作都是完成对数据的遍历操作**。

3. ###### void forEach(Consumer<? super T> action)

   - 接收一个 Consumer 接口，它只接收参数，没有返回值。然后在 Stream 的每一个元素上执行该表达式

   ```java
   Random random = new Random();
   // 产生Integer Stream -> 限制10个元素 -> 遍历输出 -> 当前元素调用
   random.ints().limit(10).forEach(System.out::println);
   ```

4. ###### Stream<T>  filter (Predicate<? super T> predicate) 

   - 用于通过设置的条件过滤出元素
   - filter 中的过滤条件返回`true`代表当前元素会**保留下来**；

   ```java
   List<String> str = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl"); 
   // 获取空字符串的数量 
   long count = str.stream().filter(s -> s.isEmpty()).count();
   ```

5. ###### removeIf  

   - Collection 的方法，移除，过滤条件为 true  的会移除掉
   - 和流没关系，这个是函数式接口；

   ```java
   List<String> strings = new ArrayList<>();
   List<String> str = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
   // 当调用Arrays.asList()方法时，返回值并非我们常用的java.util.ArrayList，而是java.util.Arrays.ArrayList，此类虽然也继承了AbstractList抽象类，但是并没有去实现remove
   strings.addAll(str);
   // 移除为空的字符串
   strings.removeIf(s -> "".equals(s));
   System.out.println(strings);
   ```

6. ###### Stream<R> map(Function<? super T, ? extends R> mapper);

   - map 方法接收一个功能型接口，功能型接口接收一个参数，返回一个值，返回的是新的流;
   - 用于将旧数据转换后变为新数据，每个输入元素按照规则转换成另一个元素；
   - 该方法是 Intermediate【中间】 操作；

   ```java
   Stream<String> stream = Stream.of("a", "b", "c", "d");
   stream.map(String::toUpperCase).forEach(System.out::println);
   ```

7. ###### Stream<T> limit(long maxSize)

   - 获取前 maxSize 个元素；

8. ###### Stream<T> sorted(Comparator<? super T> comparator)

   - 根据传入的外比较器进行比较；

   ```java
   // 传入的必须是JavaBean
   List<Student> sortDesList = list.stream()
       .sorted(Comparator.comparing(Student::getAge)
       .reversed())
       .forEach(System.out::println);
   // 内比较器，实现类
   List<Student> sortList1 = list.stream()
       .sorted((a, b) -> a.getAge().compareTo(b.getAge()))
       .collect(Collectors.toList());
   ```

9. ###### <R, A> R collect(Collector<? super T, A, R> collector)

   - 收集器，终端归约操作，产生最终的结果；
   - java.util.stream.Collector 是一个收集函数的接口, 声明了一个收集器的功能；

   ```java
   List<String> filtered = str.stream()
   	.filter(string -> !string.isEmpty())
     .collect(Collectors.toList());
   ```

10. ###### 按照某种规则进行分组

   - ```java
     final Map<Status, List<Task>> map = tasks
         .stream()
         .collect(Collectors.groupingBy(Task::getStatus));
     System.out.println(map);
     ```

11. Stream的方法**onClose** 返回一个等价的有额外引用的Stream，当Stream的close（）方法被调用的时候这个句柄会被执行

    ```java
    final Path path = new File( filename ).toPath();
    try (Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8)) {
        lines.onClose(() -> System.out.println("Done!")).forEach( System.out::println );
    }
    ```

12. Stream<T> takeWhile(Predicate<? super T> predicate)

    - takeWhile() 方法使用一个断言作为参数，返回给定 Stream 的子集直到断言语句第一次返回 false。如果第一个值不满足断言条件，将返回一个空的 Stream。

    - ```java
       Stream.of("a","b","c","","e","f").takeWhile(s->!s.isEmpty())        
           .forEach(System.out::print);   
       // abc
       ```

13. Stream<T> dropWhile(Predicate<? super T> predicate)

    - 和 takeWhile 作用相反的，使用一个断言作为参数，直到断言语句第一次返回 false 才返回给定 Stream 的子集。

    - ```java
       Stream.of("a","b","c","","e","f").dropWhile(s-> !s.isEmpty())
                .forEach(System.out::print);
       // ef
       ```

14. Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next)

    - 使用初始种子值创建顺序流，并迭代应用指定的下一个方法。 当指定的 hasNext 的 predicate 返回 false 时，迭代停止。

    - ```java
      IntStream.iterate(3, x -> x < 10, x -> x+ 3).forEach(System.out::println);
      // 3 6 9
      ```

15. Stream<T> ofNullable(T t)

    - 创建一个包含单个元素的 `Stream`，如果传入的元素为 null，则返回一个空的 `Stream`。

    - ```java
       String str = "Hello, world!";
       // 包含一个元素的 Stream
       Stream<String> stream = Stream.ofNullable(str)
       ```


##### 04：Collector

- Collector 是专门用来作为Stream的collect方法的参数的；

###### Collectors

- 工具类，内部提供了多种Collector，直接拿来使用；

  - **toCollection：**将流中的元素全部放置到一个集合中返回，这里使用Collection，泛指多种集合；

  - **toList：**将流中的元素放置到一个列表集合中去。这个列表默认为ArrayList；

  - **toSet：**将流中的元素放置到一个无序集set中去。默认为HashSet；

  - **toMap：**根据给定的键生成器和值生成器生成的键和值保存到一个map中返回，可以指定出现重复键时的处理方案和保存结果的map；

  - ```java
    // 指定键和值的生成方式keyMapper和valueMapper
    Map<String,String> map = list.stream().limit(3).collect(Collectors
    		.toMap(e -> e.substring(0,1),
               e -> e));
    // 在上面方法的基础上增加了对键发生重复时处理方式的mergeFunction，比如上面的默认的处理方法就是抛出异常
    Map<String,String> map1 = list.stream().collect(Collectors
    		.toMap(e -> e.substring(0,1),
               e -> e,
               (a,b) -> b));
    // 在第二个方法的基础上再添加了结果Map的生成方法
    Map<String,String> map2 = list.stream().collect(Collectors
    		.toMap(e -> e.substring(0,1),
               e -> e,
               (a,b) -> b,
               HashMap::new));
    ```

  - **joining(CharSequence delimiter, CharSequence prefix, CharSequence suffix)**

    - 将流中的元素全部以字符序列的方式连接到一起，可以指定连接符，甚至是结果的前后缀；

  - ```java
    String sss = list.stream().collect(Collectors.joining("-","S","E"));
    ```

  - **groupingBy ：**分组

    ```java
    groupingBy(Function<? super T, ? extends K> classifier) {
        return groupingBy(classifier, toList());
    }
    
    public static <T, K, A, D> Collector<T, ?, Map<K, D>> groupingBy(
      Function<? super T, ? extends K> classifier,
      Collector<? super T, A, D> downstream) {
            return groupingBy(classifier, HashMap::new, downstream);
    }
    
    public static <T, K, D, A, M extends Map<K, D>> Collector<T, ?, M> 
      groupingBy(Function<? super T, ? extends K> classifier,
                                      Supplier<M> mapFactory,
                                      Collector<? super T, A, D> downstream) {}
    ```

  - ```java
    // 只需一个分组参数classifier，内部自动将结果保存到一个map中，每个map的键为?类型（即classifier的结果类型），值为一个list，这个list中保存在属于这个组的元素
    Map<Integer,List<String>> s = list.stream().collect(
      Collectors.groupingBy(String::length));
    // 在上面方法的基础上增加了对流中元素的处理方式的Collector，比如上面的默认的处理方法就是Collectors.toList()
    Map<Integer,List<String>> ss = list.stream().collect(Collectors.groupingBy(String::length, Collectors.toList()));
    // 在第二个方法的基础上再添加了结果Map的生成方法
    Map<Integer,Set<String>> sss = list.stream().collect(
      Collectors.groupingBy(
        String::length,
        HashMap::new,
        Collectors.toSet()));
    ```
