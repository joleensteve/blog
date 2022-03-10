#### 我们为什么需要日志采集系统

如果我们只部署一个服务，当然就不需要日志采集系统，Aop 或 Log4j 就可以，定位错误日志还是很方便的。但如果我们需要在多个服务器上部署多个服务，链路调用又比较复杂，那么服务故障后将难以定位，运维同学需要不断切换服务器查询不同的日志，想想就是一个灾难级的工作量。这也是我们所面临的问题，一套成熟的日志收集方案，可方便我们快速定位问题，解决BUG。

目前市场上成熟的方案有两种 ELK 和 PLG，PLG 属于后起之秀，主要解决 ELK 过于 `重` 的问题。

##### 一、什么是 ELK ？

ELK 是 Elasticsearch、Logstash、Kibana 的首字母大写简称；

1. `Elasticsearch` 是实时全文搜索和分析引擎，提供搜集、分析、存储数据三大功能。
2. `Logstash` 是一个用来搜集、分析、过滤日志的工具，目前多用 Fluentd 和 Filebeats 作为常用的日志收集方案，故有 EFK 称呼。
3. `Kibana` 是一个基于 Web 的图形界面，用于搜索、分析和可视化存储在 Elasticsearch 指标中的日志数据

架构如下图所示：

![1271254-20200619151852578-1536645590](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/1271254-20200619151852578-1536645590.png)

##### 二、什么是 PLG ？

PLG 是 Promtail、Loki、Grafana 的首字母大写简称；
1. `Loki` 是一个可以水平扩展、高可用以及支持多租户的日志聚合系统，核心思想是将标签添加到日志流中而不是构建全文索引。
2. `Promtail` 是用来将容器日志发送到 Loki 或者 Grafana 服务上的日志收集工具，该工具主要包括发现采集目标以及给日志流添加上 Label 标签，然后发送给 Loki，另外 Promtail 的服务发现是基于 Prometheus 的服务发现机制实现的，对标 Logstash。
3. `Grafana` 是一个用于监控和可视化观测的开源平台，支持非常丰富的数据源，在 Loki 技术栈中它专门用来展示来自 Prometheus 和 Loki 等数据源的时间序列数据。此外，还允许我们进行查询、可视化、报警等操作，可以用于创建、探索和共享数据 Dashboard，鼓励数据驱动的文化，对标 Kibana。

架构如下图所示：

![ecc7ef5975258b6bdda790a75705d6e2](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/ecc7ef5975258b6bdda790a75705d6e2.png)

##### 三、ELK 和 PLG 对比分析

1. ELK 优点是功能丰富，允许复杂的操作。但是，这些方案往往规模复杂，资源占用高，操作苦难。很多功能往往用不上，大多数查询只关注一定时间范围和一些简单的参数（如host、service等），使用这些解决方案就有点杀鸡用牛刀的感觉了。
2. ELK 绝对算是重量级应用，ELK 要能跑起来，基本要求：ElasticSearch-2G内存，Logstash-1G内存，Kibana-一两百M 而 PLG 则省心得多，三个加起来有个一两百M就能跑了。
3. Loki 更加节省成本：操作大型索引的成本和复杂度很高，而且通常是固定的，无论是否在查询它，你都要一天24小时为它付费。而 Loki 在这种较小的索引和并行查询与较大/较快的全文索引之间做出了权衡，你可以决定你想拥有多大的查询能力，而且你可以按需变更。查询性能成为你想花多少钱的函数。同时数据被大量压缩并存储在低成本的对象存储中，比如 S3 和 GCS。这就将固定的运营成本降到了最低，同时还能提供难以置信的快速查询能力。
4. Loki 不会对原始的日志数据进行全文索引，而是采用了 Prometheus 的标签思想，只对标签索引，日志数据本身也会被压缩，并以chunks（块）的形式存储在对象存储或者本地文件系统。
5. Loki 能够以微服务模式运行，也就是把自身的各个模块分解出单个进程运行。比如，通过横向扩展多个查询器（Querier）模块可提高查询性能。
6. 我们的服务处于早期，并不需要花过多的成本去部署 ELK。

