---
sidebar_position: 13
sidebar_label: '数据源'
title: '数据源简介'
---
数据源用于创建导入集，以便在将数据映射到生产表之前（如有必要）进行处理。

## 数据源类型

| 类型 | 描述 |
| :------------- | :---------- |
| 文件|数据采用可识别的文件格式，可通过多种文件检索方法在本地或远程访问。
|JDBC | 数据在数据库中，可以使用 JDBC 访问。客户服务和支持支持 Oracle、MySQL、Sybase、DB2 Universal 和 MS SQL Server 驱动程序。
|LDAP | 数据位于 LDAP 服务器中，可分别通过 LDAP 或 LDAPS 端口 389 和 636 访问。
|OIDC | 数据可通过 OIDC 访问。
| REST | 数据位于 REST API 中，可通过Integration Hub访问。
| 数据流集成中心 |  数据从数据流操作加载，可通过集成中心访问。
| 自定义（通过脚本加载）| 使用自定义脚本获取数据。

ps: 其中文件类型、REST类型已实现
