# 工具概览

> 本文档提供了三个常用工具的概览：Jsoup爬虫、Docker容器和Maven。这些工具在现代软件开发和项目管理中扮演着重要的角色，覆盖了从数据抓取、容器化部署到项目构建和管理的各个方面。

**目录**

1. [Jsoup爬虫](工具/Jsoup爬虫)
2. [Docker容器](框架/docker)
3. [Maven](工具/maven)

**Jsoup爬虫**

Jsoup是一个Java库，用于从HTML中提取和操纵数据。它提供了一个非常便捷的API来处理HTML文档，使得从网页中提取信息变得简单。

**核心功能**

- **解析HTML**：从URL、文件或字符串中解析HTML文档。
- **选择元素**：使用CSS选择器查找特定的元素。
- **提取数据**：从解析后的HTML中提取文本、链接、图片等数据。
- **修改HTML**：在解析后的HTML文档中添加、修改或删除元素。

**使用示例**

```java
Document doc = Jsoup.connect("http://example.com").get();
Element title = doc.select("title").first();
System.out.println(title.text());
```

**Docker容器**

Docker是一个开源平台，用于开发、交付和运行应用程序。它使用容器来隔离应用程序和环境，确保应用程序在任何地方都能以相同的方式运行。

**核心概念**

- **容器**：轻量级、可移植的、自给自足的软件运行环境。
- **镜像**：用于创建容器的模板，包含了运行应用程序所需的代码、运行时、系统工具、系统库等。
- **Dockerfile**：定义镜像内容的文本文件，包括基础镜像、维护者信息、环境变量、执行的命令等。

**使用示例**

```dockerfile
# 使用官方Java镜像
FROM java:8
# 将本地jar添加到容器中并更名为app.jar
ADD helloworld-0.0.1-SNAPSHOT.jar app.jar
# 暴露端口号
EXPOSE 8080
# 运行jar文件
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

**Maven**

Maven是一个项目管理和构建自动化工具，它使用一个名为POM（Project Object Model）的XML文件来描述项目的构建过程、依赖关系等。

**核心概念**

- **POM**：项目对象模型，定义了项目的构建、报告和依赖信息。
- **依赖管理**：自动处理项目依赖，确保项目所需的库版本正确且一致。
- **生命周期**：定义了一系列阶段，如编译、测试、打包、部署等。

**使用示例**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

以上概览提供了Jsoup爬虫、Docker容器和Maven的基础知识和核心概念，为进一步学习和实践打下坚实的基础。
