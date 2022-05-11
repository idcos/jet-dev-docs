---
sidebar_position: 2
sidebar_label: 'Journals-activity-formatter'
---
期刊领域技术不要将数据存储在Journal字段所在的表中，而是在Journal Entries [ sys_journal_field] 表中创建条目。这些日记帐分录以及与Sys Audits [ sys_audit]、History [ sys_history_line] 和Record History [ sys_history_set] 表中的已审计字段更改相对应的其他值一起显示在活动格式化程序中。

### 提示

日记帐字段仅适用于已审核的表。添加这样的字段将修改未审计表的系统字典条目，使其成为审计。

您可以将三种类型的日记帐字段添加到表单中：journal_list、journal和journal_input。

journal_list字段实际上并不存储数据，但更像是格式化程序。格式化程序是在 ServiceNow 表单中显示数据或其他内容（常规表单字段除外）的一种方式。如果您访问某个journal_list字段的字典记录（例如在创建它时），您可以使用字典表单上的高级视图UI 操作导航到高级视图。在高级视图中，您将看到一个从属字段表单部分，其中包含一个使用从属字段复选框。选中此框将显示“依赖于”字段字段。使用此方法设置journal_list依赖于journal或的字段journal_input字段，将导致输入到所选字段的数据显示在journal_list字段中。