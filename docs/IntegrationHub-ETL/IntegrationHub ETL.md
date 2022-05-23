---
sidebar_position: 1
sidebar_label: 'IntegrationHub ETL简介'
---

# IntegrationHub ETL

使用 IntegrationHub ETL 应用程序创建和管理ETL转换映射，该
映射将第三方数据集成到已有逻辑表。IntegrationHub ETL提供了
一个简化的用户界面，指导您完成端到端的集成过程，包括样本数据
的测试与导入。

![etllist.png](/img/IntegrationHub-ETL/etllist.png)


## IntegrationHub ETL and 导入集

使用IntegrationHub ETL和ETL转换映射具有以下优点：
使用导入集和变换映射：
- IntegrationHub ETL使用引导式设置，提供指导和简单的用户
整合第三方数据全流程的接口。
- IntegrationHub ETL包括一个使用新ETL的小型数据集的集成
测试变换视图。预览可让您在导入之前查看结果并调整配置设置。

### 数据源

数据源菜单中维护数据源信息，支持File类型(Excel格式)与Rest
类型。

![datasource.png](/img/IntegrationHub-ETL/datasource.png)

### ETL 变换

IntegrationHub ETL生成的输出。您可以将第三方数据集成到
逻辑表中，使用为相应集成配置的ETL转换映射。

### 转换

一种操作，您可以将其应用于特定的数据列以转换数据。例如，
转换数据值的格式。使用变换来标准化数据格式并满足其他系统要求。

### 转换后的数据

某些源数据可能不符合其目标的要求。在这些情况下，您可以应用
各种类型的转换，将数据映射到目标表和属性。例如可以转换数据
格式、替换值和连接来自多个数据列的值。
每个 CMDB 应用程序可以有多个连接来检索原始数据。一个cmdb
应用
程序可以有多个ETL变换映射，每个ETL变换映射只有单个数据源。

### 嵌套数据有效负载

要处理嵌套数据负载，您必须首先确保使用的数据源使用单列数据选
项进行设置。使用该设置，您可以正确表示 JSON 有效负载中的嵌套
数据，然后 IntegrationHub ETL处理嵌套数据，而不是平面数据。

```
{
"u_computer_fqdn": "computer2-fqdn", 
"u_computer_id": 2,
"u_computer_ip": "computer2-ip", 
"u_computer_location": "PDX", 
"u_computer_mac": "computer2-mac", 
"u_computer_name": "nested-payload-computer2",
 "u_computer_os": "computer2-os",
"interfaces": [
{
 "u_interface_ip": "computer2-eth1-ip",
 "u_interface_mac": "computer2-eth1-mac",
 "u_interface_name": "computer2-eth1", 
 "ip": ""
},
{
 "u_interface_ip": "computer2-eth2-ip",
 "u_interface_mac": "computer2-eth2-mac",
 "u_interface_name": "computer2-eth2", 
 "ip": {
    "u_ip_address": "computer2-eth2-ip",
     "u_mac_address": "computer2-eth2-mac"
    }
}
],
"software": [
{
 "u_software_name": "computer2-software2",
 "u_software_version": "computer2-software2-1.0",
 "instance": {
    "u_software_instance_name": "computer2-softwar
e1-instance"
    }
},
{
 "u_software_name": "computer2-software2",
 "u_software_version": "computer2-software2-2.0",
 "instance": {
    "u_software_instance_name": "computer2-softwar
e2-instance"
    }
}
]
},
```
您可以在 IntegrationHub ETL的单独面板中查看嵌套数据的层，
应用转换、映射并将该数据集成到逻辑表中。
创建嵌套数据 JSON 有效负载时，适用以下限制：
- 字段名称必须以字母（在 A-Z 或 a-z 之间）或以“_”开头，
并且只能包含字母（在 A-Z 或 a-z 之间）、数字 (0-9) 
或“_”字符。例如，字段名称不能包含特殊字符，如 *、[、]、#、
$、空格、和点。
- 字段名称不能是“temp”或“object”，它们是为内部使用而保留的。
- 在整个有效载荷中必须一致，您必须使用数组或对象来表示
  特定级别中的数据，与级别中的项目数无关。如果你使用一个
  一个对象中的多个项目的数组，您还必须使用一个数组来表示一个
  其他对象中的项目。
- 创建 ETL 转换映射
  IntegrationHub ETL 提供引导式设置，引导您完成
  为特定集成创建 ETL 转换映射的所有必要任务。




