# 1.1 初识ElasticSearch

<ArticleTopAd/>

## 1.1.1 ElasticSearch 简介

Elasticsearch是一个开源的、基于Apache Lucene库的搜索引擎，它提供了全文搜索能力，特别适合于处理大量文本数据。下面是对Elasticsearch的一些主要特点和功能的概述：

1. 分布式搜索： Elasticsearch是分布式的，可以在多个节点上分散和复制数据，提供了高可用性和容错性。
2. 全文搜索： Elasticsearch使用Apache Lucene库进行全文搜索，可以理解自然语言，对文本进行高亮显示，并支持多种语言。
3. 实时搜索和分析： Elasticsearch可以实时地搜索和分析数据。当新增数据时，可以立即被搜索到并进行分析。
4. 简单的API： Elasticsearch的RESTful API易于使用，可以方便地进行数据的索引、查询和分析。
5. 可扩展性： Elasticsearch可以方便地扩展其功能和性能，可以通过插件机制来添加新功能。
6. 多租户能力： Elasticsearch可以支持多租户环境，每个租户可以有自己的索引和设置。
7. 支持多种数据源： Elasticsearch可以接收来自多种数据源的数据，包括关系型数据库、NoSQL数据库、日志文件等。
8. 安全性和访问控制： Elasticsearch提供了安全性和访问控制机制，可以设置用户和角色的权限。
9. 监控和管理工具： Elasticsearch提供了丰富的监控和管理工具，方便用户管理和优化其集群的性能。
10. 总的来说，Elasticsearch是一个强大、灵活、易于使用的搜索引擎，适用于各种需要全文搜索、实时数据分析和数据管道的应用场景。

## 1.1.2 ElasticSearch 基本概念

以下是ElasticSearch的一些基本概念：

1. 索引（Index）：ElasticSearch中的索引类似于传统数据库中的数据库。它是一个包含一组相关文档的逻辑命名空间。索引用于组织和存储文档，并提供快速的数据访问。
2. 类型（Type）：索引可以包含多个类型，每个类型定义了一组具有相似结构的文档。类型类似于数据库中的表。**在Elasticsearch 7.x中，Type的概念已经被删除了**，就是一个索引下面只能有一种类型`_doc`。
3. 文档（Document）：文档是ElasticSearch中的最小数据单元。它是一个以JSON格式表示的数据对象。文档被存储在索引中，并且可以使用唯一的ID进行检索。
4. 字段（Field）：文档中的数据通过字段来表示。每个字段都有一个名称和一个对应的值。字段可以是不同类型的，例如字符串、整数、日期等。
5. 映射（Mapping）：映射定义了索引中文档的字段类型和属性。它类似于数据库表的模式定义。映射可以自动创建，也可以手动定义。
6. 分片和副本（Sharding and Replication）：为了实现高性能和高可用性，ElasticSearch将索引分成多个分片进行存储和处理。每个分片可以在集群中的不同节点上进行复制，以提供数据冗余和故障恢复能力。
7. 查询（Query）：ElasticSearch提供了丰富的查询API，用于在索引中搜索文档。查询可以根据各种条件过滤和排序文档，并返回与查询条件匹配的结果。
8. 聚合（Aggregation）：聚合是一种数据分析功能，用于从大量数据中提取有意义的信息。它可以执行各种统计、分组和计算操作，以生成汇总结果。

## 1.1.3 ElasticSearch 和关系型数据库的对比

Elasticsearch是面向文档型数据库，一条数据就是一个文档。下面是Elasticsearch存储文档数据和MySQL存储数据的一个比较。

1. 数据存储格式
   ![图1-1](D:\个人文档\Guide\image\1-1.png)
   ES中的Index可以看成一个库，Types相当于表，Documents相当于表的行。**在Elasticsearch 7.x中，Type的概念已经被删除了，就是一个索引下面只能有一种类型。**
2. 数据查询：
   - ElasticSearch使用DSL语句查询，提供强大的全文搜索和分析功能。它支持复杂的文本查询、模糊匹配、聚合和分析等。
   - MySQL使用SQL查询语言，支持传统的关系型数据库查询操作，例如SELECT、JOIN、GROUP BY等。
3. 数据扩展性：
   - ElasticSearch是一个分布式数据库，可以将数据分片存储在多个节点上，以提供高性能和可扩展性。它可以处理大规模数据集和高并发请求。
   - MySQL可以通过主从复制和分区等方式实现一定程度的扩展性，但在处理大规模数据和高并发负载时，性能可能受限。
4. 数据一致性：
   - ElasticSearch在写入数据时，采用近实时（near real-time）的方式，数据会先写入内存缓冲区，然后根据配置的刷新时间刷新到磁盘。因此，在写入后的一小段时间内，数据可能不会立即可见。
   - MySQL保证数据的一致性，写入操作会立即持久化到磁盘，并且读取操作可以立即获取最新数据。
