---
sidebar_position: 1
---

# 简介
JET平台是面向开发者的平台，它借助强大的工作流引擎，从快速配置到高级编码，实现对服务编排和企业系统搭建。
JET平台能力，主要包含以下六个方面：<br>
**模型驱动**<br>
**脚本语言**<br>
**表达式语言**<br>
**可视化开发**<br>
**软件工程**<br>
**开放集成**<br>

## 模型驱动
现在低代码平台主要分为两大类：基于表达驱动和基于模型驱动；JET低代码平台是基于模型驱动低代码平台。现在看一下这
两类低代码平台本质区别。<br>
![基于表单驱动视图：](/img/intro/intro-1.png)<br>
![基于模型驱动低代码视图](/img/intro/intro-2.png)<br>

对业务进行抽象分为逻辑表**table**和逻辑字段**Field**,它们可以类似关系型数据库的物理表和字段，但是它们更主要
的是对业务层逻辑抽象。一个数据库包含一个或者多个逻辑表，一个逻辑表包含零个或者多个逻辑字段。JET平台抽象很多逻辑Field。<br>
![逻辑字段：](/img/intro/intro-3.png)

JET平台沉淀很多业务领域模型：用户领域、事件领域、HR领域、CMDB领域、ITAM领域、Test等领域模型;可以基于基础模型进行扩展，消除信息孤岛。
下面是![平台模型展示图](/img/intro/intro-4.png)

## 脚本语言
JET平台选择JavaScript为脚本语言，使用**Rhino**作为服务端脚本解析器。JavaScript支持client/server端开发，两端语言一致，
这样前后端风格就可以保持一致，学习成本更低。

## 表达式语言
JET平台表达式语言，分为Client-Side和Server-Side：
![核心的方法](/img/intro/intro-5.png)<br>
其中GlideRecord封装对数据库操作。
![GlideRecord](/img/intro/GlideRecord.png)

## 界面操作
主要针对的是在表单和列表上添加按钮、链接和上下文菜单项，使UI更具交互性、可自定义性，并且满足特定的用户活动。
![界面操作](/img/intro/intro-6.png)

## 界面策略
控制表单 只读，可见，清空，必填，设置值操作。
![界面策略](/img/intro/intro-7.png)

## 客户端脚本
当表单加载，表单提交，或者字段值变更，list单元格编辑等事件发生的时候，执行一段脚本。
![客户端脚本](/img/intro/intro-8.png)

## 业务规则
显示、插入、更新或删除记录时或查询表时运行的服务器端脚本。
![业务规则](/img/intro/intro-9.png)

## 应用设计 
JET平台提供统一应用构建入口Studio,统一的应用文件查询，修改等管理等入口
![studio](/img/intro/studio.png)

## 工作流引擎
提供了一个拖放界面，用于跨平台，复杂业务流程处理。
![工作流](/img/intro/intro-workflow.png)

## 表单设计器
提供了一个拖放界面，自定义不同视图的样式。
![表单设计器](/img/intro/intro-formdegin.png)

## 开发者协同
Studio与GitHub存储库集成，以管理应用程序开发过程的源代码控制。实现了开发者之间协同，到应用的版本管理，不同环境迁移全线贯通。
![开发者协同](/img/intro/intro-github.png)

## 脚本调试
JavaScript日志&Field Watcher: 可以监控页面日志，方便开发和错误排查。
![javascript](/img/intro/javaScript-debug.png)

## IntegrateHub-ETL
提供了 一个简化的用户界面，指导您完成端到端的数据集成、清洗、转化、映射到JET平台，包括样本数据 的测试与导入。
![IntegrateHub-ETL](/img/intro/IntegrateHub-ETL.png)
