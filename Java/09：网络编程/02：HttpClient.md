### HTTPClient 

------

[TOC]

##### 01：概述

- 尽管 java.net 包提供了通过HTTP访问资源的基本功能，但它并未提供许多应用程序所需的完全灵活性或功能。HttpClient力求通过**提供一个高效的、最新的、功能丰富的且实现最新HTTP标准和建议的客户端的包**来填补这一空白。
- HttpClient 模拟浏览器的客户端，它是一个客户端HTTP传输库。HttpClient的目的是**传输和接收 HTTP 消息**。HttpClient不会尝试处理内容、执行嵌入在HTML页面中的JS、尝试猜测内容类型(如果没有显式设置)或重新格式化请求/重写位置uri，或其他与HTTP传输无关的功能。

##### 02：执行请求

- HttpClient 最重要的功能是执行HTTP方法。 执行HTTP方法涉及一个或多个HTTP请求/ HTTP响应交换，通常由 HttpClient 内部处理。 **期望用户提供要执行的请求对象，并且 HttpClient 期望将请求发送到目标服务器返回相应的响应对象，或者如果执行不成功则抛出异常**。

###### 示例

- ```java
  CloseableHttpClient httpclient = HttpClients.createDefault();
  HttpGet httpget = new HttpGet("http://localhost/");
  CloseableHttpResponse response = httpclient.execute(httpget);
  try {
      <...>
  } finally {
      response.close();
  }
  ```

##### 03：HTTP请求

- HttpClient 支持开箱即用的HTTP / 1.1 规范中定义的所有HTTP方法：GET，HEAD，POST，PUT，DELETE，TRACE和OPTIONS。 每种方法类型都有一个特定的类：**HttpGet，HttpHead，HttpPost，HttpPut，HttpDelete，HttpTrace和HttpOptions**。

- HttpClient提供了**URIBuilder实用程序类**，以简化请求URI的创建和修改。

  ```java
  URI uri = new URIBuilder()
          .setScheme("http")
          .setHost("www.google.com")
          .setPath("/search")
          .setParameter("q", "httpclient")
          .setParameter("btnG", "Google Search")
          .setParameter("aq", "f")
          .setParameter("oq", "")
          .build();
  HttpGet httpget = new HttpGet(uri);
  // http://www.google.com/search?q=httpclient&btnG=Google+Search&aq=f&oq=
  System.out.println(httpget.getURI());
  ```

##### 04：HTTP响应

- ```java
  HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1, 
  HttpStatus.SC_OK, "OK");
  
  System.out.println(response.getProtocolVersion());
  System.out.println(response.getStatusLine().getStatusCode());
  System.out.println(response.getStatusLine().getReasonPhrase());
  System.out.println(response.getStatusLine().toString());
  ```

##### 05：HTTP实体

- 由于实体可以表示二进制和字符内容，因此它**支持字符编码**（支持后者的，即字符内容）。
- 要从实体读取内容，可以通过**HttpEntity#getContent()方法检索输入流**，该方法返回java.io.InputStream，或者可以向**HttpEntity#writeTo(OutputStream)方法提供输出流**， 一旦所有内容都写入给定流，它将返回。

######  消费实体内容

- 为了确保正确释放系统资源，必须关闭与实体关联的内容流或响应本身；

- 关闭内容流和关闭响应之间的区别在于**前者将尝试通过使用实体内容来保持底层连接处于活动状态，而后者立即关闭并丢弃连接。**

  - ```java
    CloseableHttpClient httpclient = HttpClients.createDefault();
    HttpGet httpget = new HttpGet("http://localhost/");
    CloseableHttpResponse response = httpclient.execute(httpget);
    try {
        HttpEntity entity = response.getEntity();
        if (entity != null) {
            InputStream instream = entity.getContent();
            try {
                // do something useful
            } finally {
                instream.close();
            }
        }
    } finally {
        response.close();
    }
    ```

- 在某些情况下，可能需要能够多次读取实体内容。 在这种情况下，实体内容必须以某种方式缓冲，无论是在内存中还是在磁盘上。 实现这一目标的最简单方法是使用BufferedHttpEntity类包装原始实体

  - ```java
    CloseableHttpResponse response = <...>
    HttpEntity entity = response.getEntity();
    if (entity != null) {
        entity = new BufferedHttpEntity(entity);
    }
    ```

###### 创建实体内容

