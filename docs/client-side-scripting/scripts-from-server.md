---
sidebar_position: 6
sidebar_label: 'scripts-from-server'
---
通常，处理中的几毫秒延迟非常值得添加或改进给定脚本可能添加的功能，但即便如此，重要的是要考虑是否有更有效的方法来完成相同的最终目标。滞后或无响应是一种累积的挫败感。为了保持积极的用户体验，以下是一些需要避免的事情。这些不是必须始终遵循的严格规则，但它们是一般准则，如果您违反，您应该仔细考虑原因以及是否有替代方案：
1. 不要从服务器向客户端发送比您需要的更多的数据。这包括脚本文件、不必要的大 AJAX 响应以及几乎任何其他服务器/客户端通信。
2. 不要从客户端向服务器发送比您需要的更多的数据。

**Note**
由于带宽通常是不对称的，因此上传比下载具有更高的影响和更低的速度，这意味着客户端/服务器通信可能比服务器/客户端通信更慢。将数据集保持在最低限度。

除了在某些情况下，例如onSubmit客户端脚本，异步执行从客户端到服务器的所有请求。这将防止用户必须等待直到您的请求完成，然后才能将浏览器或选项卡的控制权返回给他们。尽量不要提醒或如果您能想出更好的方法来实现您的目标，请在提交表单时确认任何内容。

**Note**
警告和确认对话通常不是最好的用户体验，并将用户从 JET 界面和流程中拉出来。想象一个示例，如果用户将某个字段更改为特定值，您想确认他们是否打算选择该字段。不要在提交时发出警报，而是尝试在用户被选中时默认通知用户。例如，使用
```
g_form.showFieldMsg('field_name', 'message text here', 'info', true);
```
此g_form方法可以接受info或error作为其第三个字符串参数，第四个（可选）参数指定是否将字段滚动到用户的视口中（如果尚未滚动）。

如果您的代码曾经包含嵌套循环，请仔细考虑是否有更好的方法来做您想做的事情。嵌套循环可能非常耗费性能。例如，考虑以下代码：
```
    var gr1 = new GlideRecord('table_name');
    gr1.query();
    变量 gr2;
    变种结果 = [];
    /* 外循环 */
    而（gr1.next（））{
        gr2 = new GlideRecord('other_table_name');
        gr2.addQuery('field', gr1.getValue('field_name'));
        gr2.query();
        /* 内循环 */
        而（gr2.next（））{
            结果.push(gr2.getValue('other_field_name'));
        }
    }
```
这会遍历gr1中的每条记录，然后将gr1查询中的一个字段与gr2查询中的相似字段进行比较，最后从该查询返回的所有结果都被推送到一个数组中。一种更有效的方法是从gr1数组中获取所有值，然后更改gr2查询以检查验证字段是否是值之一：
```
    /* 第一个查询 */
    var gr1 = new GlideRecord('table_name');
    gr1.query();
    var valuesForQuery = [];
    而（gr1.next（））{
    valuesForQuery.push(gr1.getValue('field_name'));
    }
    /* 第二个查询 */
    var gr2 = new GlideRecord('other_table_name');
    gr2.addQuery('field', 'IN', valuesForQuery);
    gr2.query();
    变种结果 = [];
    而（gr2.next（））{
        结果.push(gr2.getValue('other_field_name'));
    } 
```