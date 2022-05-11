---
sidebar_position: 1
sidebar_label: 'client-scripts'
---
Client scripts不是客户端脚本的唯一示例，但它们可能是最普遍的（因此得名）。客户端脚本是在客户端执行的，是从服务器发送到用户浏览器的 JavaScript 片段，

请记住，只要界面策略可以有效地重现您在客户端的所需功能，最好使用界面策略，当然，更高级的功能通常需要使用更高级的条件或功能，这就是client scripts的用武之地！

作为一般规则，一个写得很好的客户端脚本将非常快速地将浏览器的控制权返回给用户，并异步执行任何服务器查找。这包括从服务器查找属性、执行 GlideRecord 查询和处理时区。

任何服务器查询操作必须异步完成，但onSubmit脚本是一个例外。onSubmit脚本是一种客户端脚本，当用户单击表单上的Save、Update或Submit按钮时执行。异步操作是异步执行的，这意味着浏览器不会等待它们完成，当表单提交时页面会重新加载，因此表单可以在回调函数被调用之前提交！为此，onSubmit客户端脚本只能同步运行。作为一般规则，所有其他需要与服务器通信的客户端脚本应该异步运行。

客户端脚本，如界面策略和Client scripts，仅在表单内执行（onCellEdit除外，它在列表视图上执行）。这意味着，如果您使用客户端方法来控制数据，例如client scripts或界面策略，则必须使用另一种方法来保护该数据不被列表视图中的修改。您可以通过使用 ACL（安全规则）、数据策略、业务规则（使用当前对象的setAbortAction()方法）或完全禁用列表编辑来完成此操作。

就像在业务规则和其他服务器端脚本中一样，您可以使用g_form客户端 API 从客户端脚本中访问表单上的变量，方法是在字符串参数中像这样：
```
g_form.setValue('variables.var_name', 'var value')
```
这同样适用于其他g_form方法，例如setDisplay()/ setVisible()、setReadOnly()和setMandatory()。