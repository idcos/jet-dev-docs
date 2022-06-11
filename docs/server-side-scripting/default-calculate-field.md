---
sidebar_position: 4
sidebar_label: 'default-calculate-field'
---

# 默认值与计算值

认为给予表列一个默认值，并给它一个计算值，实际上具有相同的结果，这是一个容易的犯的错误。然而，这种情况并非如此。默认值和计算值的行为完全不同，应该用于完全不同的目的。

在本节中，我们将探讨每个的用途，以及它们之间的行为差异。我们还将讨论字段计算脚本，以及有关默认和计算字段的最佳实践。

## 默认值

默认字段值很简单：字符串、整数或任何与字段类型匹配的数据类型。



不过，您可以通过在值的开头使用关键字来获得更高级的功能，`javascript:`类似于我们在条件构建器中运行脚本的方式。之后的任何代码`javascript:`都将在表单加载时在服务器上执行，或者当一条记录插入数据库（但不是在更新时）时，该字段中没有值。

默认值脚本将可以访问该`current`对象，但请记住，在新记录表单上，默认值是在加载表单之前/期间在服务器上计算的。那时，`current`对象中没有数据。

如果在插入数据库时字段中没有值，则应用默认值。但是，如果创建记录时字段中存在任何值，则不会计算或使用默认值。

鉴于，除了插入时（只要插入时字段为空），还会计算默认值以便在新记录表单中显示，请考虑以下代码：

```
javascript:'Record created by ' + gs.getUser().getDisplayName() + ' on ' + current.getValue('sys_created_on') + '.';
```

通过`current`在默认值字段中使用上述代码中的对象，当表单加载时，我们将获得该`sys_created_on`字段的空白值，但我们的其余代码仍会执行。因此，在新记录表单上，我们将看到如下默认值：

```
由 John Smith 在 上创建的记录。
```

请注意默认值字符串中缺少预期的创建日期。

如果我们要删除新记录表单上的此值或通过以不使用表单的方式创建记录（例如通过脚本或通过列表编辑）将其留空，则默认值将重新在插入时进行评估，此时将有一个`current`对象供其引用，因此我们将获得预期的输出。但是，如果我们加载表单，获取缺少创建日期的值，然后将其保存，那么错误的值将被保存到数据库中。默认值不会在插入时重新计算，因为该字段现在将有一个值！

### 提示

从表单创建新记录时，默认值将预先填充到它所应用的字段中（正如我们在上面了解到的）。但是，您可以通过检查当前对象是否已填充来防止这种情况发生，以便默认值仅在插入时将数据放入字段（而不是在新记录表单上）。这是一个使用与上面相同的代码的示例，但包装在一个条件块中，如果记录被插入到数据库中（当当前对象可用时），它应该只填充默认值：

```javascript
javascript:if (!current.sys_created_on.nil()) { '这条记录是由 ' + gs.getUser().getDisplayName() + ' on ' + current.getValue('sys_created_on'); }
```

这种行为是从根本上将默认值功能与计算字段功能区分开来。





## 计算值

虽然默认值仅在表单加载或插入时应用，如果稍后更新字段或值更改，则不会重新评估，计算字段始终适用，并且在更新记录时重新评估。此外，虽然字段的默认值可以使用`javascript:`关键字编写脚本，但计算字段始终是编写脚本的。

要启用将字段设置为计算，首先通过单击**高级视图**UI 操作加载字段字典记录的**高级视图：**

<!-- ![计算值](../images/00217.jpeg) -->



接下来，转到“**计算值**”表单部分，然后选中“**计算**”复选框。这将启用字段计算脚本编辑器：

<!-- 
![计算值](../images/00218.jpeg) -->





计算字段values 确实可以访问该`current`对象（就像默认值脚本一样），但是由于每当更新记录时都会重新评估它们，因此`current`在加载新记录表单时对象为空的问题较小。

即使提交后会更改，但最好避免用户在新记录表单的计算字段中看到 null。`current`如果对象为空，甚至在提交之前将该字段留空，通常最好在对象为空时添加一些要显示的填充文本，如以下示例脚本所示：

```javascript
(function calculatedFieldValue(current) {

   var userName, updatedDate;
   if (current.sys_created_on.nil()) {
      return '';
   }
   var grUser = new GlideRecord('sys_user');
   if (grUser.get(gs.getUserID())) {
      userName = grUser.getDisplayValue();
      updatedDate = current.getValue('sys_updated_on');
      return 'Record updated by ' + userName + ' on ' + updatedDate + '.';
   }

})(current);
```