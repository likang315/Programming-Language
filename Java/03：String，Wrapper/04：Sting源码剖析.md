### String 源码剖析

------

[TOC]

##### String 构造方法

```java
package java.lang;
// 首先lang包下不需要导包
// final 不可被继承
// 实现的三个接口分别可以序列化，排序，有序字符序列
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    // 用来存储字符串,本质是一个final的数组，基定了不可变的因素
    private final char value[];

    // 缓存字符串的哈希
    private int hash; // Default to 0

    // 序列化版本号
    private static final long serialVersionUID = -6849794470754667710L;

    // 无参，空白串
    public String() {
        this.value = "".value;
    }

    // 深拷贝了一个形参String对象
    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }

    // 拷贝到字符数组
    public String(char value[]) {
        this.value = Arrays.copyOf(value, value.length);
    }

    // 从offset开始，截取count个长度的字符集构成一个新的String对象
    public String(char value[], int offset, int count) {
        if (offset < 0) {
            throw new StringIndexOutOfBoundsException(offset);
        }
        if (count <= 0) {
            if (count < 0) {
                throw new StringIndexOutOfBoundsException(count);
            }
            // count=0  空串
            if (offset <= value.length) {
                this.value = "".value;
                return;
            }
        }
        if (offset > value.length - count) {
            throw new StringIndexOutOfBoundsException(offset + count);
        }
        this.value = Arrays.copyOfRange(value, offset, offset+count);
    }

    // int[] 传入的是ASCII的整数数组，会转换成对应的字符，然后截取
    public String(int[] codePoints, int offset, int count) {
        if (offset < 0) {
            throw new StringIndexOutOfBoundsException(offset);
        }
        if (count <= 0) {
            if (count < 0) {
                throw new StringIndexOutOfBoundsException(count);
            }
            if (offset <= codePoints.length) {
                this.value = "".value;
                return;
            }
        }
        if (offset > codePoints.length - count) {
            throw new StringIndexOutOfBoundsException(offset + count);
        }

        final int end = offset + count;

        // Pass 1: Compute precise size of char[]
        int n = count;
        for (int i = offset; i < end; i++) {
            int c = codePoints[i];
            if (Character.isBmpCodePoint(c))
                continue;
            else if (Character.isValidCodePoint(c))
                n++;
            else throw new IllegalArgumentException(Integer.toString(c));
        }


        final char[] v = new char[n];
        for (int i = offset, j = 0; i < end; i++, j++) {
            int c = codePoints[i];
            if (Character.isBmpCodePoint(c))
                // 将int类型显式转换为char类型
                v[j] = (char)c;
            else
                Character.toSurrogates(c, v, j++);
        }

        this.value = v;
    }

    // 将StringBuffer构建成一个新的String,比较特别的就是这个方法有同步锁,
    public String(StringBuffer buffer) {
        synchronized(buffer) {
            this.value = Arrays.copyOf(buffer.getValue(), buffer.length());
        }
    }

    // 将StringBuilder构造为新的String对象
    public String(StringBuilder builder) {
        this.value = Arrays.copyOf(builder.getValue(), builder.length());
    }

    // 这个构造函数比较特殊，有用的参数只有char数组value,是一个不对外公开的构造函数,加入这个share参数只是为了区分于String(char[] value)方法，用于重载；
    // 为什么提供这个方法呢，因为性能好，不需要拷贝。如果对外提供会打破value为不变数组的限制
    String(char[] value, boolean share) {
        // assert share : "unshared not supported";
        this.value = value;
    }

}
```

##### 长度、是否为空方法

```java
public int length() {
  return value.length;
}
// ""
public boolean isEmpty() {
  return value.length == 0;
}
```

##### charAt、codePointtAt类型方法

