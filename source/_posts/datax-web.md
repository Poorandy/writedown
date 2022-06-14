---
title: datax web使用手册
tags:
  - datax
  - bigdata
categories:
  - data
date: 2022-01-21 10:30:12
---

> :green_book: 您正在浏览的是DataX Web v1.x的操作文档

## 新功能更新记录

### 重要功能发布记录

| 日期       | 内容                                                         |
| :--------- | ------------------------------------------------------------ |
| 2021-08-05 | 增量同步功能，及前端用户体验优化；EsWriter默认upsert方式增量同步 |
| 2021-08-06 | 新增指定EsWriter指定某个字段作为_id；                        |
| 2021-08-12 | 项目管理删除时改为逻辑删除；任务管理筛选条件新增执行状态；   |
| 2021-08-19 | 前端更新：增量时间字段模板，pg和es做区分                     |
| 2021-08-21 | 用户信息增加邮箱，创建任务时，告警邮件默认加入用户邮箱       |
| 2021-08-25 | 更新数据源时,可以选择更新所有用到该数据源的datax任务；判断相同调度时间jobCron的任务是否超过十个,如是,则调度时间后移10分钟 |
| 2021-08-31 | reader和writer字段模糊匹配接口                               |
| 2021-09-07 | 前端更改查看日志方式，新增查看最近一条日志                   |
| 2021-09-09 | 对任务管理，任务日志，数据源以及，datax任务模板做权限管理 ；邮件加入详细日志； 前端针对pg,加入视图配置 |
| 2021-09-13 | 加入钉钉报警                                                 |
| 2021-09-16 | 优化自定postgresql自定义sql的构建方式 ；postgresql前端增加writeMode |
| 2021-12-02 | esReader加scroll和size，去掉es7读取字段有doc和doc_as_upsert  |
| 2022-01-07 | 增加esWriter,自动创建索引                                    |
| 2022-01-14 | mysqlWriter增加writeMode                                     |
| 2022-01-16 | 子任务，根据project_id获取job list                           |
| 2022-01-18 | 日志搜索，可以根据日志id和任务名称模糊搜索； 项目管理增加用户权限 ；增加项目导出功能 |

### 历史版本



## 指南

### 介绍

