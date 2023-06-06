### IO、NIO

------

[TOC]


##### 0：BIO、NIO 和 AIO 的区别

- Java BIO ： 同步阻塞型，客户端有连接请求时服务器端（必须先启动一个线程等待）就需要启动一个线程进行处理，此时连接不能做任何事情；
- Java NIO ： 同步非阻塞型，客户端发送的连接请求都会**注册到多路复用器**上，多路复用器**轮询到连接有I/O请求时才启动一个线程进行处理**
- Java AIO ： 异步非阻塞型，服务器实现模式为一个有效请求一个线程，**客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理**

##### 02：缓冲区（Buffer）

​	缓冲区本质是一个容器对象，其实就是**一个字节数组**，在NIO库中，所有数据都是用缓冲区处理的。在读取数据时，它是直接从缓冲区中读； 在写入数据时，它是直接写入到缓冲区中的；**任何时候访问 NIO 中的数据，都是操作缓冲区的**；

###### 在NIO中，所有的缓冲区类型都继承于抽象类 Buffer，非线程安全的类

- ByteBuffer，ShortBuffer，IntBuffer，LongBuffer
- FloatBuffer，DoubleBuffer
- CharBuffer
- Boolean 没有对应的 Buffer

```java
// 缓冲的实现
public abstract class Buffer {
	  // 0 <= mark <= position <= limit <= capacity 
    // 标记：一个备忘地址，作为临时标记位置使用，标记在设定前是未定义的
    private int mark = -1;
		// 位置：下一个将要被读或写的元素的索引，一定小于limit， 值由get()和put()方法的调用来更新
    private int position = 0;
		// 上界：指第一个不能再读也不能再写的元素索引. 最大值，并且一定是小于等于capacity的
    private int limit;
    // 容量：缓冲区能够容纳的数据元素的最大数量，容量在缓冲区创建时被设定，永远不能改变
    private int capacity;

  	// 将 Buffer 内部状态置为原始状态
  	public final Buffer clear() {
        position = 0;
        limit = capacity;
        mark = -1;
        return this;
    }
  	// 反转，使buffer从写模式转换到读模式
  	public final Buffer flip() {
        limit = position;
        position = 0;
        mark = -1;
        return this;
    }
  	// 标记备忘地址
  	public final Buffer mark() {
        mark = position;
        return this;
    }
  	// 恢复到备忘地址
    public final Buffer reset() {
        int m = mark;
        if (m < 0)
            throw new InvalidMarkException();
        position = m;
        return this;
    }
}
```

###### ByteBuffer

```java
public abstract class ByteBuffer extends Buffer implements Comparable<ByteBuffer>{
    // 相对存入/取出，即 position位置自动前进, 超出limit 也会抛出异常
    public abstract byte get();
    // 绝对存入/取出，位置是通过参数指定, 绝对操作不影响position位置，但是如果索引位置超出limit，则会抛出IndexOutOfBoundsException；
    public abstract byte get(int index);
    public abstract ByteBuffer put(byte b);
}
```

##### 0：Channel 机制（通道）

- Channel 是数据源和数据归宿之间通道的抽象，通过通道把数据读入或者到 Buffer
- 在NIO中，提供了多种通道对象，而**所有的通道对象都实现了Channel接口**

```java
public interface Channel extends Closeable {
    // 打开通道
    public boolean isOpen();
	  // 关闭通道
    public void close() throws IOException;
}
```

###### 无论读写操作都必须先获得 Channel 通过通道再去读写数据 

1. 从 FileInputStream 获取 Channel
2. 创建 Buffer 缓存
3. 将数据从 Channel 读取到 Buffer中 或者 数据不是直接写入通道，而是写入缓冲区 

```java
public class FileInputProgram {  
    public static void main( String args[] ) throws Exception{  
        FileInputStream fin = new FileInputStream("c:\\test.txt");  
        // 获取通道
        FileChannel fc = fin.getChannel();
        // 创建缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);  
        // 读取数据到缓冲区
        fc.read(buffer);
        // 将写模式转换成读模式
        buffer.flip();  
        while (buffer.remaining() > 0) { 
            byte b = buffer.get();  
            System.out.print(((char)b));  
        }  
        fin.close();
    }  
}
```

###### NIO 与 IO相比而言，Channel 就如同 Stream，使用 Channel 读取对象

- Channel 与 Stream 的区别在于：**Channel 是双向的，而Stream只能是单向的**。Channel在被打开之后，即可以读，也可以写，或者同时进行读写操作。 因为Channel是双向的，因此它比Stream更好的反应了底层操作系统IO的实质；

###### Channel 的映射文件机制

- Channel 需要将文件映射到Buffer中，应用程序再从 Buffer 中读取数据，就相当于从文件中读数据；
- 内存映射文件它虽然最终也是要从磁盘读取数据，但是它并不需要将数据读取到 OS 内核缓冲区，而是直接**将进程的用户私有地址空间中的一部分区域与文件对象建立起映射关系**，就好像直接从内存中读、写文件一样，速度当然快；