```java
// 返回String对象的char数组index位置的元素
// index不允许小于0，不允许大于等于String的长度
public char charAt(int index) {
  if ((index < 0) || (index >= value.length)) {
    throw new StringIndexOutOfBoundsException(index);
  }
  return value[index];
}

// 返回String对象的char数组index位置的元素的ASSIC码(int类型)
// (int)str.charAt(0)
public int codePointAt(int index) {
  if ((index < 0) || (index >= value.length)) {
    throw new StringIndexOutOfBoundsException(index);
  }
  return Character.codePointAtImpl(value, index, value.length);
}
```

##### getChar、getBytes 方法

```java
// 得到char字符数组，原理是getChars() 方法将一个字符串的字符复制到目标字符数组中。 
public void getChars(int srcBegin, int srcEnd, char dst[], int dstBegin) {
  // 异常抛出方式
  if (srcBegin < 0) {
    throw new StringIndexOutOfBoundsException(srcBegin);
  }
  if (srcEnd > value.length) {
    throw new StringIndexOutOfBoundsException(srcEnd);
  }
  if (srcBegin > srcEnd) {
    throw new StringIndexOutOfBoundsException(srcEnd - srcBegin);
  }
  System.arraycopy(value, srcBegin, dst, dstBegin, srcEnd - srcBegin);
}

// 使用给定字符集编码指定字符串
public byte[] getBytes(Charset charset) {
  if (charset == null) throw new NullPointerException();
  // 内部指定编码工具类, 编码成字节数组，可以解码
  return StringCoding.encode(charset, value, 0, value.length);
}

// 使用平台默认的编码格式获得bytes数组
public byte[] getBytes() {
  return StringCoding.encode(value, 0, value.length);
}
```

##### equal 类方法

```java
// 重写了Object的equals方法（区分大小写）
// 比较的是两个字符串的值是否相等
public boolean equals(Object anObject) {
    // 比较地址是否相等
    if (this == anObject) {
        return true;      
    }
    // 判断str2对象是否是一个String类型，过滤掉非String类型的比较
    if (anObject instanceof String) { 
        String anotherString = (String)anObject;
        int n = value.length;
        // 比较str1的长度和str2的长度是否相等
        if (n == anotherString.value.length) {
            //如是进入核心算法
            char v1[] = value;                  
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) { 
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}

// String的equals方法的补充，与上一个方法不用，该方法(不区分大小写)
public boolean equalsIgnoreCase(String anotherString) {
    return (this == anotherString) ? true
        : (anotherString != null) 
    && (anotherString.value.length == value.length)
        && regionMatches(true, 0, anotherString, 0, value.length);
}

/**
	* 这是一个公有的比较方法，参数是StringBuffer类型
	* 实际调用的是contentEquals(CharSequence cs)方法，可以说是StringBuffer的特供版
	*/
public boolean contentEquals(StringBuffer sb) {
    return contentEquals((CharSequence)sb);
}

/**
	* 这是一个常用于String对象跟StringBuffer和StringBuilder比较的方法
	* 参数是StringBuffer或StringBuilder或String或CharSequence
	* StringBuffer和StringBuilder和String都实现了CharSequence接口
	*/
public boolean contentEquals(CharSequence cs) {
    // Argument is a StringBuffer, StringBuilder
    if (cs instanceof AbstractStringBuilder) {
        if (cs instanceof StringBuffer) {
            synchronized(cs) {
                // 方法算法和CharSequence一样
                return nonSyncContentEquals((AbstractStringBuilder)cs);
            }
        } else {                                 
            // 如果是StringBuilder类型，则进入非同步块
            return nonSyncContentEquals((AbstractStringBuilder)cs);
        }
    }

    /***下面就是String和CharSequence类型的比较算法*****/
    // Argument is a String
    if (cs instanceof String) {                    
        return equals(cs);
    }
    // Argument is a generic CharSequence
    char v1[] = value;
    int n = v1.length;
    if (n != cs.length()) {
        return false;
    }
    for (int i = 0; i < n; i++) {
        if (v1[i] != cs.charAt(i)) {
            return false;
        }
    }
    return true;
}
```

