---
sidebar_position: 1
sidebar_label: '一对多关系'
---
一对多关系是一种父子关系。它们由一个父记录组成，该记录链接到许多子记录。此链接是使用数据库表键完成的。  


正如我们在前一章中简要提到的，记录有一个主键（PK），和外键(FK) 列；Jet平台中的 PK 是Sys ID [ sys_id] 列。Jet平台中的每条记录都有一个 Sys ID，它通常在整个数据库中是唯一的（尽管从技术上讲，主键只需要在表中是唯一的）。FK 列的一个示例是用于保存另一条记录的 PK 的任何列。这些字段是Jet平台中的参考字段。例如，Incident [ incident] 表包含一个带有标签Assigned to的 FK 列，以及一个实际的列名assigned_to。这是一个指向User [ sys_user] 表的引用字段，并包含该表中的一条记录的 PK（系统 ID）。引用字段只能引用一个表中的记录，在该字段的字典记录中指定。  


**笔记**  

当您查看表单上的这些 FK（引用）字段之一时，您将看到它包含的 Sys ID 对应的记录的显示值。显示值是display_value属性设置为true的列中的值。例如，在Incident表（以及任何其他扩展Task表的表）中，默认显示值为Number列。  


Assign to字段还有助于说明一对多关系的性质。正如您可能想象的那样，每个事件一次只能分配给一个用户（一次只能将一个 Sys ID 值放入参考字段！）但是，一个用户可能是许多事件的受让人。因此，关系是一（用户）对多（事件），而不是相反。  


为了更好地了解这种关系是如何运作的，让我们探索一个想象中的鞋店的数据库：Lou's Shoes。  

| Name(PK) | Shoe size | Preferred material |
| :----:| :----: | :----: |
| Larry Lopez | 11 Men's | Leather |
| Sally Stewart | 7 Women's | Suede |
| Molly Mae | 8.5 Women's | Microfiber |
  
  接下来，假设我们有一个产品表，如下所示：  
  
| item(PK) | Color | Material |
| :----:| :----: | :----: |
| Lavender Leather Loafer | Purple | Leather |
| Scarlet Suede Stiletto | Red | Suede |
| Mahogany Microfiber Moccasin | Brown | Microfiber |