**总结：通过以上对比分析，最终采用 PLG 日志采集分析方案。并且可以将服务监控融于其中一起解决**

#### PLG 安装和使用（仅适合本地测试）

想了解一个东西，莫过于上手试用下，让我们使用 Docker 快速部署一个能基本使用的 PLG 服务。

##### 使用 Docker 部署 PLG 服务

Ps: 本文档默认你熟悉 Docker 的基本使用，并安装启动了 Docker 服务。

首先通过以下命令找到官方提供的 `docker-compose.yaml` demo 文件。你也可以不 clone 官方库，直接复制下一步的示例代码, 新建一个 docker-compose.yaml 文件直接粘贴进去即可。

```bash
# 打开终端 克隆 Loki 官方 Github 库
git clone https://github.com/grafana/loki.git
# 进入 production 目录并查看 docker-compose.yaml 文件
cd loki/production/
cat docker-compose.yaml
```

`docker-compose.yaml` 的代码展示。

```yaml
version: "3"

networks:
  loki:

services:
  loki:
    image: grafana/loki:2.4.2
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml
    networks:
      - loki

  promtail:
    image: grafana/promtail:2.4.2
    volumes:
      - /var/log:/var/log
    command: -config.file=/etc/promtail/config.yml
    networks:
      - loki

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    networks:
      - loki
```

打开终端进入 `docker-compose.yaml` 文件所在目录

```bash
# 默认使用当前目录下面的 docker-compose.yaml 文件创建容器
docker-compose up -d
```

执行完毕后，查看以创建的容器

```bash
docker-compose ps
     Name                    Command               State           Ports
---------------------------------------------------------------------------------
loki_grafana_1    /run.sh                          Up      0.0.0.0:3000->3000/tcp
loki_loki_1       /usr/bin/loki -config.file ...   Up      0.0.0.0:3100->3100/tcp
loki_promtail_1   /usr/bin/promtail -config. ...   Up
```

确认三个容器的 state 都处于 Up 状态，恭喜你已经部署成功了

##### 基本使用（让我们来看下我们部署了个啥。。。

打开浏览器输入以下网址，进入 Grafana 登录界面。

http://localhost:3000

默认账号密码都是 admin，输入后点击 `Log in` 登录，首次登录需要修改密码，按照提示来即可。

![image-20220308113616743](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/image-20220308113616743.png)

然后点击上图蓝框的部分，进入添加数据源页面，选择 Loki 进入配置页面

![image-20220308113820782](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/image-20220308113820782.png)

![image-20220308114146388](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/image-20220308114146388.png)

这里我们只需要修改 HTTP 下的 URL 配置即可，URL 值为 http://loki:3100

然后点击下面的 `save & test` 按钮

再点击 `1` 箭头的按钮进入日志查询页面，`2` 按钮选择 Loki 数据源

![image-20220308114917468](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/image-20220308114917468.png)

`3` 这部分是查询交互部分，你可以直接输入 查询的语法，也可以点击 `3` 按钮选择要查询的日志

![image-20220308115557241](https://cdn.jsdelivr.net/gh/joleensteve/images/blog/image-20220308115557241.png)

ok 安装使用就介绍到这一步，你可以随便点点看看。随后将介绍 loki 的架构设计、 查询语法、标签规则等。



文档参考：

loki，promtail详细介绍：https://www.qikqiak.com/k8strain2/logging/loki/overview/
Spring Boot 2 实战：轻量级日志loki集成1：https://felord.cn/loki.html
Spring Boot 2 实战：轻量级日志loki集成2：https://felord.cn/promtail-custom.html
简明教程：https://www.qikqiak.com/tags/loki/