##### compareTo 类方法 和 CaseInsensitiveComparator静态内部类

```java
// 正数为大，负数为小，是基于字符的ASSIC码比较的
public int compareTo(String anotherString) {
  int len1 = value.length;
  int len2 = anotherString.value.length;
  int lim = Math.min(len1, len2);      
  char v1[] = value;                       
  char v2[] = anotherString.value; 

  int k = 0;
  while (k < lim) {                         
    char c1 = v1[k];
    char c2 = v2[k];
    // 从前向后遍历，只要其中一个不相等，返回字符ASSIC的差值
    if (c1 != c2) {
      return c1 - c2;
    }
    k++;
  }
   // 如果两个字符串同样位置的索引都相等，返回长度差值，完全相等则为0
  return len1 - len2;
}

// 可以用来不区分大小写的比较大小
public int compareToIgnoreCase(String str) {
  return CASE_INSENSITIVE_ORDER.compare(this, str);
  // static内部类 的实现算法
  public int compare(String s1, String s2) {
      int n1 = s1.length();
      int n2 = s2.length();
      int min = Math.min(n1, n2);
      for (int i = 0; i < min; i++) {
          char c1 = s1.charAt(i);
          char c2 = s2.charAt(i);
          if (c1 != c2) {
              c1 = Character.toUpperCase(c1);
              c2 = Character.toUpperCase(c2);
              if (c1 != c2) {
                  c1 = Character.toLowerCase(c1);
                  c2 = Character.toLowerCase(c2);
                  if (c1 != c2) {
                      // No overflow because of numeric promotion
                      return c1 - c2;
                  }
              }
          }
      }
      return n1 - n2;
    }
}

/**
* 这是一个饿汉单例模式，是String类型的一个不区分大小写的比较器
* 提供给Collections和Arrays的sort方法使用
* 例如：Arrays.sort(strs,String.CASE_INSENSITIVE_ORDER);
* 效果就是会将strs字符串数组中的字符串对象进行忽视大小写的排序
*/
public static final Comparator<String> CASE_INSENSITIVE_ORDER
																					= new CaseInsensitiveComparator();
```

##### regionMatchs() 方法：片段比较

```java
/**
	* 这是一个类似于equals的方法，比较的是字符串的片段，也即是部分区域的比较
	* toffset是当前字符串的比较起始位置(偏移量),other是要比较的String对象参数，ooffset是要参数String的比较片段起始位置，len是两个字符串要比较的片段的长度大小
	* 
	* 例子：String str1 = "0123456",Str2 = "0123456789"; 
	* str1.regionMatchs(0,str2,0,6);意思是str1从0位置开始于str2的0位置开始 比较6个长度 的字符串片段
	* 相等则返回 true,不等返回false 
	*/
public boolean regionMatches(int toffset, String other, int ooffset, int len) {
    char ta[] = value;
    int to = toffset;
    char pa[] = other.value;
    int po = ooffset;
    if ((ooffset < 0) || (toffset < 0)
        || (toffset > (long)value.length - len)
        || (ooffset > (long)other.value.length - len)) {
        return false;
    }
    while (len-- > 0) { 
        if (ta[to++] != pa[po++]) {
            return false;
        }
    }
    return true;
}

// 只不过多了一个参数，既ignoreCase，既是否为区分大小写。	
public boolean regionMatches(boolean ignoreCase, int toffset,
                             String other, int ooffset, int len) {
    char ta[] = value;
    int to = toffset;
    char pa[] = other.value;
    int po = ooffset;
    if ((ooffset < 0) || (toffset < 0)
        || (toffset > (long)value.length - len)
        || (ooffset > (long)other.value.length - len)) {
        return false;
    }
    while (len-- > 0) {
        char c1 = ta[to++];
        char c2 = pa[po++];
        if (c1 == c2) {
            continue;
        }
        if (ignoreCase) {
            char u1 = Character.toUpperCase(c1);
            char u2 = Character.toUpperCase(c2);
            if (u1 == u2) {
                continue;
            }
            if (Character.toLowerCase(u1) == Character.toLowerCase(u2)) {
                continue;
            }
        }
        return false;
    }
    return true;
}
```

