---
sidebar_position: 3
sidebar_label: 'JET中的示例'
---

# JET中的示例

## 在form design中渲染formatter
### 渲染效果
应用于表 **sn_test_management_test_version**
![image.png](/img/formatter/example/test-step-render-result.gif)
### formatter定义
名称为 **Test Version Steps **，格式字段使用了 **test_version_steps** 的macro
![image.png](/img/formatter/example/test-step-formatter.png)
### macro定义
主要UI宏为 **test_version_steps** ，内嵌的子UI宏有 **test_version_steps_template** 、 **test_version_step_list**

![image.png](/img/formatter/example/test_version_steps_macro.png)
![image.png](/img/formatter/example/test_version_steps_template_macro.png)
![image.png](/img/formatter/example/test_version_step_list_macro.png)
## 以column_renderer形式在表格中渲染formatter
### 渲染效果
![image.png](/img/formatter/example/column_renderer.png)
## 基于Flow Formatters和form design渲染formatter
### 渲染效果 
应用于表 u_test_flow
![image.png](/img/formatter/example/flow-formatters-render.png)
### 在 sys_process_flow 表中添加 flow节点

- Table：u_test_flow
- Condition： flow节点满足为当前节点的条件
- Order：节点排序状态

![image.png](/img/formatter/example/add-flow-node.png)
### 添加完sys_process_flow之后会自动生成一个process flow的formatter
![image.png](/img/formatter/example/auto-gen-formatter.png)
![image.png](/img/formatter/example/auto-gen-formatter-record.png)
### 对应的macro
![image.png](/img/formatter/example/process-flow-macro.png)
