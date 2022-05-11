---
sidebar_position: 5
sidebar_label: 'GlideRecord'
---
GlideRecord 类是 JET 中最无处不在和最有用的类之一，它的主要功能是查询数据库表，并显示与该表中与给定查询匹配的每条记录相对应的值。它还可用于添加、修改或删除记录，GlideRecord 对象由具有与表中每个字段对应名称的属性组成，在客户端 Glide API 中，这些属性通常包含字符串，而在服务器端 API 上，这些属性包含具有自身方法和属性的 GlideElement JavaScript 对象。

就像服务器端版本一样，GlideRecord 用于对 JET 中的记录执行数据库操作，例如查询、修改和创建记录。GlideRecord API 的客户端版本仅包含服务器上可用方法的子集，但它确实启用了一个重要的新功能：回调函数。
客户端 GlideRecord 的 query()方法以及 insert()和 deleteRecord()都接受回调函数。事实上，这些方法中的每一个都不应该在没有回调函数的情况下从客户端脚本调用。
客户端 GlideRecord API 中记录的方法的完整列表是：

- addQuery()
- deleteRecord()
- get()
- getEncodedQuery()
- getLimit()
- getTableName()
- hasNext()
- insert()
- next()
- query()
- setLimit()

在本节中，我们将了解其中一些常用的方法，这些方法与服务器端表亲不同。这意味着我们不会重新构建 addQuery()等方法，这些方法的行为与服务器端脚本中的行为大致相同。

#### 参考示例：  
查询
```
var now_GR = new GlideRecord('java_developer');
now_GR.addQuery('name', 'java2'); // Name is java2
now_GR.query(response);
 
function response(result, answer) {
    while(result.next()) {
       // Print all name is java2
        console.log(result.getValue('age')); 
    }
}
```
删除
```
var recordGR = new GlideRecord('java_developer');
if (recordGR.get('sys_id')) {
  recordGR.deleteRecord(response);
}

function response(result) {
  alert('Deleted record sys_id: ' + result.getValue('sys_id'));
}
```