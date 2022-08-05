---
title: ElasticSearch学习
date: 2021-08-25 18:42:55
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825185001.png
---

# 这是ElasticSearch技术的相关学习嗷！

<!--more-->

# ES概述：

- 开源的高拓展的分布式全文搜索i引擎

## 安装：

[ElasticSearch官网](https://www.elastic.co/cn/)

- Windows版本是一个zip，解压缩就能用嗷！！！

- bin中有一个.bat文件：![image-20210825190026556](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825190026.png)

  和JMeter一样，解压缩就能够使用嗷！！！

- 启动结果：

![image-20210825190228714](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825190228.png)

```json
{
  "name" : "ALEXANDER",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "xaE-lmEEQmq9asjVK1QZow",
  "version" : {
    "number" : "7.14.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "dd5a0a2acaa2045ff9624f3729fc8a6f40835aa1",
    "build_date" : "2021-07-29T20:49:32.864135063Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```



- 可能出现的问题以及解决措施：

![image-20210825190346381](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825190346.png)

- JSON中的集合可以使用[]，并且在其中列出每一个元素来表示嗷！！！

## 基本概念：

- 类比：

![image-20210825191728267](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825191728.png)

- 倒排索引：

和正向索引相对应

what is 正向索引？

通过主键关联文章，在文章中去一个个查对应的值

what is 倒排索引？

将查询关键字和主键关联，从而建立和文章的关联

![image-20210825192822070](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825192822.png)

![image-20210825192839131](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825192839.png)

![image-20210825192858840](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825192859.png)

这样找某个关键词就不用扫描所有的行去获得，直接根据我们记录的倒排列表，找到列表中对应的行，然后查找对应的行就行了嗷！！！

**倒排索引创建索引的流程：**

**1） 首先把所有的原始数据进行编号，形成文档列表**

**2） 把文档数据进行分词，得到很多的词条，以词条为索引。保存包含这些词条的文档的编号信息。**

**搜索的过程：**

**当用户输入任意的词条时，首先对用户输入的数据进行分词，得到用户要搜索的所有词条，然后拿着这些词条去倒排索引列表中进行匹配。找到这些词条就能找到包含这些词条的所有文档的编号。**

**然后根据这些编号去文档列表中找到文档**

作者：喔喔牛
链接：https://www.zhihu.com/question/23202010/answer/254503794
来源：知乎

## 索引操作：

- 对比关键型数据库，创建索引等同于创建数据库嗷！！！

![image-20210825193710897](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825193901.png)

### PUT创建索引：

![image-20210825194020804](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194020.png)

Put请求具有幂等性，就是反复提交都是一样的结果，没有影响嗷！！！

![image-20210825194130626](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194130.png)

Post请求在这种情况下无法使用嗷！！！

![image-20210825194242973](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194243.png)





### GET获得索引信息：

![image-20210825194508151](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194508.png)

如果想要查看所有的索引，也是GET，但是就要使用特殊路径了嗷！！！

![image-20210825194641977](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194642.png)

`http://127.0.0.1:9200/_cat/indices?v`

### DELETE删除索引：

![image-20210825194757555](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194757.png)

再去查看索引信息：

![image-20210825194856219](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825194856.png)

发现被成功删除了嗷！！！

## 文档操作：

### POST创建文档：

![image-20210825201457647](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825201457.png)

下面这个id是唯一标识嗷，同样的请求发出之后，id会有不同。因此POST请求不是幂等性的嗷！！！PUT不行，因为PUT是幂等性的操作！！！

![image-20210825202005422](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202005.png)

- 自定义ID如何进行：

![image-20210825201902782](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825201902.png)

这样每次返回值就是一样的了嗷！！！这是啥？？？这是幂等性操作！！！我们再来试试PUT！！！成功了！！！

![image-20210825202139316](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202139.png)

- 并且上面这种方式，使用PUT也能够成功CREAT嗷！！！

![image-20210825202225989](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202226.png)

### GET获得文档信息：

![image-20210825202417855](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202418.png)

- 这个_id就类似于我们的主键嗷！！！
- 如果查不到的话下面那个found就会为false

- 如果想要查找所有文档数据呢？

![image-20210825202627542](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202627.png)

### PUT更新文档信息：

![image-20210825202817180](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202817.png)

![image-20210825202835549](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825202835.png)

- 上面这种称之为全量数据的更新
- 更新局部的数据，每次更新结果是不一样的，不符合幂等性，因此用POST请求嗷！！！

![image-20210825203501196](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825203501.png)

注意地址中间是update嗷！！！

![image-20210825203629704](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825203629.png)

### DELETE删除文档：

![image-20210825203727520](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825203727.png)

不能够重复删除嗷！！！

![image-20210825203757814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825203758.png)



## 条件查询：

### 分页查询和排序等：

![image-20210825204055644](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825204055.png)

- 上面的请求参数不好加哈，而且容易乱码，写到请求体中

![image-20210825204253535](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825204253.png)

- 全部查询：

![image-20210825205914388](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825205914.png)

- 分页查询方式：

![image-20210825210112680](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825210112.png)

上面这个就是分页查询，从第几页开始，查多少条记录这种嗷！！！

至于这个分页的情况下，页码的计算公式如下：

![image-20210825221030166](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825221030.png)

这种方式就能够非常方便的计算出我们现在的那条数据在第几页嗷！！！

- 过滤数据源，只显示某些字段：

![image-20210825221551853](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825221551.png)

在JSON中添加对应的字段的指定嗷！！！

- 甚至可以对于特定的字段排序：

![image-20210825221803545](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825221803.png)

用什么排序，以及怎么排序嗷！！！



### 范围查询和逻辑连接等：

- ![image-20210825222702250](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825222702.png)

上面match中的就是条件，例如"_source"中的category等，限制它们匹配等。

相当于逻辑判断中的and

- 那么如果想用逻辑判断中的or呢？

![image-20210825223611903](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825223612.png)

- 范围的查询：

再多加上一个条件嗷！！！

![image-20210825223756820](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825223756.png)



### 完全匹配和高亮显示等：

- 在保存文档数据的时候，ElasticSearch会自动对于文档进行分值拆解工作，将拆解后的数据保存在倒排索引当中。即使使用文字的一部分也能够成功查询到数据。

![image-20210825224101933](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825224102.png)

- ES查询的时候也会将我们查询的内容分解，然后去索引中找到对应的值进行匹配。导致”小华“，既可以查到小米，又可以查到华为嗷！！！这个肯定是有问题的！！！
- 那么如何进行完全匹配呢？

![image-20210825224325304](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825224325.png)

- 高亮显示如何做到呢？

![image-20210826085602153](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826085602.png)



### 聚合查询：

- 分组统计：

![image-20210826085739285](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826085739.png)

![image-20210826085841738](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826085855.png)

- 平均值：

![image-20210826085940995](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826085941.png)

- 创建Mapping：

![image-20210826090557371](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826090557.png)

![image-20210826090705123](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826090705.png)

查询验证：

![image-20210826090902327](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826090902.png)

![image-20210826090924697](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826090925.png)

![image-20210826091011282](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826091011.png)



# Java操作ES：

1. 建立项目
2. POM
3. 测试

## 索引操作：

### 连接测试：

```java
public class ESTest_Client {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );
        
        restHighLevelClient.close();
    }
}
```

### 创建索引：

```java
public class ESTest_Index_Create {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //创建索引
        CreateIndexRequest createIndexRequest = new CreateIndexRequest("user");
        CreateIndexResponse createIndexResponse = restHighLevelClient.indices().create(createIndexRequest, RequestOptions.DEFAULT);

        //相应状态
        boolean acknowledged = createIndexResponse.isAcknowledged();
        System.out.println("acknowledged = " + acknowledged);

        restHighLevelClient.close();
    }
}
```

### 查询索引：

![image-20210826104545374](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826104545.png)

```java
public class ESTest_Index_Search {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //获得索引
        GetIndexRequest request = new GetIndexRequest("user");
        GetIndexResponse getIndexResponse = restHighLevelClient.indices().get(request,RequestOptions.DEFAULT);

        //结果响应
        System.out.println("getIndexResponse.getAliases() = " + getIndexResponse.getAliases());
        System.out.println("getIndexResponse.getMappings() = " + getIndexResponse.getMappings());
        System.out.println("getIndexResponse.getSetting() = " + getIndexResponse.getSettings());

        restHighLevelClient.close();
    }
}
```

- 这里其实就获取到了对应的属性嗷！！！

### 删除索引：

```java
public class ESTest_Index_Delete {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //获得索引
        DeleteIndexRequest deleteRequest = new DeleteIndexRequest("user");
        AcknowledgedResponse acknowledgedResponse = restHighLevelClient.indices().delete(deleteRequest, RequestOptions.DEFAULT);

        //结果响应
        System.out.println("acknowledgedResponse.isAcknowledged() = " + acknowledgedResponse.isAcknowledged());

        restHighLevelClient.close();
    }
}
```

- summary：
  - 构建对应操作的Request
  - 利用client发送到ES，并获得对应的返回结果Response
  - 分析Response



## 文档操作：

### 插入数据：

```java
public class ESTest_Doc_Insert {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //插入数据
        IndexRequest request = new IndexRequest();
        request.index("user").id("1001");
        User user = new User();
        user.setName("tuzi");
        user.setAge(20);
        user.setSex("男");

        //插入的数据必须转换为JSON格式嗷！！！
        ObjectMapper mapper = new ObjectMapper();
        String userJson = mapper.writeValueAsString(user);
        request.source(userJson, XContentType.JSON);

        //返回结果
        IndexResponse response = restHighLevelClient.index(request,RequestOptions.DEFAULT);
        System.out.println(response.getResult());


        restHighLevelClient.close();
    }
```

### 更新数据：

```java
public class ESTest_Doc_Update {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //修改数据
        UpdateRequest request = new UpdateRequest();
        request.index("user").id("1001");
        request.doc(XContentType.JSON,"sex","女");

        //返回结果
        UpdateResponse response = restHighLevelClient.update(request, RequestOptions.DEFAULT);
        System.out.println("response = " + response);

        restHighLevelClient.close();
    }
}
```

![image-20210826112126641](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826112127.png)

### 查询数据：

```java
public class ESTest_Doc_Get {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //修改数据
        GetRequest getRequest = new GetRequest();
        getRequest.index("user").id("1001");
        GetResponse getResponse = restHighLevelClient.get(getRequest, RequestOptions.DEFAULT);

        //返回结果
        System.out.println("getResponse = " + getResponse.getSourceAsString());

        restHighLevelClient.close();
    }
}
```

### 删除数据：

```java
public class ESTest_Doc_Delete {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //删除数据
        DeleteRequest request = new DeleteRequest();
        request.index("user").id("1001");
        DeleteResponse response = restHighLevelClient.delete(request,RequestOptions.DEFAULT);


        //返回结果
        System.out.println("response.toString() = " + response.toString());

        restHighLevelClient.close();
    }
}
```



### 批量更新&批量删除：

```java
public class ESTest_Doc_Insert_Batch {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //批量插入数据
        BulkRequest request = new BulkRequest();

        request.add(new IndexRequest().index("user").id("1001").source(XContentType.JSON,"name","zhangsan"));
        request.add(new IndexRequest().index("user").id("1002").source(XContentType.JSON,"name","lisi"));
        request.add(new IndexRequest().index("user").id("1003").source(XContentType.JSON,"name","wangwu"));

        BulkResponse response = restHighLevelClient.bulk(request, RequestOptions.DEFAULT);
        System.out.println("response.getTook() = " + response.getTook());
        System.out.println("response.getItems() = " + response.getItems());

    }
}
```



```java
public class ESTest_Doc_Delete_Batch {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //批量插入数据
        BulkRequest request = new BulkRequest();

        request.add(new DeleteRequest().index("user").id("1001"));
        request.add(new DeleteRequest().index("user").id("1002"));
        request.add(new DeleteRequest().index("user").id("1003"));

        BulkResponse response = restHighLevelClient.bulk(request, RequestOptions.DEFAULT);
        System.out.println("response.getTook() = " + response.getTook());
        System.out.println("response.getItems() = " + response.getItems());

    }
}
```



## 高级查询：

### 全量查询：

```java
public class ESTest_Doc_Insert_Batch {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //批量插入数据
        BulkRequest request = new BulkRequest();

        request.add(new IndexRequest().index("user").id("1001").source(XContentType.JSON,"name","zhangsan","age",30,"sex","男"));
        request.add(new IndexRequest().index("user").id("1002").source(XContentType.JSON,"name","lisi","age",30,"sex","女"));
        request.add(new IndexRequest().index("user").id("1003").source(XContentType.JSON,"name","wangwu","age",40,"sex","男"));
        request.add(new IndexRequest().index("user").id("1004").source(XContentType.JSON,"name","wangwu1","age",40,"sex","女"));
        request.add(new IndexRequest().index("user").id("1005").source(XContentType.JSON,"name","wangwu2","age",50,"sex","男"));
        request.add(new IndexRequest().index("user").id("1006").source(XContentType.JSON,"name","wangwu3","age",50,"sex","男"));


        BulkResponse response = restHighLevelClient.bulk(request, RequestOptions.DEFAULT);
        System.out.println("response.getTook() = " + response.getTook());
        System.out.println("response.getItems() = " + response.getItems());

    }
}
```

- 准备工作先插入数据嗷！！！
- 下面就是查询代码：

```java
public class ESTest_Doc_Query {
    public static void main(String[] args) throws IOException {
        //创建客户端
        RestHighLevelClient restHighLevelClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost",9200,"http"))
        );

        //查询索引中所有的数据嗷
        SearchRequest request = new SearchRequest();
        request.indices("user");

        request.source(new SearchSourceBuilder().query(QueryBuilders.matchAllQuery()));

        SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);

        //返回结果
        System.out.println("getResponse = " + search.getHits());
        System.out.println("search.getTook() = " + search.getTook());
        for (SearchHit hit : search.getHits()) {
            System.out.println("hit.getSourceAsString() = " + hit.getSourceAsString());
        }

        restHighLevelClient.close();
    }
}
```

### 条件查询：

```java
//查询索引中符合条件的数据嗷
SearchRequest request = new SearchRequest();
request.indices("user");

request.source(new SearchSourceBuilder().query(QueryBuilders.termQuery("age",30)));

SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);

//返回结果
System.out.println("getResponse = " + search.getHits());
System.out.println("search.getTook() = " + search.getTook());
for (SearchHit hit : search.getHits()) {
    System.out.println("hit.getSourceAsString() = " + hit.getSourceAsString());
}

restHighLevelClient.close();
```

- 实际上就是构造的query条件不一样而已嗷！！！

### 分页查询：

```java
SearchRequest request = new SearchRequest();
request.indices("user");

SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
builder.from(0);
builder.size(2);
request.source(builder);

SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);

//返回结果
System.out.println("getResponse = " + search.getHits());
System.out.println("search.getTook() = " + search.getTook());
for (SearchHit hit : search.getHits()) {
    System.out.println("hit.getSourceAsString() = " + hit.getSourceAsString());
}

restHighLevelClient.close();
```

### 查询排序：

```java
SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
builder.sort("age", SortOrder.DESC);
request.source(builder);
```

- 本质上就是设置builder，然后将对应设置好的builder添加到request中嗷！！！通过request去执行嗷！！！有点像定制化装配builder，然后request带去执行的感觉嗷！！！

### 字段过滤：

```java
SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
        String[] excludes = {};       //查询中过滤掉的字段
        String[] includes = {"name"}; //查询中包含的字段
        builder.fetchSource(includes,excludes);
        request.source(builder);
```

### 组合查询：

```java
 	   SearchSourceBuilder builder = new SearchSourceBuilder();
        BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();

//        boolQueryBuilder.must(QueryBuilders.matchQuery("age",30));
//        boolQueryBuilder.must(QueryBuilders.matchQuery("sex","男"));
//        boolQueryBuilder.mustNot(QueryBuilders.matchQuery("sex","男"));
        boolQueryBuilder.should(QueryBuilders.matchQuery("age",30));
        boolQueryBuilder.should(QueryBuilders.matchQuery("age",40));

        builder.query(boolQueryBuilder);

        request.source(builder);

        SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);
```

### 范围查询：

```java
SearchRequest request = new SearchRequest();
request.indices("user");

SearchSourceBuilder builder = new SearchSourceBuilder();
RangeQueryBuilder rangeQuery = QueryBuilders.rangeQuery("age");

rangeQuery.gte(30);
rangeQuery.lt(40);

builder.query(rangeQuery);

request.source(builder);

SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);
```

- 最大的builder是SearchSourceBuilder
- 下面还有类似于BoolQueryBuilder和RangeQueryBuilder等下属builder
- 设置完对应的条件之后，再整体组装为大的builder即可实现不同的功能嗷！！！

### 模糊查询：

- 讲的太乱了，后面补上，服了这个老师了。

### 最大值查询&分组查询：