##### startsWith、endsWith 类方法

```java
// 是否以prefix开始
// prefix是需要判断的前缀字符串，toffset是当前对象的判断起始位置
public boolean startsWith(String prefix, int toffset) {
  char ta[] = value;
  int to = toffset;
  char pa[] = prefix.value;
  int po = 0;
  int pc = prefix.value.length;
  if ((toffset < 0) || (toffset > value.length - pc)) {
    return false;
  }
  // 循环pc次，既prefix的长度
  while (--pc >= 0) {                  
    if (ta[to++] != pa[po++]) {
      return false; 
    }
  }
  return true;
}

// 判断当前字符串对象是否以字符串prefix起头
public boolean startsWith(String prefix) {
  return startsWith(prefix, 0);
}

// 判断当前字符串对象是否以字符串suffix结尾
public boolean endsWith(String suffix) {
  return startsWith(suffix, value.length - suffix.value.length); 
}
```

##### indexOf、lastIndexOf 类方法

```java
// 返回cn对应的字符在字符串中第一次出现的位置，从字符串的索引0位置开始遍历
public int indexOf(int ch) {
    return indexOf(ch, 0);
}

// 既从fromIndex位置开始查找，从头向尾遍历，ch整数对应的字符在字符串中第一次出现的位置
// -1代表字符串没有这个字符，整数代表字符第一次出现在字符串的位置
public int indexOf(int ch, int fromIndex) {
    final int max = value.length;
    if (fromIndex < 0) {
        fromIndex = 0;
    } else if (fromIndex >= max) {
        return -1;
    }

    if (ch < Character.MIN_SUPPLEMENTARY_CODE_POINT) {
        final char[] value = this.value;
        // 从fromIndex位置开始向后遍历
        for (int i = fromIndex; i < max; i++) {
            if (value[i] == ch) {                
                return i; 
            }
        }
        return -1;
    } else {
        return indexOfSupplementary(ch, fromIndex);
    }
}

// 返回cn对应的字符在字符串中最后出现的位置
public int lastIndexOf(int ch) {
    return lastIndexOf(ch, value.length - 1);
}

// 从尾部向头部遍历，从fromIndex开始作为起点，返回ch对应字符第一次在字符串出现的位置
public int lastIndexOf(int ch, int fromIndex) {
    if (ch < Character.MIN_SUPPLEMENTARY_CODE_POINT) {
        final char[] value = this.value;
        int i = Math.min(fromIndex, value.length - 1);   
        // 从后向前遍历，直到i<0,退出循环
        for (; i >= 0; i--) {      
            if (value[i] == ch) {
                return i;
            }
        }
        return -1;
    } else {
        return lastIndexOfSupplementary(ch, fromIndex);
    }
}

// 返回第一次出现的字符串的位置
public int indexOf(String str) {
    return indexOf(str, 0);
}

// 从fromIndex开始遍历，返回第一次出现str字符串的位置
public int indexOf(String str, int fromIndex) {
    return indexOf(value, 0, value.length,
                   str.value, 0, str.value.length, fromIndex);
}

// 查找字符串Str最后一次出现的位置
public int lastIndexOf(String str) {
    return lastIndexOf(str, value.length);
}

// 从fromIndex开始遍历，返回最后一次出现str字符串的位置
public int lastIndexOf(String str, int fromIndex) {
    return lastIndexOf(value, 0, value.length,
                       str.value, 0, str.value.length, fromIndex);
}
```

##### substring() 方法

