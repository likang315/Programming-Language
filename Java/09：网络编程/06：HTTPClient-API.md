### HTTPClient API

------

[TOC]

##### 01：org.apache.http.config.SocketConfig

- 套接字配置：

- ```java
  SocketConfig socketConfig = SocketConfig.custom()
       // 是否立即发送数据，设置为true会关闭Socket缓冲，默认为false
      .setTcpNoDelay(true)
       // 是否可以在一个进程关闭Socket后，即使它还没有释放端口，其它进程还可以立即重用端口
  	.setSoReuseAddress(true) 
       // 接收数据的等待超时时间，单位ms
      .setSoTimeout(5000)
       // 关闭Socket时，要么发送完所有数据，要么等待60s后，就关闭连接，此时socket.close()是阻塞的
      .setSoLinger(60)
       // 开启监视TCP连接是否有效
      .setSoKeepAlive(true).build();
  ```

##### 02：org.apache.http.config.ConnectionConfig

- Http connection相关配置，一般不修改该配置；
- 用于设置编码格式和消息的约束条件；

##### 0：org.apache.http.impl.conn.PoolingHttpClientConnectionManager

- 连接池：管理客户端连接，并且能够为来自多个执行线程的连接请求提供服务；

###### 方法

- setMaxTotal
  - 设置最大连接数
- setDefaultMaxPerRoute
  - 默认每个路由的最大连接数
- closeIdleConnections(long idleTimeout, TimeUnit timeUnit)
  - 连接驱除策略，关闭超多idleTime 时间不活动的连接。
- setDefaultSocketConfig(SocketConfig defaultSocketConfig)
  - 设置socket 配置；
- setDefaultConnectionConfig(final ConnectionConfig defaultConnectionConfig)
  - 设置HTTP conn配置；

##### 04：org.apache.http.impl.client.CloseableHttpClient

- HTTPClient 的实现类，用与发送HTTP请求；

###### 方法

- execute(final HttpUriRequest request) throws IOException, ClientProtocolException
  - 执行请求；
- execute(final HttpUriRequest request, final ResponseHandler<? extends T> responseHandler) throws IOException,
          ClientProtocolException
  - 执行清楚，根据响应处理器处理；

##### 05：org.apache.http.client.config.RequestConfig

- HTTP 请求配置

###### 方法

- ```java
  RequestConfig requestConfig = RequestConfig.custom()
      // 设置从connectManager获取Connection 超时时间
      .setConnectionRequestTimeout(1000)
      // 设置建立连接超时时间
      .setConnectTimeout(10000)
      // 请求获取数据的超时时间
      .setSocketTimeout(10000)
      // 确定是否应自动处理身份验证
      .setAuthenticationEnabled(true)
      // 确定循环重定向(重定向到相同位置)是否应该重定向
      .setCircularRedirectsAllowed(false)
      // 重定向的最大数目。对重定向次数的限制是为了防止无限循环
      .setMaxRedirects(3)
      // 确定是否应自动处理重定向
      .setRedirectsEnabled(true)
      // 确定是否应拒绝相对重定向。HTTP规范要求位置值是一个绝对URI
      .setRelativeRedirectsAllowed(true)
      // 确定是否应自动解压缩压缩实体，默认为true
      .setContentCompressionEnabled(true)
      // 确定用于HTTP状态管理的cookie规范的名称
      .setCookieSpec("")
      // 返回用于请求执行的本地地址。在具有多个网络接口的计算机上，此参数可用于选择其中的网络接口连接产生。
      .setLocalAddress()
      // 代理配置
      .setProxy()
      // 在使用代理主机进行身份验证时，确定支持的身份验证方案的优先顺序。
      .setProxyPreferredAuthSchemes()
      // 在使用目标主机进行身份验证时，确定受支持的身份验证模式的首选项顺序
      .setTargetPreferredAuthSchemes()
      .build();
  ```

  

