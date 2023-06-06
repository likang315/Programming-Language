### StringJoiner

------

##### 01：概述

- 用于分隔符拼接字符串，JDK1.8 新特性，底层是StringBuilder。

##### 02：源码

```java
public final class StringJoiner {
    private final String prefix;
    private final String delimiter;
    private final String suffix;

    private StringBuilder value;

    public StringJoiner(CharSequence delimiter) {
        this(delimiter, "", "");
    }
    public StringJoiner(CharSequence delimiter,
                        CharSequence prefix,
                        CharSequence suffix) {
        Objects.requireNonNull(prefix, "The prefix must not be null");
        Objects.requireNonNull(delimiter, "The delimiter must not be null");
        Objects.requireNonNull(suffix, "The suffix must not be null");
        this.prefix = prefix.toString();
        this.delimiter = delimiter.toString();
        this.suffix = suffix.toString();
        this.emptyValue = this.prefix + this.suffix;
    }

    public StringJoiner add(CharSequence newElement) {
        prepareBuilder().append(newElement);
        return this;
    }
    private StringBuilder prepareBuilder() {
        if (value != null) {
            value.append(delimiter);
        } else {
            value = new StringBuilder().append(prefix);
        }
        return value;
    }
}
```

##### 03：示例

- ```java
  public static void main(String[] args) {
      StringJoiner stringJoiner = new StringJoiner(":").add("hello").add("likang").add("西安");
      System.out.println(stringJoiner.toString());
      // 底层是 StringJoiner
      System.out.println(String.join("&", "hello", "likang", "西安"));
  }
  ```

  