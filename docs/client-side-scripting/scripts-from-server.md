---
sidebar_position: 6
sidebar_label: 'scripts-from-server'
---
无论您是从服务器向客户端发送AJAX请求，还是从服务器向客户端发送脚本文件，无论您是同步执行还是异步执行，始终都要考虑“这将对用户体验产生什么影响，我可以改进吗，以及（如果没有），值得吗？”

为了保持良好的用户体验，以下是一些在使用过程中需要避免的事项，但这些并不是必须遵循的严格规则，只是一般准则，如果您违反，您应该仔细考虑原因以及是否有替代方案：
1. 不要从服务器向客户端发送比实际需要的更多的数据。这包括脚本文件、不必要的大 AJAX 响应以及其他服务器/客户端通信。
2. 不要从客户端向服务器发送比实际需要的更多的数据。

**Note**

由于带宽通常是不对称的，上传比下载影响更大，速度也更慢，这意味着客户端/服务器通信可能比服务器/客户端通信更慢，所以我们需要将数据集保持在最低限度。

除了在某些情况下，例如`onSubmit`客户端脚本，请异步执行从客户端到服务器的所有请求，这将防止用户必须在您的请求完成前一直等待浏览器或选项卡的控制权返回。

**Note**

警告和确认对话框通常不是最好的用户体验，会将用户从 JET 界面和流程中拉出来。想象一个示例，如果用户将某个字段更改为特定值时，需要确认他们是否打算选择该字段。这时不要在提交时发出警报，而是尝试在用户被选中时默认通知用户。例如，使用
```
g_form.showFieldMsg('field_name', 'message text here', 'info', true);
```
此g_form方法可以接受info或error作为其第三个字符串参数，第四个（可选）参数指定是否将字段滚动到用户的视口中（如果尚未滚动）。

如果您的代码包含循环嵌套，请仔细考虑是否有更好的方法来做您想做的事情，循环嵌套是非常耗费性能的。例如，考虑以下代码：
```
    var gr1 = new GlideRecord('table_name');
    gr1.query();
    var gr2;
    var res = [];
    /* 外循环 */
    while（gr1.next（））{
        gr2 = new GlideRecord('other_table_name');
        gr2.addQuery('field', gr1.getValue('field_name'));
        gr2.query();
        /* 内循环 */
        while（gr2.next（））{
            res.push(gr2.getValue('other_field_name'));
        }
    }
```
上面的代码会遍历`gr1`中的每条记录，然后依次在`gr2`中查询`gr1`中的`field_name`字段，并将最后的查询结果都存放在一个数组中。我们有一种更有效的方法，就是从`gr1`中获取所有值并存放在一个数组中，然后在`gr2`中使用`IN`查询：
```
    /* 第一个查询 */
    var gr1 = new GlideRecord('table_name');
    gr1.query();
    var valuesForQuery = [];
    while（gr1.next（））{
        valuesForQuery.push(gr1.getValue('field_name'));
    }
    /* 第二个查询 */
    var gr2 = new GlideRecord('other_table_name');
    gr2.addQuery('field', 'IN', valuesForQuery);
    gr2.query();
    var res = [];
    while（gr2.next（））{
        res.push(gr2.getValue('other_field_name'));
    } 
```