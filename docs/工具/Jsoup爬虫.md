# Jsoup爬虫

# HTTP请求示例

## 使用GET带参数请求

```java
public static void main(String[] args) throws Exception {
    // 创建HttpClient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    // 设置请求地址是: http://yun.itheima.com/search?keys=Java
    // 创建URIBuilder
    URIBuilder uriBuilder = new URIBuilder("http://yun.itheima.com/search");
    // 设置参数
    uriBuilder.setParameter("keys", "Java");
    // 创建HttpGet对象，设置url访问地址
    HttpGet httpGet = new HttpGet(uriBuilder.build());
    System.out.println("发起请求的信息: " + httpGet);
}
```

## 使用POST带参数请求

```java
public static void main(String[] args) throws Exception {
    // 创建HttpClient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    // 创建HttpPost对象，设置url访问地址
    HttpPost httpPost = new HttpPost("http://yun.itheima.com/search");
    // 声明List集合，封装表单中的参数
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    // 设置请求地址是:http://yun.itheima.com/search?keys=Java
    params.add(new BasicNameValuePair("keys", "Java"));
    // 创建表单的Entity对象, 第一个参数就是封装好的表单数据，第二个参数就是编码
    UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(params, "utf-8");
    // 设置表单的Entity对象到Post请求中
    httpPost.setEntity(formEntity);
    CloseableHttpResponse response = null;
    try {
        // 使用httpClient发起请求，获取response
        response = httpClient.execute(httpPost);
        // 解析响应
        if (response.getStatusLine().getStatusCode() == 200) {
            String content = EntityUtils.toString(response.getEntity(), "utf-8");
            System.out.println(content.length());
        }
    } finally {
        // 关闭响应
        if (response != null) {
            response.close();
        }
    }
}
```

请注意，上述代码中的`URIBuilder`和`HttpPost`对象的创建方式可能需要根据您使用的HttpClient库的版本进行调整。此外，确保在实际应用中正确处理异常和资源释放。

# 设置连接池

```java
public static void main(String[] args) {
    // 创建连接池管理器
    PoolingHttpClientConnectionManager cm = new PoolingHttpClientConnectionManager();

    // 设置最大连接数
    cm.setMaxTotal(100);

    // 设置每个主机的最大连接数
    cm.setDefaultMaxPerRoute(10);

    // 使用连接池管理器发起请求
    doGet(cm);
    doPost(cm);
}
```

## 发起GET请求

```java
private static void doGet(PoolingHttpClientConnectionManager cm) {
    // 不是每次创建新的HttpClient，而是从连接池中获取HttpClient对象
    CloseableHttpClient httpClient = HttpClients.custom().setConnectionManager(cm).build();

    HttpGet httpGet = new HttpGet("http://www.itcast.cn");
    CloseableHttpResponse response = null;
    try {
        response = httpClient.execute(httpGet);
        if (response.getStatusLine().getStatusCode() == 200) {
            String content = EntityUtils.toString(response.getEntity(), "utf-8");
            System.out.println(content.length());
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (response != null) {
            try {
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        // 不能关闭httpClient，由连接池管理HttpClient
        // httpClient.close();
    }
}
```

请确保在实际应用中正确处理异常和资源释放。上述代码中的`HttpClients.custom().setConnectionManager(cm).build()`用于创建一个可从连接池获取连接的`CloseableHttpClient`实例。在`finally`块中，我们不关闭`httpClient`，因为连接池管理器会负责管理这些连接。

# 设置网络连接配置

```java
// 创建HttpGet对象，设置URL访问地址
HttpGet httpGet = new HttpGet("http://www.itcast.cn");

// 配置请求信息
RequestConfig config = RequestConfig.custom()
    .setConnectTimeout(1000) // 创建连接的最长时间，单位是毫秒
    .setConnectionRequestTimeout(500) // 设置获取连接的最长时间，单位是毫秒
    .setSocketTimeout(10 * 1000) // 设置数据传输的最长时间，单位是毫秒
    .build();

// 给请求设置请求信息
httpGet.setConfig(config);

CloseableHttpResponse response = null;
try {
    // 使用httpClient发起请求，获取response
    response = httpClient.execute(httpGet);
    // 处理响应
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (response != null) {
        try {
            response.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    // httpClient的关闭应该在所有请求完成后进行
    // httpClient.close();
}
```

