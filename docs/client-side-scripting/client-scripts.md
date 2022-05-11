---
sidebar_position: 1
sidebar_label: 'client-scripts'
---
客户端脚本是从服务器发送到用户浏览器在客户端执行的 JavaScript 片段。

请记住：只要界面策略可以有效地重现您在客户端想实现的功能，最好使用界面策略。

一般来说，一个好的客户端脚本会十分快速地响应用户的操作，并异步执行服务器查找（这包括从服务器查找属性、执行 `GlideRecord` 查询和处理时区）。

一般来说，与服务器通信的客户端脚本都应该是异步执行的，但`onSubmit`脚本的执行是一个例外，它是同步执行的。异步操作是异步执行的，这意味着浏览器不会等待它们完成，`onSubmit`脚本是当用户单击表单上的`Save`、`Update`或`Submit`按钮时执行，表单提交时页面会重新加载，为此，`onSubmit`客户端脚本只能同步运行。

像界面策略和客户端脚本中编写的操作，仅在表单视图中执行（`onCellEdit`除外，它在列表视图上执行）。这意味着，如果您在界面策略或者客户端脚本中使用客户端方法来控制数据，那么你必须使用另一种方法来保护该数据不会在列表视图中被修改：您可以通过使用 ACL（安全规则）、数据策略、业务规则（使用当前对象的`setAbortAction()`方法）或完全禁用列表编辑来完成此操作。

和在业务规则和其他服务器端脚本中一样，您可以使用 `g_form`客户端 API 访问表单上的变量：
```
g_form.setValue('variables.var_name', 'var value')
```
这同样适用于其他`g_form`方法，例如`setDisplay()/ setVisible()、setReadOnly()和setMandatory()`。

## 客户端脚本的类型
客户端脚本有多种类型：界面操作（如果配置为在客户端运行）、界面策略、客户端脚本。

客户端脚本是一种特定类型的客户端脚本，它在表单或者列表视图上执行。客户端脚本有四种基本类型：`onLoad`、`onChange`、`onSubmit`和`onCellEdit`。每种类型都有不同的使用条件和用途。
### onLoad
`onLoad`脚本在加载表单后立即执行，可用于执行一些操作：例如根据从数据库中查询到的值更改表单字段的状态或者值。

**提示**

`onLoad`客户端脚本可以提供类似`onChange`的功能，因为它允许您访问 DOM，并使用标准`JavaScript API `或` g_form.getElement()`方法直接与表单组件交互。此时，我们可以使用标准的`element.addEventListener()`JavaScript API 来监控键盘、鼠标或其他事件。但是，如果可以避免，强烈建议不要这样做，并且在服务门户中也不起作用，我们不应依赖 DOM 操作。

### onChange
`onChange`客户端脚本每当修改选定字段的值时执行。此外，当表单首次加载时，它也会触发`onChange`脚本，因为从技术上讲，这构成了一个更改事件。

onChange方法有一个默认参数`isLoading`，如果变量`isLoading`为`true`意味着更改是表单加载触发的，而不是用户手动更改字段的造成的。我们可以使用它编写一个条件块，当`isLoading`为`true`或者如果所选字段的新值为空，则`return`停止执行脚本的其余部分。

考虑这样一个例子，每当事件分配给具有管理权限的组时，该组的经理应设置为事件的受让人，如果`Assignment group`被清空，也清空`Assigned to`字段

为此，我们可能会使用这样的`onChange`客户端脚本，对`Assignment group`字段进行操作：

```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading) {
        return; //Do nothing if we're just loading the form
    } 
    else if (newValue == '') {
        //If assignment group blank, blank out assigned to
        g_form.setValue('assigned_to', '');
    }
    else {
        g_form.getReference('assignment_group', assignmentGroupCallBack);
    }
}

function assignmentGroupCallBack(grAssignmentGroup) {
    g_form.setValue('assigned_to', grAssignmentGroup.getValue('manager'));
}
```


### onSubmit
`onSubmit`客户端脚本会在用户保存表单时执行，无论他们单击保存、提交还是更新（或任何其他UI操作）。

十分重要的一点是，我们要了解`onSubmit`客户端脚本中的任何异步代码都有不执行的风险，因为表单可能会在收到服务器的响应之前重新加载！因此，任何必要的查询或其他服务器请求都应在 `onSubmit`脚本中同步执行，这也应该是唯一一种应该故意在客户端脚本中同步运行查询的情况。

此外，`onSubmit`客户端脚本的好处是能够在数据发送到服务器之前默认允许或阻止提交，只需返回`true`或`false`即可。出于这个方面，`onSubmit`客户端脚本是验证表单上数据的绝佳方式，可以调用confirm()函数与用户确认某些意图。

### onCellEdit
`onCellEdit`客户端脚本与`onChange`客户端脚本非常相似，不过`onCellEdit`是在列表上而不是在表单上执行的。

您可以双击其中一个字段并编辑值，如下截图所示：
![image-20220510110150846](/img/client-side-scripting/onCellEdit.jpg)

`onCellEdit`脚本函数具有以下参数：
- sysIDs：您选择的每个记录的sys_ids数组。如果用户只选择一个，这将是一个只有一个元素的数组。
- table：包含正在修改的记录的表的名称。
- oldValues：每个被修改的单元格的旧值数组。
- newValue：要放置在被编辑的单元格中的新值。
- callback：完成此脚本后要执行的回调函数。如果传递true到此回调中，那么其他的onCellEdit客户端脚本将继续运行，如果传递false到此回调中，则不会再执行进一步的脚本，更改将被中止。