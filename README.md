# kingdeehit-sharding-sphere
## 概述

ShardingSphere是一套开源的分布式数据库中间件解决方案组成的生态圈，它由Sharding-JDBC、Sharding-Proxy和Sharding-Sidecar（计划中）这3款相互独立的产品组成。
他们均提供标准化的数据分片、分布式事务和数据库治理功能，可适用于如Java同构、异构语言、容器、云原生等各种多样化的应用场景。

ShardingSphere定位为关系型数据库中间件，旨在充分合理地在分布式的场景下利用关系型数据库的计算和存储能力，而并非实现一个全新的关系型数据库。
它与NoSQL和NewSQL是并存而非互斥的关系。NoSQL和NewSQL作为新技术探索的前沿，放眼未来，拥抱变化，是非常值得推荐的。反之，也可以用另一种思路看待问题，放眼未来，关注不变的东西，进而抓住事物本质。
关系型数据库当今依然占有巨大市场，是各个公司核心业务的基石，未来也难于撼动，我们目前阶段更加关注在原有基础上的增量，而非颠覆。

ShardingSphere目前已经进入[Apache孵化器](http://incubator.apache.org/projects/shardingsphere.html)，
欢迎通过[shardingsphere的dev邮件列表](mailto:dev@shardingsphere.apache.org)与我们讨论。

__目前所有的可用发布都是进入Apache孵化器之前的发布版本，Apache官方发布将从4.0.0版本开始。__

![ShardingSphere Scope](https://shardingsphere.apache.org//document/current/img/shardingsphere-scope_cn.png)

### Sharding-JDBC

[![Maven Status](https://maven-badges.herokuapp.com/maven-central/org.apache.shardingsphere/sharding-jdbc/badge.svg)](https://mvnrepository.com/artifact/org.apache.shardingsphere/sharding-jdbc)

定位为轻量级Java框架，在Java的JDBC层提供的额外服务。
它使用客户端直连数据库，以jar包形式提供服务，无需额外部署和依赖，可理解为增强版的JDBC驱动，完全兼容JDBC和各种ORM框架。

* 适用于任何基于Java的ORM框架，如：JPA, Hibernate, Mybatis, Spring JDBC Template或直接使用JDBC。
* 基于任何第三方的数据库连接池，如：DBCP, C3P0, BoneCP, Druid, HikariCP等。
* 支持任意实现JDBC规范的数据库。目前支持MySQL，Oracle，SQLServer和PostgreSQL。

![Sharding-JDBC Architecture](https://shardingsphere.apache.org//document/current/img/sharding-jdbc-brief.png)

### Sharding-Proxy

[![Download](https://img.shields.io/badge/release-download-orange.svg)](https://www.apache.org/dyn/closer.cgi?path=incubator/shardingsphere/4.0.0-RC1/apache-shardingsphere-incubating-4.0.0-RC1-sharding-proxy-bin.tar.gz)
[![Docker Pulls](https://img.shields.io/docker/pulls/shardingsphere/sharding-proxy.svg)](https://store.docker.com/community/images/shardingsphere/sharding-proxy)

定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务端版本，用于完成对异构语言的支持。
目前先提供MySQL和PostgreSQL版本，它可以使用任何兼容MySQL和PostgreSQL协议的访问客户端(如：MySQL Command Client, MySQL Workbench, Navicat等)操作数据，对DBA更加友好。

* 向应用程序完全透明，可直接当做MySQL或PostgreSQL使用。
* 适用于任何兼容MySQL或PostgreSQL协议的的客户端。

![Sharding-Proxy Architecture](https://shardingsphere.apache.org//document/current/img/sharding-proxy-brief_v2.png)

### Sharding-Sidecar（TBD）

定位为Kubernetes或Mesos的云原生数据库代理，以DaemonSet的形式代理所有对数据库的访问。
通过无中心、零侵入的方案提供与数据库交互的的啮合层，即Database Mesh，又可称数据库网格。

Database Mesh的关注重点在于如何将分布式的数据访问应用与数据库有机串联起来，它更加关注的是交互，是将杂乱无章的应用与数据库之间的交互有效的梳理。
使用Database Mesh，访问数据库的应用和数据库终将形成一个巨大的网格体系，应用和数据库只需在网格体系中对号入座即可，它们都是被啮合层所治理的对象。

![Sharding-Sidecar Architecture](https://shardingsphere.apache.org//document/current/img/sharding-sidecar-brief_v2.png)

|           | *Sharding-JDBC* | *Sharding-Proxy* | *Sharding-Sidecar* |
| --------- | --------------- | ---------------- | ------------------ |
| 数据库     | 任意            | MySQL/PostgreSQL | MySQL/PostgreSQL   |
| 连接消耗数 | 高              | 低               | 高                  |
| 异构语言   | 仅Java          | 任意             | 任意                |
| 性能       | 损耗低          | 损耗略高          | 损耗低              |
| 无中心化   | 是              | 否               | 是                  |
| 静态入口   | 无              | 有               | 无                  |

### 混合架构

Sharding-JDBC采用无中心化架构，适用于Java开发的高性能的轻量级OLTP应用；Sharding-Proxy提供静态入口以及异构语言的支持，适用于OLAP应用以及对分片数据库进行管理和运维的场景。

ShardingSphere是多接入端共同组成的生态圈。
通过混合使用Sharding-JDBC和Sharding-Proxy，并采用同一注册中心统一配置分片策略，能够灵活的搭建适用于各种场景的应用系统，使得架构师更加自由的调整适合与当前业务的最佳系统架构。

![ShardingSphere Hybrid Architecture](https://shardingsphere.apache.org//document/current/img/shardingsphere-hybrid.png)

## 功能列表

### 数据分片

* 分库 & 分表
* 读写分离
* 分片策略定制化
* 无中心化分布式主键

### 分布式事务

* 标准化事务接口
* XA强一致事务
* 柔性事务

### 数据库治理

* 配置动态化
* 编排 & 治理
* 数据脱敏
* 可视化链路追踪
* 弹性伸缩(规划中)

## Roadmap

![Roadmap](https://shardingsphere.apache.org//document/current/img/shardingsphere-roadmap_cn.png)

## 如何构建

安装`maven`至您的开发环境并且执行命令：

```shell
mvn clean install -Prelease
``` 

构建产物：

```
sharding-distribution/sharding-jdbc-distribution/target/apache-shardingsphere-incubating-${latest.release.version}-sharding-jdbc-bin.tar.gz: Sharding-JDBC的二进制包
sharding-distribution/sharding-proxy-distribution/target/apache-shardingsphere-incubating-${latest.release.version}-sharding-proxy-bin.tar.gz: Sharding-Proxy的二进制包
sharding-distribution/shardingsphere-src-distribution/target/apache-shardingsphere-incubating-${latest.release.version}-src.zip: ShardingSphere的源码包
```