- HttpClient为大多数常见数据容器提供了几个类，如字符串，表单，字节数组，输入流和文件：StringEntity，UrlEncodedFormEntity，ByteArrayEntity，InputStreamEntity和FileEntity。

  - ```java
    // 提交文件
    File file = new File("somefile.txt");
    FileEntity entity = new FileEntity(file, 
        ContentType.create("text/plain", "UTF-8"));        
    HttpPost httppost = new HttpPost("http://localhost/action.do");
    httppost.setEntity(entity);
    // 表单提交
    List<NameValuePair> formparams = new ArrayList<NameValuePair>();
    formparams.add(new BasicNameValuePair("param1", "value1"));
    formparams.add(new BasicNameValuePair("param2", "value2"));
    UrlEncodedFormEntity entity = new UrlEncodedFormEntity(formparams, Consts.UTF_8);
    HttpPost httppost = new HttpPost("http://localhost/handler.do");
    httppost.setEntity(entity);
    // POST-Json 提交，
    StringEntity entity = new StringEntity("important message",
    ContentType.create("plain/text", Consts.UTF_8));
    // 设置为true，可以通知HttpClient优先使用块编码
    entity.setChunked(true); 
    HttpPost httppost = new HttpPost("http://localhost/acrtion.do");
    httppost.setEntity(entity);
    ```

##### 06：响应处理

- 处理响应的最简单和最方便的方法是**使用 ResponseHandler 接口，该接口包含 handleResponse(HttpResponse response)方法**。 该方法使用户完全**不必担心连接管理**。 使用 ResponseHandler 时，无论请求执行成功还是导致异常，HttpClient都会**自动确保将连接释放回连接管理器**。

- ```java
  CloseableHttpClient httpclient = HttpClients.createDefault();
  HttpGet httpget = new HttpGet("http://localhost/json");
  
  ResponseHandler<MyJsonObject> rh = new ResponseHandler<MyJsonObject>() {
  
      @Override
      public JsonObject handleResponse(
              final HttpResponse response) throws IOException {
          StatusLine statusLine = response.getStatusLine();
          HttpEntity entity = response.getEntity();
          if (statusLine.getStatusCode() >= 300) {
              throw new HttpResponseException(
                      statusLine.getStatusCode(),
                      statusLine.getReasonPhrase());
          }
          if (entity == null) {
              throw new ClientProtocolException("Response contains no content");
          }
          Gson gson = new GsonBuilder().create();
          ContentType contentType = ContentType.getOrDefault(entity);
          Charset charset = contentType.getCharset();
          Reader reader = new InputStreamReader(entity.getContent(), charset);
          return gson.fromJson(reader, MyJsonObject.class);
      }
  };
  MyJsonObject myjson = client.execute(httpget, rh);
  ```

##### 07：HTTPClient 接口

- HTTPClient 是线程安全的，建议将此类的**同一实例重用于多个请求**执行。
- **资源释放：**当不再需要实例 CloseableHttpClient 并且即将超出使用范围时，必须通过调用**CloseableHttpClient#close()**方法关闭与其关联的连接管理器。

###### HTTP 执行上下文

- HttpClient允许在特定的执行上下文中执行HTTP请求，称为HTTP上下文(HttpContext)；

- 在HTTP请求执行过程中，HttpClient将以下属性添加到执行上下文中：

  - HttpConnection 实例表示与目标服务器的实际连接。
  - HttpHost：实例表示连接的目标。
  - HttpRoute：实例表示完整的连接路由
  - HttpRequest：实例表示实际HTTP请求。 
    - 执行上下文中的最终HttpRequest对象始终表示**消息的状态它与发送到目标服务器的状态完全相同**。 默认HTTP/1.0和HTTP/1.1使用相对请求URI。 但是，如果请求是通过代理以非隧道模式发送的，那么URI将是绝对的。
  - HttpResponse：实例表示实际的HTTP响应。
  - RequestConfig：实例表示实际的请求配置。
  - java.util.List <URI>对象：表示在请求执行过程中收到的**所有重定向位置的集合**。

- 示例

  - ```java
    // 在以下示例中，初始请求设置的请求配置将保留在执行上下文中，并传播到共享相同上下文的连续请求。
    CloseableHttpClient httpclient = HttpClients.createDefault();
    RequestConfig requestConfig = RequestConfig.custom()
            .setSocketTimeout(1000)
            .setConnectTimeout(1000)
            .build();
    HttpGet httpget1 = new HttpGet("http://localhost/1");
    httpget1.setConfig(requestConfig);
    CloseableHttpResponse response1 = httpclient.execute(httpget1, context);
    try {
        HttpEntity entity1 = response1.getEntity();
    } finally {
        response1.close();
    }
    
    HttpGet httpget2 = new HttpGet("http://localhost/2");
    CloseableHttpResponse response2 = httpclient.execute(httpget2, context);
    try {
        HttpEntity entity2 = response2.getEntity();
    } finally {
        response2.close();
    }
    ```

##### 08：HTTP 协议拦截器

- 通常期望协议拦截器作用于**传入消息的一个特定报头或一组相关报头，或者用一个特定报头或一组相关报头填充传出消息**。

- ```java
  CloseableHttpClient httpclient = HttpClients.custom()
      .addInterceptorLast(new HttpRequestInterceptor() {
          public void process(final HttpRequest request, final HttpContext context)
              throws HttpException, IOException {
              AtomicInteger count = (AtomicInteger) context.getAttribute("count");
              request.addHeader("Count", Integer.toString(count.getAndIncrement()));
          }
      }).build();
  ```

