---
title: Nebula Graph图库预研
tags:
  - graph
  - nebula graph
  - go
categories:
  - graph
cover: 'https://www-cdn.nebula-graph.com.cn/nebula-blog/Online01.png'
date: 2022-01-22 12:48:00
---
#                                                                  图数据库相关预研交流

## 为什么要用图数据库

图数据库是基于图模型的数据库。相比较于关系型数据库，图数据库是真正注重“关系”的数据库，图数据库在处理关联关系上具有完全的优势。世界上几乎所有领域的事物都有内在联系，像关系型数据库这样的建模系统会将关系和属性单独存储到表和列中，这使得数据管理费时费力，并且难以挖掘和发现深层的数据之间的联系。


- 图数据库相对于传统关系型数据库的优势：

    - 易建模：图中存储的是带属性的实体和带属性的关系，能够更为直接、自然的表达现实世界。

    - 易查询：相比于关系型数据库的复杂关联查询sql，图查询语言，方便描述查询条件，并且图数据库得益于其存储模型和基于存储模型上优化的查询算法，可以高效的处理复杂关联关系查询。

    - 易展示：由于建模的简单，基于图数据库的图形化展示也非常友好。

    - 易扩展：相对于关系型数据库对新类型的数据和关系需要重新设计模式，图数据库的数据结构清晰简明，可以灵活应对海量的关系变化进行增删改。

- 应用场景：

    - 知识图谱：知识图谱对海量信息进行智能化处理，形成大规模的知识库并进而支撑业务应用，为项目中的决策，搜索，智能推荐功能提供支撑。

    - 行业领域：图是基于事物关联关系的模型表达，具有天然解释性，因此图数据库与图处理引擎集成的图系统带来的强大的图存储和分析能力，推动了产业数据中信息和知识的挖掘与利用。

## 图数据库选型

#### *为什么放弃 JanusGraph*

- 写入效率低，基于ES的索引后端和Hbase的存储后端本身具有局限性
- 查询效率低，索引本身基于ES，非图结构
- 生态不完善，周边工具少
- 中文社区不活跃，文档少

#### *为什么选择Nebula Graph*

- 索引和存储都基于RocksDB，本身就是高效磁盘KV数据库，能做到和Redis内存数据库差不多的查询性能

- 自带语法类SQL，易于学习，且支持Cypher语法（Neo4j语法）

  ```sql
  (root@nebula) [test]> (GO 1 TO 15 STEPS FROM 9596900216 OVER label_parent   YIELD dst(edge) AS id |GO 1  STEPS FROM $-.id OVER label YIELD properties($$).ks_name AS NAME,id($$) as id ,labels($$) AS label UNION DISTINCT GO 1 STEPS FROM 9596900216 OVER label YIELD properties($$).ks_name AS NAME,id($$) as id ,labels($$) AS label LIMIT [100]) | GO 1 STEPS FROM $-.id OVER branch, invest, workIn BIDIRECT YIELD properties($$).ks_name AS NAME,id($$) as id ,labels($$) AS label LIMIT [100];
  ```

  ```shell
  Got 61 rows (time spent 164274/175179 us)
  ```

- 面向中国市场，社区活跃，官方文档完善

- 周边生态完善，且开发活跃度高

#### *Nebula Graph生态*

> Nebula Graph 是一款开源的、分布式的、易扩展的原生图数据库，能够承载数千亿个点和数万亿条边的超大规模数据集，并且提供毫秒级查询。

