---
sidebar_position: 1
sidebar_label: '表单设计器'
---
  让我们从创建virtual war room(应用菜单里用于测试的应用）记录开始。按照以下步骤在我们的应用程序中创建一条记录，我们可以使用它来创建并使用表单设计器修改视图：
1. 在应用程序导航器过滤器文本框中，键入Virtual War Room，您将看到 Virtual War Room 应用程序标题以及Virtual War Rooms模块。
2. 单击该模块，然后单击列表顶部的**新增按钮**，以查看表单并创建新记录。
3. 在**Assigned to**字段中输入Beth Anglin(这是开发实例中包含演示数据的随机默认用户)。
4. 单击配置项字段旁边的放大镜，然后从内置演示数据中选择任意配置项。
5. 在表单里输入一些测试内容，例如为virtual war room应用输入一些描述内容。this is a test ticket
6. 点击**提交**。这将使您返回到列表视图。
7. 单击Virtual War Room您刚刚创建的工单，返回表单。

  您将在该表单上看到的所有字段实际上都是从任务表派生的字段。事实上，除了基本系统字段（**Created**、**Created by**、**Updated**、**Updated by**和其他一些字段）和我们新创建Major incident的字段之外，所有其他字段都是从**Task**表派生的。

  您会在此表单上注意到许多字段和字段类型，包括但不限于以下内容：

- **Number**字段，为字符串类型。
- **assign to**字段，它是一种引用类型（指向用户表）。
- **short description**字段，它是跨越两个布局列的字符串类型。
  
  **笔记**  

  **short description**和**description**字段之间的区别在于字段字典条目的最大长度值。如果字符串字段的最大长度为255 个字符或更少，它将显示为单行或小字段。但是，如果其最大长度为256或更大，它将是表单上的多行字段。

- **Active**字段，它是true / false (AKA Boolean ) 类型。
- **status**字段，其中是一个带有下拉选择列表的整数字段。

  笔记  

  虽然**State**字段在技术上是一个**整数**类型（意味着数据库中的实际值是一个整数），但可以选择的选项实际上存储在另一个表中，标记为选项（名称为sys_choice）。该表的记录与表本身一样，既有标签又有值。标签是您在**State**字段下拉列表中看到的内容（**Open**、**Closed Complete**、**Work in Progress**等），但值是实际存储在字段和数据库中的内容。
  
  对于virtual war room，我们不需要所有字段 - 但这个表单甚至没有显示我们的Major incident字段！让我们按照以下步骤解决这个问题：

1. 右键单击表单标题，然后转到**配置| 表单设计**。这将打开表单设计器，这是 Jet 中的一个方便的小应用程序。
2. 由于我们希望virtual war room仅针对具有最高优先级的**Major incident**而存在，因此我们实际上并不需要它本身的**Priority**字段。继续并单击**Priority**字段右侧的小x图标，然后将鼠标悬停在**Priority**字段上。
3. 使用相同的方法删除**Active**字段，以及**Parent**、**Short description**和**Description**。虽然删除了**Short description**和**Description**，但我们依然希望在表单上有一个简短的描述和描述，我们会从**Major incident**中获取.
4. 现在让我们将我们的**Major incident**字段添加到表单中。过滤左侧的字段列表以找到它，然后将**Major incident**字段向右拖动，将其放在**State**的正下方。
5. 最后，在底部的**Work Notes**下添加Activities(filterd)(Formatter) 。这将显示您的日志。章节“任务和工作流”中有关此内容和活动的更多信息。
  
  此时，您的**表单设计**窗口应如下所示：
    

  ![表单设计图](/img/forms/form-designer.jpeg)
  
  继续并单击页面右上角的绿色保存按钮，然后返回到您之前所在的选项卡。右键单击标题，然后重新加载表单，您应该会看到设计精美的新表单。做得好！




 


