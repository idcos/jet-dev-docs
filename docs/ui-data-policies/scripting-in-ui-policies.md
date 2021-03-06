---
sidebar_position: 3
sidebar_label: '界面策略脚本'
---
# 界面策略脚本

界面策略条件是评估两个负载（符合执行的脚本以及不符合执行的脚本）以及每当对 ServiceNow 中的表单上的任何字段进行更新时。进行更新时，将重新评估界面策略的条件。如果条件返回 true，则界面策略操作运行。如果条件返回 false（**如果启用不符合撤回，则返回 Reverse**），则运行与界面策略操作相反的操作。例如，如果界面策略在某个条件下**必填**某个字段，那么当不满足该条件时，它通常会将该字段显式设置为**非必填**。

由于这些事件都发生在浏览器（也称为客户端）中告诉我们的，界面策略显然是在客户端评估的，这意味着客户端 JET API 可用。在界面策略表单的**高级视图**中，有一个**脚本**部分。在本节中，如果选中**是否执行脚本**复选框，将显示两个脚本输入字段：如果为**true**则**执行，如果为 false**则执行。每个脚本都有一个预定义的默认值，作为脚本的脚手架：

```
function onCondition() {

}
```

<!-- ![image-20220510114125407](../../static/img/scripting-in-ui-policies.assets/image-20220510114125407.png) -->

如上一节所述，您在该函数内部编写的任何代码都将在客户端执行，因此可以访问与任何客户端脚本相同的 API。使用这些脚本字段，您可以定义在非脚本条件为真或假时触发的功能（取决于您将其输入的脚本字段），以及用于检查您无法检查的更复杂条件的附加逻辑使用简单的条件生成器进行检查。何时可以使用此功能的一个很好的例子是，如果您需要检查服务器上的一个值，除了某些条件构建器中的客户端条件，以确定是否应将字段设置为**只读** / **忽略** / **可见**。

我们将在后面关于客户端脚本的章节中了解所有关于客户端脚本和客户端 API 的知识。