![Nebula Graph 鸟瞰图](https://docs.nebula-graph.com.cn/2.6.1/1.introduction/nebula-birdview.png)

#### *Nebula Graph架构*

> Nebula Graph 由三种服务构成：Graph 服务、Meta 服务和 Storage 服务，是一种存储与计算分离的架构。各个节点的Meta 服务和Storage 服务通过**Raft**协议进行

##### *Raft*

> 分布式系统中，同一份数据通常会有多个副本，这样即使少数副本发生故障，系统仍可正常运行。这就需要一定的技术手段来保证多个副本之间的一致性。
>
> 基本原理：Raft 就是一种用于保证多副本一致性的协议。Raft 采用多个副本之间竞选的方式，赢得”超过半数”副本投票的（候选）副本成为 Leader，由 Leader 代表所有副本对外提供服务；其他 Follower 作为备份。当该 Leader 出现异常后（通信故障、运维命令等），其余 Follower 进行新一轮选举，投票出一个新的 Leader。Leader 和 Follower 之间通过心跳的方式相互探测是否存活，并以 Raft-wal 的方式写入硬盘，超过多个心跳仍无响应的副本会认为发生故障。
>
> 读写流程：对于客户端的每个写入请求，Leader 会将该写入以 Raft-wal 的方式，将该条同步给其他 Follower，并只有在“超过半数”副本都成功收到 Raft-wal 后，才会返回客户端该写入成功。对于客户端的每个读取请求，都直接访问 Leader，而 Follower 并不参与读请求服务。

![Nebula Graph architecture](https://docs-cdn.nebula-graph.com.cn/docs-2.0/1.introduction/2.nebula-graph-architecture/nebula-graph-architecture-1.png)

## 图构建

#### *图数据模型*

- 图空间（Space）

  图空间用于隔离不同团队或者项目的数据。不同图空间的数据是相互隔离的，可以指定不同的存储副本数、权限、分片等。

- 点（Vertex）

  点用来保存实体对象，特点如下：

    - 点是用点标识符（`VID`）标识的。`VID`在同一图空间中唯一。VID 是一个 int64，或者 fixed_string(N)。
    - 点必须有至少一个 Tag，也可以有多个 Tag。但不能没有 Tag。

- 边（Edge）

  边是用来连接点的，表示两个点之间的关系或行为，特点如下：

    - 两点之间可以有多条边。
    - 边是有方向的，不存在无向边。
    - 四元组 `<起点 VID、Edge type、边排序值 (Rank)、终点 VID>` 用于唯一标识一条边。边没有 EID。
    - 一条边有且仅有一个 Edge type。
    - 一条边有且仅有一个 rank。其为 int64，默认为 0。

- 标签（Tag）

  Tag 由一组事先预定义的属性构成。

- 边类型（Edge type）

  Edge type 由一组事先预定义的属性构成。

- 属性（Properties）

  属性是指以键值对（Key-value pair）形式存储的信息。

#### *图模型设计*

<img src="/img/hs-graph/image-20211126091300422.png" alt="image-20211126091300422"  />



##### 如何获取语义层级？

采用普林斯顿大学WordNet语义词典，将不同实体进行语义层级上的链接，形成hierarchy层级结构的概念-实体网络。

##### 有什么作用？

当实体和实体之间具有语义关系的时候，我们可以举个例子:

当用户需要查询一家生产研究某种药品的机构的时候，用户本身并不知道与该药品相关联的是科研机构（**academyn02**）还是企业（**companyn01**）或者是科研院校（**universityn03**）甚至是医疗机构（**hospitaln02**），那么传统关系型数据库的查询方式是通过药品id去关联四张表获取到的结果进行排序返回。独立建模的图数据库的查询方式是通过药品vid通过一度关系查询终点，并且限制终点的标签

当加入语义层级之后，我们可以看到该4个实体的上级概念都是**institutionn01**，我们在图数据库中对打上上面4个标签的个体另外打上**institutionn01**的标签，这样通过药品vid直接一度关系查询终点标签为**institutionn01**，就可以得到相同的结果

这对客户在不清楚目标实体具体是什么，但是大概有个目标概念范围的时候能得到很好的查询体验。

![image-20211126092643482](/img/hs-graph/image-20211126092643482.png)

- Repo
    <!-- - http://git.aihuoshi.net/wud/knowledge_graph -->
- Language
    - Markdown
    - nGQL
- Dependences

#### *图数据流*

##### *离线数据流*

<img src="/img/hs-graph/data_flow.png" alt="data_flow" style="zoom:80%;" />

- Repo
    <!-- - http://git.aihuoshi.net/keystone/graph-builder -->
- Language
    - Scala
- Dependences
    - Jdk 1.8
    - Scala 2.2.0
    - nebula-spark-connector
    - Maven 3.6.3



##### *实时数据流*

> 利用nebula-flink-connector库



## 图服务

#### *基础服务*

对nGQL进行封装，提供标准化查询接口，提供标准化写入接口，并提供标准接口文档

- Repo
    <!-- - http://git.aihuoshi.net/keystone/graph-server -->
- Language
    - Go
- Dependences
    - Gin 1.7.4+
    - Go 1.6+
- Features
    - [ ] 通用查询
        - [x] go语句 `根据指定条件遍历图`
            - [x] aggregation
            - [x] orderby
            - [x] n-steps
            - [x] sorting
            - [x] groupby
            - [x] direction
            - [x] pagination
        - [ ] match语句 `模式匹配`
        - [ ] lookup语句 `根据索引遍历图`
        - [ ] fetch语句 `获取点和边的属性`
        - [ ] unwind语句 `拆分列表`
    - [ ] 变量和复合查询
    - [x] 运算符
    - [x] 函数和表达式
    - [ ] 子图和路径
    - [x] Graphql
        - 支持graphql风格



#### *分析模型*

提供通用的分析模型，标准化出参和入参

- 股权穿透
- 社区发现
- 最短距离
- 标签传播
- 串标围标
- 产业链分析
- ...



#### *算法调度平台(一站式工作台)*



##### 基本功能

![img](https://p0.meituan.net/travelcube/cb913705f4da670a665bf6f00b66e044359254.png)



##### 与业务系统的结合

- 算法工程师发布模型，定义出入参和算法服务路由
- 用户注册apiKey
- 用户带apiKey和对应的参数RPC/HTTP调用算法服务
- 算法服务返回结果

![图2 机器学习在线服务示意图](https://p0.meituan.net/travelcube/fb9cbe21519c1c291ef74e568ba526f3165491.png)





#### *搜索场景*

Elasticsearch / Solr做搜索是目前工业上比较成熟的技术方案，通过Lucene的倒排索引数据结构能够低时耗、高准确返回搜索结果。但是传统的搜索场景局限于**关键字检索**，对于海量数据实体之间的关系和搜索query的意图识别力不从心，缺乏“人性化“。

在图数据库以及图计算近几年的崛起之下，基于知识图谱的搜索引擎以及成为主流，比如Google就是把搜索核心从倒排索引转向知识图谱。

在这个背景下，对query的意图识别成为搜索引擎的核心关键技术，加也是NLP在工业上应用的重点领域。

因此，构建产业大数据知识图谱并且支撑搜索、推荐业务，是可以预见的，明确的发展目标。从而给公司的产品提供更多的可能性。

搜索、推荐业务有几个特点

- 高适用

  > 基本所有项目和产品都具有搜索的场景，火石的产业搜索引擎没有技术到落地的业务需求沟壑

- 高频率

  > 搜索场景是用户高频使用的功能模块

- 高反馈

  > 用户对搜索的结果具有较高、较清晰的体验反馈



##### *基本架构*

<img src="/img/hs-graph/image-20211126095126214.png" alt="image-20211126095126214" style="zoom:70%;" />


## 图工具扩展

#### *图分析平台*

- 项目管理
- 数据源管理

- 可视化算子
    - filter
    - steps
    - match
    - scan
    - yield
- 模型管理
- 模型发布
- 控制台（带权限）

#### *图数据库管理工具*

- Nebula Writer (datax plugin？)
- 图库运行监控

- 元数据管理
    - space
    - schema
    - stats
- ETL流程任务管理
    - spark任务集群
- 图空间数据切割（SaaS -> 项目）