5. 数据处理能力：
   - ElasticSearch在大规模数据的搜索和分析方面表现出色，特别适用于全文搜索、日志分析、实时监控等场景。
   - MySQL在传统的事务处理和关系型数据操作方面表现出色，适用于事务处理、数据关联和复杂查询等场景。

总体来说，ES在海量数据检索上有很大的性能优势，处理事务性业务不行。需要的配置（金钱）会比较昂贵。

**这里如果看不太明白没有关系，可以从[搭建](/chapter1/install_elastic_search)开始学习，先上手操作起来。**

<ArticleDownAd/>

# 1.2 搭建ElasticSearch环境

> ElasticSearch下载地址：[点击打开](https://www.elastic.co/cn/downloads/past-releases#elasticsearch)
>
> 检索7.9.3版本，本书用7.9.3版本讲解，注意版本不要差距过大，否则导致会出现很多小问题

![图1-2](D:\个人文档\Guide\image\1-2.png)

**ElasticSearch是基于Java语言开发的，所以需要先配置Java的JDK环境，我用的环境是JDK1.8**



## 1.2.1 Linux环境下安装ES

### 1. 先新建一个用户（出于安全考虑，Elasticsearch默认不允许以root账号运行。）

```shell
创建用户：
useradd esuser
设置密码：
passwd esuser
```

### 2. 下载ES安装包并解压到es目录

修改ES配置文件设置JVM堆大小 **此处为演示，要根据实际情况来，一般情况下，堆大小=机器内存/2**

config/jvm.options

```java
-Xms1g
-Xmx1g
```

### 3. 配置limits.conf文件 

修改系统 /etc/security/limits.conf文件 

```shell
vi /etc/security/limits.conf 
```

增加配置

```shell
* soft nofile 65536
* hard nofile 65536
```

**注意*不要手贱去掉**

### 4. 修改系统/etc/sysctl.conf文件

```shell
vi /etc/sysctl.conf
最后添加一行
vm.max_map_count=655360
sysctl -p
```

### 5. 启动ES

1. 将ES文件夹下的所有目录的所有权限迭代给esuser用户

```shell
chgrp -R esuser ./es
chown -R esuser ./es
chmod 777 es
```

2. 先切换到esuser用户启动

1）切换esuser用户

```shell
su esuser
```

2）通过 -d 参数，表示后台运行

```shell
./bin/elasticsearch -d
```

到这里就已经结束了，可以通过 logs/elasticsearch.log 日志，查看启动是否成功。

## 1.2.2 Windows环境下安装ES

### 1. 下载Windows版本

### 2. 下载安装包后解压文件

### 3. 启动ES

进入 bin 目录下，双击执行 elasticsearch.bat 文件。执行文件后，可以在窗口中看到 Elasticsearch 的启动过程。

![图1-3](D:\个人文档\Guide\image\1-3.png)

在 Elasticsearch 启动后，可以在浏览器的地址栏输入：[http://localhost:9200/](http://localhost:9200/) 查看启动情况



## 1.2.3 Docker环境下安装ES

### 1. 拉取对应的ES版本镜像

```shell
docker pull elasticsearch:7.9.3
```

![图1-4](D:\个人文档\Guide\image\1-4.png)

### 2. 查看镜像是否拉取成功

```shell
docker images
```

![图1-5](D:\个人文档\Guide\image\1-5.png)

### 3. 创建数据卷挂载目录

```shell
mkdir -p /user/es/config
mkdir -p /user/es/data
mkdir -p /user/es/logs
mkdir -p /user/es/plugins
```

### 4. 启动ES

```shell
docker run --name=es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -v /user/es/config:/usr/share/elasticsearch/config -v /user/es/data:/usr/share/elasticsearch/data -v /user/es/logs:/usr/share/elasticsearch/logs -v /user/es/plugins:/usr/share/elasticsearch/plugins -d elasticsearch:7.9.3
```

Docker所用启动参数说明：

> –-name 容器命名  
> -p 端口映射  
> -e 环境变量，单机  
> -v 数据卷挂载，映射配置文件，数据，日志，插件  
> -d 后台运行  

### 5. 查看运行状态

```shell
docker ps
```

# 1.3 ElasticSearch客户端

ElasticSearch目前比较流行的可视化客户端

- Head插件
- Kibana
- Cerebro

本书演示Kibana和Cerebro安装，任选其一学习即可  

<font color="red">**我个人比较喜欢用Cerebro，所以在本书大部分可视化场景操作都是用这个，书中示例语句可以直接copy到Kibana运行**</font>

对于Head插件，这个东西我个人感觉不太好用，所以本文不讲述  

**当然，有兴趣分享的同学可以提issues，我来做更新**

## 1.3.1. Kibana

### 1. 下载kibana

Cerebro是ES官网提供的一个开源的分析与可视化平台，设计出来用于和Elasticsearch一起使用的。

> Kibana下载地址：[点击打开](https://www.elastic.co/cn/downloads/past-releases#kibana)
>
> 检索7.9.3版本，本书用7.9.3版本讲解

![图1-6](D:\个人文档\Guide\image\1-6.png)

选择对应自己需要的操作系统版本，下载后解压。

### 2. 配置Kibana

Kibana的配置文件位于config/kibana.yml中。您需要打开该文件并进行以下配置：

- server.port：设置Kibana使用的端口。默认端口是5601。
- elasticsearch.hosts：设置Elasticsearch的地址。默认地址是[http://localhost:9200](http://localhost:9200)。

```shell
server.port: 5601
elasticsearch.hosts: ["http://localhost:9200"]
```

>可以新增配置，也可以把文件对应的配置注释删掉

### 3. 启动Kibana

- Linux下进入目录，输入以下运行命令

  ```shell
  bin/kibana
  ```

- Windows下直接双击kibana.bat文件启动

启动较慢，需要有耐心等几分钟

一旦Kibana启动，您可以在Web浏览器中访问它。默认情况下，Kibana运行在5601端口，所以您可以在浏览器中输入[http://localhost:5601](http://localhost:5601)访问它

<img alt='图1-7' src="D:\个人文档\Guide\image\1-7.png" width='50%'/>

## 1.3.2. Cerebro

### 1. 下载Cerebro

Cerebro是一款非常好用的用来监控ES集群的项目。通过此插件我们可以查看ES集群的详细状况、索引的创建、配置等工作。

> Cerebro下载地址：[点击打开](https://github.com/lmenezes/cerebro/releases)
>
> 我用v0.8.5版本

### 2. 解压Cerebro

不用多讲

### 3. 启动Cerebro

```shell
bin/cerebro
```

默认启动端口是9000，浏览器中输入[http://localhost:9000](http://localhost:9000)访问它

![图1-8](D:\个人文档\Guide\image\1-8.png)

**Node address输入 `http://localhost:9200` 点击Connect连接**

![图1-9](D:\个人文档\Guide\image\1-9.png)

# 1.4 程序连接ElasticSearch


## 1.4.1 SpringBoot集成ElasticSearch

SpringBoot是目前Java开发主流的开源框架，很多项目都是基于SpringBoot开发，在本书中，我们使用SpringBoot演示操作ES。

### 1. 创建项目

创建SpringBoot项目，导入Maven依赖
导入es依赖需要的依赖、fastjson依赖，fastjson后续会用

```xml
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>7.9.3</version>
</dependency>
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.9.3</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.78</version>
</dependency>
```

### 2. 编写配置

1. 新建配置类 `ElasticSearchConfig` 写入以下代码

```java
import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class ElasticSearchConfig {

    @Bean
    public RestHighLevelClient esRestClient(){
        // ES连接地址，集群写多个
        RestClientBuilder builder = RestClient.builder(
                new HttpHost("localhost", 9200, "http"));
        RestHighLevelClient client = new RestHighLevelClient(builder);
        return client;
    }
}
```

创建ES客户端对象 `RestHighLevelClient` 添加配置信息，注入到Spring容器中

2. 新建控制层类 `EsController` 增加创建索引接口

```java
package cn.chaosopen.es.controller;

import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.CreateIndexRequest;
import org.elasticsearch.common.xcontent.XContentType;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;

@RestController
public class EsController {

    @Autowired
    private RestHighLevelClient client;

    @RequestMapping("/createIndex")
    public Boolean createIndex(String indexName) {
        CreateIndexRequest request = new CreateIndexRequest(indexName);
        request.mapping(
                "{\n" +
                        "  \"properties\": {\n" +
                        "    \"message\": {\n" +
                        "      \"type\": \"text\"\n" +
                        "    }\n" +
                        "  }\n" +
                        "}",
                XContentType.JSON);
        try {
            client.indices().create(request, RequestOptions.DEFAULT);
            return true;
        } catch (IOException e) {
            e.printStackTrace();
        }
        return false;
    }
}
```

此段代码定义一个接口，根据请求参数值，创建对应名称的索引

示例目录如下：

![图1-10](D:\个人文档\Guide\image\1-10.png)

### 3. 启动项目测试

1. 启动项目，查看日志如下所示为运行成功

![图1-11](D:/WorkSpace/elasticsearch_in_action/src/imgs/1-11.png)

2. 打开接口地址，并传递索引名称参数创建索引

[http://localhost:8080/createIndex?indexName=test_index](http://localhost:8080/createIndex?indexName=test_index)

SpringBoot默认端口是8080，请根据项目实际端口进行改动

3. 验证创建结果

- kibana执行语句

```shell
GET _cat/indices
```

查看索引列表

- 打开Cerebro [http://localhost:9000](http://localhost:9000)

![图1-12](D:\个人文档\Guide\image\1-12.png)

我们会发现多了一个索引，说明我们操作成功了

## 1.4.2 其他程序连接方式待补充