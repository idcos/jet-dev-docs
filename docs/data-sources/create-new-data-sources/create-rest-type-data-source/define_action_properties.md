---
sidebar_position: 1
sidebar_label: '动作属性定义'
title: '动作属性定义'
---
在Integration Hub中创建和配置数据源请求操作。在Integration Hub中发布请求操作后，您可以在数据源记录中选择它作为**请求操作**。

## 在开始之前
你所需要的角色：import_admin

## 步骤
1. 菜单栏 > 系统导入集成 > 数据源

![etl](/img/data-sources/datasource_new_1.png)

2. 点击**新建**

![etl](/img/data-sources/datasource_new_2.png)

3. 选择**类型** 为 **REST (IntegrationHub)**

4. 点击 **请求操作**的跳转链接

![etl](/img/data-sources/datasource_rest.png)

5. 填写完成表单

![etl](/img/data-sources/flowdesigner_new.png)

| 字段 | 描述 |
| :-------------:| :------------- |
| 名称 | 请求操作的名称。
| 应用 | 应用范围。
| 来自 | 可访问请求操作的范围。
| 保护 | 指定请求操作是否为只读。
| 类别 | 请求操作的类别。
| 描述 | 请求操作的描述。
| 注解 | 您希望在 “数据源请求操作”页面上的“操作”标题下显示的文本。

6. 点击**提交**

7. 出现数据源请求操作页面，其中包含以下字段

![etl](/img/data-sources/flowdesigner_action_input.png)

| 字段 | 描述 |
| :-------------:| :------------- |
| 限制 | 用户在 REST (IntegrationHub) 数据源记录中设置的限制。如果 REST API 支持分页并且 在数据源记录中选择了**支持分页**，则您必须将此输入提供给您的 REST 步骤。
| 偏移量 | 用户在 REST (IntegrationHub) 数据源记录中设置的偏移量。如果 REST API 支持分页并且 在数据源记录中选择了**支持分页**，则您必须将此输入提供给您的 REST 步骤。
| 最后成功导入时间 | 上次成功导入数据。一个数值，表示自 1970 年 1 月 1 日 00:00:00 GMT 以来的毫秒数。数据源填充此值。如果 REST API 支持基于时间的查询，您必须将此输入提供给您的 REST 步骤。
| 导入开始时间 | 导入实际开始的时间。一个数值，表示自 1970 年 1 月 1 日 00:00:00 GMT 以来的毫秒数。数据源填充此值。如果 REST API 支持基于时间的查询，您必须将此输入提供给您的 REST 步骤。

8. 如果需要，您可以在执行 REST 步骤之前添加一个脚本步骤来验证或修改操作输入、设置默认值或构建请求负载。

    a. 从动作概要中选择**数据源请求**。

    b. 选择**启用处理脚本**。

    c. 从动作概要中选择**脚本步骤**。

    b. 如脚本步骤中所述配置[脚本步骤](/docs/intro)。

9. 从动作概要中选择REST 步骤。

10. 按照REST步骤中的说明配置[REST步骤](/docs/data-sources/create-new-data-sources/create-rest-type-data-source/rest_step)
![etl](/img/data-sources/flowdesigner_action_reststep.png)

11. 可以从以下选项进行选择和操作
    * 单击**属性**返回到操作属性窗口。
    * 单击**测试**以在发布之前测试请求操作。
    * 单击**执行**以查看请求操作的执行历史记录。
    * 单击**保存**以保存请求操作。
    * 单击**发布**以发布请求操作并使其可供使用。  