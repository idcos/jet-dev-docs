---
sidebar_position: 5
sidebar_label: 'GlideRecord'
---
GlideRecord类是JET中最无处不在和最有用的类之一。它的主要功能是查询数据库表，并显示与该表中与给定查询匹配的每条记录相对应的值。它还可用于添加、修改或删除记录。GlideRecord对象由具有与表中每个字段对应名称的属性组成。在客户端Glide API中，这些属性通常包含字符串，而在服务器端API上，这些属性包含具有自身方法和属性的GlideElement JavaScript对象。
就像服务器端版本一样，GlideRecord用于对JET中的记录执行数据库操作，例如查询、修改和创建记录。GlideRecord API的客户端版本仅包含服务器上可用方法的子集，但它确实启用了一个重要的新功能：回调函数。
客户端GlideRecord的query()方法以及insert()和deleteRecord()都接受回调函数。事实上，这些方法中的每一个都不应该在没有回调函数的情况下从客户端脚本调用。
客户端GlideRecord API中记录的方法的完整列表是：

- addOrderBy()
- addQuery()
- deleteRecord()
- get()
- getEncodedQuery()
- getLimit()
- getTableName()
- hasNext()
- insert()
- next()
- orderBy()
- query()
- setLimit()

在本节中，我们将了解其中一些常用的方法，这些方法与服务器端表亲不同。这意味着我们不会重新构建addQuery()等方法，这些方法的行为与服务器端脚本中的行为大致相同。
