---
sidebar_position: 6
sidebar_label: 'converting-data-ui-policies.'
---
# 6.数据策略和界面策略之间转换（Converting a data policy to a UI policy）

通常我们可以将界面策略转换为数据策略，反之亦然。在界面策略表单中，您会发现一个名为**Convert this to Data Policy**的 UI 操作，在数据策略表单中，会发现一个名为**Convert this to UI Policy**的UI操作。

仅当满足以下条件时，才会显示将界面策略转换为数据策略的此 UI 操作：

1. 不得选中界面策略表单的**高级视图**上的**运行脚本复选框。**这是因为数据策略不支持脚本，因此无法将脚本化的 界面策略转换为数据策略。
2. **必须选中全局**复选框。
3. 与 界面策略关联的任何界面策略操作都不得影响字段的可见性。这是因为无法使用数据策略修改字段的可见性。

如果前面满足条件，您将看到将其**转换为数据策略**UI 操作在表格的**相关链接**部分下：

![在数据和 界面策略之间转换](/img/ui-data-policies/converting-data-ui-policies.assets/00141.jpeg)

单击此链接将自动生成新的数据策略并带您进入显示新记录的表单。在底部，您还将在相关列表中看到自动生成的数据策略规则。