```java
// 截取当前字符串，从beginIndex 到完
public String substring(int beginIndex) {
    if (beginIndex < 0) {
        throw new StringIndexOutOfBoundsException(beginIndex);
    }
    int subLen = value.length - beginIndex;
    if (subLen < 0) {
        throw new StringIndexOutOfBoundsException(subLen);
    }
    return (beginIndex == 0) ? this : new String(value, beginIndex, subLen);
}

// 截取一个区间范围[beginIndex,endIndex)
public String substring(int beginIndex, int endIndex) {
    if (beginIndex < 0) {
        throw new StringIndexOutOfBoundsException(beginIndex);
    }
    if (endIndex > value.length) {
        throw new StringIndexOutOfBoundsException(endIndex);
    }
    int subLen = endIndex - beginIndex;
    if (subLen < 0) {
        throw new StringIndexOutOfBoundsException(subLen);
    }
    return ((beginIndex == 0) && (endIndex == value.length)) 
        ? this
        : new String(value, beginIndex, subLen);
}
```

##### concat() 方法

```java
// String 的拼接函数
public String concat(String str) {
    int otherLen = str.length();
    if (otherLen == 0) {
        return this;
    }
    int len = value.length;
    // 将数组扩容，将value数组拷贝到buf数组中，长度为len + str.lengh
    char buf[] = Arrays.copyOf(value, len + otherLen); 
    // 将str字符串从buf字符数组的len位置开始覆盖，得到一个完整的buf字符数
    str.getChars(buf, len);组
        return new String(buf, true);
}
```

##### replace、replacesAll 类方法

```java
// 替换，将字符串中的oldChar字符全部替换成newChar
public String replace(char var1, char var2) {
    if (var1 != var2) {
        int var3 = this.value.length;
        int var4 = -1;
        char[] var5 = this.value;
        // 看不懂 感觉这一步的作用不到
        do {
            ++var4;
        } while(var4 < var3 && var5[var4] != var1);

        if (var4 < var3) {
            char[] var6 = new char[var3];

            for (int var7 = 0; var7 < var4; ++var7) {
                var6[var7] = var5[var7];
            }
            // 全部替换
            while(var4 < var3) {
                char var8 = var5[var4];
                var6[var4] = var8 == var1 ? var2 : var8;
                ++var4;
            }

            return new String(var6, true);
        }
    }

    return this;
}

// 替换第一个旧字符
public String replaceFirst(String var1, String var2) {
    return Pattern.compile(var1).matcher(this).replaceFirst(var2);
}

// 当不是正规表达式时，与replace效果一样，都是全体换。如果字符串的正则表达式，则规矩表达式全体替换
public String replaceAll(String regex, String replacement) {
    return Pattern.compile(regex).matcher(this).replaceAll(replacement);
}

// 用旧字符串替换新字符串
public String replace(CharSequence target, CharSequence replacement) {
    return Pattern.compile(target.toString(), Pattern.LITERAL).matcher(
        this).replaceAll(Matcher.quoteReplacement(replacement.toString()));
}
```

##### matches()和contains() 方法

```java
// matches() 方法用于检测字符串是否匹配给定的正则表达式。
public boolean matches(String regex) {
  return Pattern.matches(regex, this);
}

// 是否含有CharSequence这个子串
public boolean contains(CharSequence s) {
  return indexOf(s.toString()) > -1;
}
```

##### format() 方法

```java
// 新字符串使用本地语言环境，制定字符串格式和参数生成格式化的新字符串
public static String format(String format, Object... args) {
  return new Formatter().format(format, args).toString();
}

// 使用指定的语言环境，制定字符串格式和参数生成格式化的字符串
public static String format(Locale l, String format, Object... args) {
  return new Formatter(l).format(format, args).toString();
}
```

##### valueOf 方法

```java
// 将Object转换为String
public static String valueOf(Object obj) {
  return (obj == null) ? "null" : obj.toString();
}
```

##### intern() 方法

```java
// 去字符串常量池中寻找str字符串，如果有则返回str在常量池中的引用，如果没有则在常量池中创建str对象
public native String intern();
```

