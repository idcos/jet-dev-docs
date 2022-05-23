---
sidebar_position: 3
sidebar_label: '任务表的继承表'
---
&emsp;&emsp;任务表是非常强大的基表，但有趣的是，不应该直接在Task [ task] 表上创建任何记录。相反，任务记录是在子表上创建的；扩展表的task表。事实上，如果您尝试直接从task表中创建新任务，您会发现您被重定向到任务拦截器，这消除了您想要创建的任务类型的歧义：

![tasks-table](/img/tasks-workflows/tasks-table.png)

### 提示

&emsp;&emsp;拦截器将特定页面（在这种情况下，基本任务[ task] 表上的新表单页面）替换为另一个（在这种情况下，链接到各种其他任务类型的消歧页面）。

&emsp;&emsp;到扩展任务表并继承任何相关字段和业务逻辑，首先导航到系统定义表。在表列表中，单击左上角的新建。
由于我们已经创建了Virtual War Rooms表（它扩展了Task表），让我们创建一个也扩展Task表的子表。我们将其命名为War Room Task，因此将其输入到New table 表单的Label字段中，然后按Tab。名称字段将自动填充适当的名称：u_war_room_task

### 提示

&emsp;&emsp;输入新表记录的标签字段后按Tab取消选择标签字段。修改字段然后离开它（例如将光标移动到新字段）称为更改事件。更改事件可以触发onChange客户端脚本。在这种情况下，这个表单上已经运行了一个 onChange 客户端脚本，它会自动填充表的Name字段。

&emsp;&emsp;在Add module to menu字段中，选择我们之前创建的Virtual War Room应用程序菜单，然后然后单击扩展表字段输入框旁边的放大镜图标。搜索 text task，并确保选择正确的Task [ task] 表，如以下屏幕截图所示：

![tasks-table2](/img/tasks-workflows/tasks-table_2.png)

&emsp;&emsp;右键单击New Table表单标题，然后单击Save。这将显示从系统和表继承的表列task。
接下来，转到Controls form 部分，并选中Live feed和Auto-number复选框。这将允许用户在作战室任务表中看到活动的实时提要，并为每个新的作战室任务自动生成下一个作战室任务编号。
选中自动编号复选框后，将显示几个新字段。在Prefix字段中，您可能会注意到默认情况下会填充 WAR。这意味着每个作战室任务编号都将以 WAR 开头，后跟 7 个数字。由于这是我们为Virtual War Room记录选择的相同前缀，所以让我们选择不同的前缀。我选择了 WRT，但您可以轻松地使用四个字母甚至更多。
最后，更换来自用户角色字段的值，带有ITIL。这样，任何 ITIL 用户都可以访问作战室任务表。完成后，右键单击标题，然后再次单击保存。恭喜，您刚刚创建了一个扩展表的task表！这意味着我们的新表将具有Active和Assigned to等字段，以及业务逻辑，例如在我们表中的记录关闭后自动停用。

&emsp;&emsp;任务不应该被等待执行；它们的目的是在整个生命周期中取得进展。该生命周期可能包括分配、批准和手工工作；或者它可能是完全自动化的！


