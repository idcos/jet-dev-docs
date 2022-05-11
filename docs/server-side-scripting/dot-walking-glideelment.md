---
sidebar_position: 1
sidebar_label: 'dot-walking-glideelment.'
---

dot-walking和 GlideElement
正如我们在前一章中学到的，dot遍历允许您通过引用字段（一种包含对另一条记录的引用的字段类型）访问相关记录上的字段，方法是链接以点分隔的字段名称。例如，要从Incident表上的业务规则中获取分配事件的人员的电子邮件地址，您可以使用以下代码：

var assigneeEmail = current.assigned_to.email.toString();
您可能会注意到我们也使用toString()了前面代码段中的方法。这是因为，正如我们在服务器端 Glide API 文档中看到的，从服务器端访问的字段GlideRecords返回GlideElement对象；不仅仅是价值观。JavaScript 通常会强制（有时称为强制转换）值到您尝试使用它的任何数据类型，但是最好显式地转换从GlideElement对象派生的值。否则，您可能会得到错误的数据类型。由于 GlideElement 对象是…… objects，您最终可能会引用该对象，这可能会导致意外且极难解决的问题。

当不进行点遍历时，您可以使用该getValue()方法从字段中显式获取字符串值，如下行所示：

var assigneeSysId = current.getValue('assigned_to');
点行走可以被链接，这意味着通过点遍历到引用字段，您可以继续遍历它以访问该字段中引用的表上的字段。例如，要从业务规则中获取分配给事件的人员的经理的电子邮件地址，我们可以使用以下代码：

var managerEmail = current.assigned_to.manager.email.toString();
如上所示，dot-walk 链中的最大推荐步数为 3。要进一步进行 dot-walk，请考虑使用.getRefRecord()API 来获取与引用记录对应的 GlideRecord 对象。例如：

var grAssignee = current.assigned_to.getRefRecord();
var grAssigneeManager = grAssignee.manager.getRefRecord();
var managerEmail = grAssigneeManager.getValue('email');
对于三个或更少的点遍历步骤，使用 可能效率较低getRefRecord()，因为它返回整个记录，但对于更长的点遍历，它可能非常有用。

点遍历不仅用于在业务规则和脚本中获取子记录字段值，还包括；它对于在脚本和条件构建器中构建查询也很有用！

为了直观地说明点移动，导航到Incident表，打开列表顶部的查询构建器。

在 List v2 中，您可能需要先打开列下拉菜单，然后选择Show Related Fields；之后，列表中的引用字段后跟一个点可移动引用。

dot-walking和 GlideElement
但是在列表 v3 中，点遍历在查询构建器中更简单，并且更清楚地说明了如何在查询中使用点遍历。只需单击引用字段名称右侧的点行箭头，您就会看到该引用字段引用的表上的字段列表。

dot-walking和 GlideElement
例如，Assigned [ assigned_to] 字段引用了Users [ sys_user] 表。单击此处查询构建器中的点行箭头，允许我们选择要过滤的引用表中的字段值。

dot-walking和 GlideElement
点遍历脚本中的引用字段，为了访问被引用记录上的值，通常是可能的，因为 GlideElement 对象可用于服务器端脚本。在查询中可以进行点遍历，因为查询条件是在服务器端评估的。这也意味着您可以访问服务器端 Glide API 以在查询中执行一些有限的脚本（只要您在任何要作为查询的一部分执行的代码前加上关键字javascript:。例如，以下查询将仅显示 Incident分配给您的票——或者更具体地说，分配给使用此查询加载列表的任何用户：

dot-walking和 GlideElement
即使我们调用的 GlideSystem ( ) 对象是服务器端 API，这也是可能的，因为脚本实际上是在服务器上评估的，就在应用查询之前。gs

如果我们使用相同的过滤器作为脚本GlideRecord查询的一部分，它看起来像这样：

```javascript
var gr = new GlideRecord('incident'); 
gr.addQuery('assigned_to.sys_id', gs.getUserID()); 
gr.query(); 
while (gr.next()) { 
    //do something 
} 
```

正如我们所讨论的，dot-walking在几乎任何执行服务器端的脚本中都是可能的；这包括邮件脚本、引用限定符和查询过滤器！但是，重要的是要知道，为了成功进行点遍历，用户必须能够通过安全规则访问目标表和字段（被dot遍历的那个）。