火石DataX Web（以下简称DataX）是基于开源项目[datax-web](https://github.com/WeiYe-Jing/datax-web) 二次开发的分布式数据同步工具。特别对火石常用的数据库中间件插件做了针对性的优化。

DataX提供简单易用的操作界面，降低用户使用DataX进行数据同步的学习成本，提供傻瓜式任务配置方式，智能匹配字段映射，极大避免配置过程中出错。支持RDBMS、Hive、Hbase等数据源，支持实时查看数据同步进度及日志并且提供任务终止功能，集成XXL-JOB任务调度框架，提供定时增量方式同步数据。

DataX目前已经在公司内部推广服务于绝大多数项目和团队，展现其易用性和泛用性。公司内部也有专门的团队对DataX进行维护和迭代，并且建立钉钉群方便答疑。

#### 为什么是DataX

DataX是阿里云DataWorks数据集成的开源版本，在阿里巴巴集团内被广泛使用的离线数据同步工具/平台。DataX采用（plugin-framework）框架，将不同数据源的同步抽象为从源头数据源读取数据的Reader插件，以及向目标端写入数据的Writer插件，理论上DataX框架可以支持任意数据源类型的数据同步工作。




### 安装与登录

#### 依赖环境

- Language: Java 8（jdk版本建议1.8.201以上）
  Python2.7(支持Python3需要修改替换datax/bin下面的三个python文件，替换文件在doc/datax-web/datax-python3下)
- Environment: MacOS, Windows,Linux
- Database: Mysql5.7

#### 安装指南

##### 使用源码安装

###### 1：下载源码

<!-- http://git.aihuoshi.net/keystone/tool_datax_web.git -->

###### 2：编译打包

直接从Git上面获得源代码，在项目的根目录下执行如下命令

```shell
mvn clean install 
```

执行成功后将会在工程的build目录下生成安装包

```shell
build/datax-web-{VERSION}.tar.gz
```

###### 3：开始部署

###### 步骤1.解压安装包

在选定的安装目录，解压安装包

```shell
tar -zxvf datax-web-{VERSION}.tar.gz
```

###### 步骤2.执行一键安装脚本

进入解压后的目录，找到bin目录下面的install.sh文件，如果选择交互式的安装，则直接执行

```shell
./bin/install.sh
```

在交互模式下，对各个模块的package压缩包的解压以及configure配置脚本的调用，都会请求用户确认,可根据提示查看是否安装成功，如果没有安装成功，可以重复尝试； 如果不想使用交互模式，跳过确认过程，则执行以下命令安装

```shell
./bin/install.sh --force
```

###### 步骤3：数据库初始化

如果你的服务上安装有mysql命令，在执行安装脚本的过程中则会出现以下提醒：

```shell
Scan out mysql command, so begin to initalize the database
Do you want to initalize database with sql: [{INSTALL_PATH}/bin/db/datax-web.sql]? (Y/N)y
Please input the db host(default: 127.0.0.1): 
Please input the db port(default: 3306): 
Please input the db username(default: root): 
Please input the db password(default: ): 
Please input the db name(default: exchangis)
```

按照提示输入数据库地址，端口号，用户名，密码以及数据库名称，大部分情况下即可快速完成初始化。 如果服务上并没有安装mysql命令，则可以取用目录下/bin/db/datax-web.sql脚本去手动执行，完成后修改相关配置文件

```shell
vi ./modules/datax-admin/conf/bootstrap.properties
#Database
#DB_HOST=
#DB_PORT=
#DB_USERNAME=
#DB_PASSWORD=
#DB_DATABASE=
```

按照具体情况配置对应的值即可。

###### 步骤4：配置

安装完成之后，

在项目目录： /modules/datax-admin/bin/env.properties 配置邮件服务(可跳过)

```shell
MAIL_USERNAME=""
MAIL_PASSWORD=""
```

此文件中包括一些默认配置参数，例如：server.port，具体请查看文件。

在项目目录下/modules/datax-execute/bin/env.properties 指定PYTHON_PATH的路径

```shell
vi ./modules/{module_name}/bin/env.properties

### 执行datax的python脚本地址
PYTHON_PATH=

### 保持和datax-admin服务的端口一致；默认是9527，如果没改datax-admin的端口，可以忽略
DATAX_ADMIN_PORT=
```

此文件中包括一些默认配置参数，例如：executor.port,json.path,data.path等，具体请查看文件。

###### 步骤5：启动服务

- 一键启动所有服务

```shell
./bin/start-all.sh
```

中途可能发生部分模块启动失败或者卡住，可以退出重复执行，如果需要改变某一模块服务端口号，则：

```shell
vi ./modules/{module_name}/bin/env.properties
```

找到SERVER_PORT配置项，改变它的值即可。 当然也可以单一地启动某一模块服务：

```shell
./bin/start.sh -m {module_name}
```

- 一键取消所有服务

```shell
./bin/stop-all.sh
```

当然也可以单一地停止某一模块服务：

```shell
./bin/stop.sh -m {module_name}
```

###### 步骤6：查看服务（注意！注意！）

在Linux环境下使用JPS命令，查看是否出现DataXAdminApplication和DataXExecutorApplication进程，如果存在这表示项目运行成功

如果项目启动失败，请检查启动日志：modules/datax-admin/bin/console.out或者modules/datax-executor/bin/console.out

:::tip
脚本使用的都是bash指令集，如若使用sh调用脚本，可能会有未知的错误
:::

###### 步骤7：运行

部署完成后，在浏览器中输入 http://ip:port 就可以访问对应的主界面（ip为datax-admin部署所在服务器ip,port为为datax-admin 指定的运行端口）

输入用户名 admin 密码 123456 就可以直接访问系统

###### 步骤8：运行日志

部署完成之后，在modules/对应的项目/data/applogs下(用户也可以自己指定日志，修改application.yml 中的logpath地址即可)，用户可以根据此日志跟踪项目实际启动情况

如果执行器启动比admin快，执行器会连接失败，日志报"拒绝连接"的错误，一般是先启动admin,再启动executor,30秒之后会重连，如果成功请忽略这个异常。

##### 使用Docker 部署

```
构建镜像，包含Datax-web，datax，mysql,python,java
1.打包tool_datax_web
mvn clean install
打包后在build文件夹，解压后放入docker文件夹（datax-web文件夹命名datax-web-2.1.2）
2.打包datax
mvn -U clean package assembly:assembly '-Dmaven.test.skip=true'
打包后在target文件夹，解压后放入docker文件夹（datax文件夹命名datax）
3.构建镜像：docker build -t datax-web-hs:v1.0 .
4.运行并开放9527和3306端口
5.mysql配置，默认在容器中生成mysql，如需更改mysql数据源
  5.1编译前:datax-admin模块resources中更改application.yml，可配置Apollo，也可在bootstrap.properties中设置相关变量
  5.2编译后：这两个文件在datax-web-2.1.2/modules/datax-admin/conf目录下
```


#### 登录DataX Web

启动成功后，在浏览器地址输入`http://ip address:9527`。

如果在浏览器窗口中能看到以下登录界面，表示已经成功部署并启动DataX Web

![image-20220117144304659](/img/datax-web/image-20220117144304659.png)



### 快速入手

#### 创建项目

在使用DataX的过程中，项目就是工作空间的概念，每个用户可以创建多个属于自己的项目，后续构建的同步任务需要指定项目来管理。

- 创建：在左侧菜单栏点击项目管理，点击添加，填入项目名称和项目描述，点击确认，这样就创建好了一个工作空间。
- 编辑：在项目管理列表中，对每个属于用户的项目可以点击编辑按钮来修改项目的名称和描述。
- 删除：同样的，每个用户也有权限删除自己的项目。
- 导出：在现实的生产过程中，需要在不同的环境中复制离线数据同步任务，数据开发可以选择点击导出按钮，导出选择项目的所有任务配置信息，方便迁移，目前提供多种导出方式。
  
:::warning
删除项目会导致项目内的所有任务也一并删除
:::


#### 创建数据源

DataX是离线数据同步工具，因此支持异构数据源是核心功能点，目前DataX支持以下数据源的读写

|         数据源          |   读   |   写   | 增量模式 |
| :---------------------: | :----: | :----: | :------: |
|          Mysql          |   √    |   √    |    √     |
|         Oracle          | 未测试 | 未测试 |  未测试  |
| Postgresql（Greenplum） |   √    |   √    |    √     |
|        Sqlserver        | 未测试 | 未测试 |  未测试  |
|          Hive           |   √    |   √    |  未测试  |
|          Hbase          | 未测试 | 未测试 |  未测试  |
|         Mongodb         | 未测试 | 未测试 |  未测试  |
|      Elasticsearch      |   √    |   √    |    √     |
|       Clickhouse        | 未测试 | 未测试 |  未测试  |

- 创建：在左侧菜单栏点击数据源管理，点击添加，选择对应的数据源，按要求填写数据源连接配置。在测试连接通过后，点击确认。`这里会出现一个弹框，后面会讲到，可以点击确认。`

- 编辑：在数据源管理列表中，对每个用户创建的数据源可以进行修改参数

    - 自动替换：在实际开发过程中，DBA可能会改变数据源的地址或账号密码，由于DataX的数据源连接信息是保存在jobjson中的，所以不批量修改jobjson的话，会导致所有涉及到的任务都会报数据库连接错误。因此DataX提供根据数据源和任务的关联关系自动替换jobjson中的连接信息的功能，也就是上面所说的点击确认以后的弹框，点击确定后，会自动更新相关任务的信息。

      <img src="/img/datax-web/image-20220117152119388.png" alt="image-20220117152119388"  />
    
- 删除：用户有权限对自己创建的数据源进行删除，目前不会一并删除相关的任务。
  
:::danger
更新引用数据源的操作会重置相关任务的offset
:::

#### 配置任务

当创建完项目以及同步需要的源数据源和目标数据源的准备工作以后，接下来就是构建数据同步任务了。

在开始之前，得先知道DataX提供什么类型的任务，在实际开发中，可以根据不同的业务需求来选择要创建的任务类型。

- DataX任务
- Shell任务
- Powershell任务
- Python任务

##### 创建DataX任务模板

为了避免在构建任务的过程中重复工作，DataX提供任务模板功能。

- 创建：在左侧菜单栏点击任务管理 - DataX任务模板，点击添加，我们来介绍一下每个选项的含义。
    - 执行器：默认datax执行器。
    - 路由策略：当分布式部署Datax Web的情况下，任务执行的datax执行器节点的分配策略。
    - 阻塞处理：当任务并发阻塞或者重复提交调度时候的处理策略。
    - 任务类型：提供Datax、Shell、Powershell、Python四种任务类型。
    - 所属项目：该任务所属项目。
    - 子任务：子任务的概念是，当该任务完成以后，运行子任务，简单的串行任务流可以用该功能实现。
    - 增量模式：支持非增量模式和时间增量模式。
        - 时间增量模式
            - 增量开始时间：offset增量开始时间。
            - 增量时间字段：
                - lastTime：任务上次运行时间。
                - currentTime：当前时间-5min。
            - 增量时间格式：时间格式。
    - 任务描述：任务名称。
    - Cron：任务定时启动的Cron表达式。`DataX Web对引用模板自动创建任务的cron表达式做了削峰处理，默认同时刻启动的任务不超过10个`
    - 报警邮件：任务运行或者调度失败日志报警邮件方式。
    - 钉钉报警：任务运行或者调度失败日志报警钉钉发送方式。
    - 失败重试次数：任务失败重试次数。
    - 超时时间（分钟）：数据库session超时等待时间。
    - JVM启动参数：任务JVM运行参数。
- 编辑：修改任务模板参数。
- 删除：用户有权限删除自己创建的任务模板。

##### 任务构建

###### 步骤1：构建reader

1.选择数据库源

2.选择schema

3.选择对象类型（以实体表为例）

4.选择数据库表名

![image-20220120170804547](/img/datax-web/image-20220120170804547.png)

5.如果需要增量同步数据，则点增量时间字段的解析，如果增量时间字段不是sys_update_date则将where条件里的sys_update_date换成你对应的字段（注意时间格式需保持一致）



###### 步骤2：构建writer

- 选择数据库源

- 写入模式选择（默认upsert）

- 选择schema
- 填写数据库表名
- 点击下一步

![image-20220120172219160](/img/datax-web/image-20220120172219160.png)

![image-20220120172900054](/img/datax-web/image-20220120172900054.png)

:::warning
如果writer是Elasticsearch且index不存在则会跳出弹窗
:::

![image-20220120173009450](/img/datax-web/image-20220120173009450.png)

点击确定自动创建index和alias。alias和指定_id必须要填

###### 步骤3：字段映射

单机源端字段会全部选中

注意：如果源端字段和目标字段有字段没对齐，需核对两边字段对应关系，如需修改可在右侧移动字段位置和修改字段名称

![image-20220120173309161](/img/datax-web/image-20220120173309161.png)

###### 步骤4：构建

进入构建页面，先点构建会出现对应jobjson，然后点选择之前创建的DataX任务模板

![image-20220120173818786](/img/datax-web/image-20220120173818786.png)

一切正常的话，构建成功后会跳出任务构建成功，此时选择是否执行此任务，任务就构建成功了

![image-20220120173941300](/img/datax-web/image-20220120173941300.png)

##### 任务运行

任务已经配好，现在就要考虑如何对任务进行调度。目前Datax Web提供定时模式和手动模式。

###### 定时模式

每个配置成功的任务可以在任务管理中看到，初始的任务状态都是“停止”，这代表不进行定时调度，如果需要，可以单机定时开关显示“运行”，则该任务会根据任务的cron表达式定时启动。

###### 执行一次（手动模式）

手动模式执行任务需要在任务管理中找到想要执行的任务，点击操作 - 执行一次提交调度。



#### 日志管理

DataX Web提供可以实时查看的运行日志，我们有两个方式查看某个任务的日志

- 在任务管理中找到想要查看日志的任务，点击操作 - 历史日志（最新日志）即可查看。

- 在日志管理中搜索任务名进行查看。



#### 执行器管理

在集群部署方式下，对多个datax执行器节点进行分组标识，多用来解决longtime任务阻塞问题。

#### 用户管理

用户信息以及用户类型管理。

#### 资源监控

对多个datax执行器的系统运行监控。