## 说明

在上述代码中，我们首先创建了一个`HttpGet`对象，并设置了要访问的URL。接着，我们通过`RequestConfig.custom()`方法来配置请求的超时设置，包括连接创建超时、获取连接超时以及数据传输超时。这些配置对于在网络响应慢的情况下优化请求性能非常重要。

请确保在实际应用中正确处理异常和资源释放。上述代码中的`finally`块用于关闭响应和HttpClient，以释放系统资源。注意，`httpClient.close()`的调用应该在所有请求完成后进行，以确保所有请求都已处理完毕。

# Maven依赖项

## Jsoup

```xml
<!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.14.3</version>
</dependency>
```

## Apache Commons Lang 3

```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.12.0</version>
</dependency>
```

## Commons IO

```xml
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.11.0</version>
</dependency>
```

## 使用说明

使用Jsoup，添加依赖项：

- **Jsoup**：用于解析HTML和XML文档。
- **JUnit**（测试）：用于编写和运行测试。
- **commons-lang3** 和 **commons-io**（工具类）：提供额外的工具方法和功能。

# Jsoup解析 URL

> Jsoup可以直接输入URL，它会发起请求并获取数据，封装为Document对象。
>

## 使用Jsoup解析URL

```java
@Test
public void testJsoupUrl() throws Exception {
    // 解析URL地址
    Document document = Jsoup.parse(new URL("http://www.itcast.cn/"), 1000);
    // 获取title的内容
    Element title = document.getElementsByTag("title").first();
    System.out.println(title.text());
}
```

## 使用Jsoup解析字符串

```java
@Test
public void testString() throws Exception {
    // 使用工具类读取文件，获取字符串
    String content = FileUtils.readFileToString(new File("C:\\Users\\tree\\Desktop\\test.html"), "utf8");
    // 解析字符串
    Document doc = Jsoup.parse(content);
    String title = doc.getElementsByTag("title").first().text();
    System.out.println(title);
}
```

# 使用DOM方式遍历文档

元素获取：

1. 根据id查询元素 `getElementById`
2. 根据标签获取元素 `getElementsByTagName`
3. 根据class获取元素 `getElementsByClassName`
4. 根据属性获取元素 `getElementsByAttribute`

```java
// 1. 根据id查询元素
Element element = document.getElementById("city_bj");

// 2. 根据标签获取元素
element = document.getElementsByTag("title").first();

// 3. 根据class获取元素
element = document.getElementsByClass("s_name").last();

// 4. 根据属性获取元素
element = document.getElementsByAttribute("abc").first();
element = document.getElementsByAttributeValue("class", "city_con").first();
```

解析字符串时，地址后面还有个参数为“utf8”。

# 爬虫京东项目配置

## 资源配置 (`application.properties`)

> 将 `application.properties` 文件放在 `main` 目录下的 `resources` 文件夹中。
>

```properties
# DB Configuration
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/crawler
spring.datasource.username=root
spring.datasource.password=147258

# JPA Configuration
spring.jpa.database=MySQL
spring.jpa.show-sql=true
```

## Maven依赖项 (`pom.xml`)

确保在 `pom.xml` 文件中添加以下依赖项。

### Spring Boot Starter Web

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.6.2</version>
</dependency>
```

### MySQL Connector Java

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>
```

### Spring Boot Starter Data JPA

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>2.6.2</version>
</dependency>
```

### Apache HttpComponents HttpClient

```xml
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
```

### Jsoup

```xml
<!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.13.1</version>
</dependency>
```

### Apache Commons Lang 3

```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.12.0</version>
</dependency>
```

这是项目所需的依赖项。