##### 09：异常处理

- HTTP 协议处理器可以抛出两种类型的异常：
  - java.io.IOException：在I/O失败的情况下抛出，例如套接字超时或套接字重置，非致命的，可恢复的；
  - HttpException：它表示HTTP故障，例如违反HTTP协议，致命的，无法自动恢复；
    - 将 HttpExceptions 重新抛出为 **ClientProtocolException**，它是java.io.IOException的子类。 这使HttpClient的用户能够**从单个catch子句处理I/O错误和协议违规**。

###### HTTP 传输安全

-  HTTP是一种简单的面向请求/响应的协议，最初设计用于支持静态或动态生成的内容检索。它**从未打算支持交易操作**。例如，如果HTTP服务器成功接收并处理请求，生成响应并将状态代码发送回客户端，则HTTP服务器将考虑约定的一部分。
- 如果客户端由于读取超时，请求取消或系统崩溃而未能完全接收响应，则服务器将不会尝试回滚事务。如果客户端决定重试相同的请求，则服务器将不可避免地多次执行同一事务。在某些情况下，这可能会导致应用程序数据损坏或应用程序状态不一致。
- 尽管HTTP从未被设计为支持事务处理，但**只要满足某些条件，它仍可用作关键任务应用程序的传输协议**。为确保HTTP传输层安全，系统**必须确保应用层上HTTP方法的幂等性**。

###### 自动异常恢复

- 默认情况下，HttpClient会尝试从 IO 异常中自动恢复。 默认的自动恢复机制仅限于一些已知安全的异常情况。
  - HttpClient **不会尝试从任何逻辑或HTTP协议错误（从HttpException类派生的错误）中恢复。**
  - HttpClient将自动重试那些被认为是幂等的方法。
  - 当HTTP请求仍在传输到目标服务器时（即请求尚未完全传输到服务器），HttpClient将**自动重试那些因传输异常而失败的方法**。

###### 请求重试处理程序

- 为了启用**自定义异常恢复机制**，应提供**HttpRequestRetryHandler**接口的实现；

- ```java
  HttpRequestRetryHandler myRetryHandler = new HttpRequestRetryHandler() {
      public boolean retryRequest(IOException exception, int executionCount, HttpContext context) {
          if (executionCount >= 5) {
              // Do not retry if over max retry count
              return false;
          }
          if (exception instanceof InterruptedIOException) {
              // Timeout
              return false;
          }
          if (exception instanceof UnknownHostException) {
              // Unknown host
              return false;
          }
          if (exception instanceof ConnectTimeoutException) {
              // Connection refused
              return false;
          }
          if (exception instanceof SSLException) {
              // SSL handshake exception
              return false;
          }
          HttpClientContext clientContext = HttpClientContext.adapt(context);
          HttpRequest request = clientContext.getRequest();
          boolean idempotent = !(request instanceof HttpEntityEnclosingRequest);
          if (idempotent) {
              // Retry if the request is considered idempotent
              return true;
          }
          return false;
      }
  
  };
  CloseableHttpClient httpclient = HttpClients.custom()
      .setRetryHandler(myRetryHandler)
      .build();
  ```

##### 10：中断请求

- 在某些情况下，由于**目标服务器上的高负载或客户端发出的并发请求太多**，HTTP请求执行无法在预期的时间范围内完成。 在这种情况下，可能需要**提前终止请求**并**解除阻塞I/O操作中阻止的执行线程**。 通过调用 **HttpUriRequest＃abort（）**方法，可以在执行的任何阶段中止由HttpClient执行的HTTP请求。 此方法是线程安全的，可以从任何线程调用。

##### 11：重定向处理程序

- HttpClient**自动处理所有类型的重定向**，除了HTTP规范明确禁止的需要用户干预的重定向。 

- HttpClient通常必须**在执行过程中重写请求消息**。 默认情况下，HTTP/1.0和HTTP/1.1通常使用相对请求URI。 可以**使用原始请求和上下文构建最终绝对HTTP位置**。实用程序 **URIUtils#resolution 可构建用于生成最终请求的绝对URI**。

  ```java
  CloseableHttpClient httpclient = HttpClients.createDefault();
  HttpClientContext context = HttpClientContext.create();
  HttpGet httpget = new HttpGet("http://localhost:8080/");
  CloseableHttpResponse response = httpclient.execute(httpget, context);
  try {
      HttpHost target = context.getTargetHost();
      List<URI> redirectLocations = context.getRedirectLocations();
      URI location = URIUtils.resolve(httpget.getURI(), target, redirectLocations);
      System.out.println("Final HTTP location: " + location.toASCIIString());
  } finally {
      response.close();
  }
  ```



