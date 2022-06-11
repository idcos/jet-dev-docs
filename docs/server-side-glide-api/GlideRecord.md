---
sidebar_position: 3 
sidebar_label: 'GlideRecord'
---

# GlideRecord

GlideRecord类是JET中最常用的类之一。它的主要功能是查询数据库表，并显示该表中与给定查询匹配的值。它还可用于添加、修改或删除记录。GlideRecord 对象由属性组成，其名称对应于表中的每个字段。在客户端
Glide API中，这些属性通常包含字符串，而在服务器端API中，这些属性包含具有自己的方法和属性的GlideElement对象。

使用GlideRecord进行查询可以有两种方式：1.使用sys_id直接查询 2. 根据field查询条件进行查询。   
示例代码：
```
    // 根据 sys_id 进行查询
    var sys_id = 'sys_id';
    var gr1 = new GlideRecord('project');
    // 添加sys_id查询条件
    gr1.get(sys_id);
    gs.print(gr1.getValue('project_name'));
    
    // 根据字段查询条件进行查询
    var gr2 = new GlideRecord('x_yunji_jet_05_offer_info');
    // 定义查询条件
    gr2.addQuery('sys_id', sys_id);
    // 执行
    gr2.query();
    gs.print('查询结果集数目：' + gr2.resultSet.total);
    // 遍历结果集
    while(gr2.hasNext()) {
          var dto = gr2.next();
          // 获取属性并打印
          gs.print(dto.project_name);
    }

```
如下是GlideRecord API的一些常用的方法:
- query 查询记录
- getValue 获取字段值
- setValue 设置字段值
- hasNext 是否有下一条记录
- next 获取下一条记录
- insert 插入记录
- update 更新记录
- addActiveQuery 查询active记录列表
- addInactiveQuery 查询active记录为false列表
- addNotNullQuery 非空条件查询
- addNullQuery 空条件查询
- addQuery 添加查询条件
- deleteMultiple 按条件删除
- udpateMultiple 按条件更新
- deleteRecord 删除记录
- getRowCount 获取行数
- orderBy 升序排序
- orderByDesc 降序排序
- setLimit 限定条数
- setAbortAction 设置中断标记
- getRecordClassName 获取当前记录的表名
- get 获取满足某字段条件的记录
- updateWithReferences 更新引用表记录值
- setQueryReferences 设置是否用引用值来做条件查询
- setDisplayValue 设置引用值
- insertWithReferences 带引用值插入
- getDisplayValue 获取引用值
- addJoinQuery 添加join条件
- getED 获取元素描述
- getElement 获取指定的element
- getEncodedQuery 获取编码后的查询条件
- addEncodedQuery 已编码查询条件查询
- getAttribute 获取字段属性
- getTableName 获取表名
- isValidRecord 当前记录是否有效
- isNewRecord 是否新记录
- getFields 获取字段列表
- getLabel 获取字段标签
- canCreate 是否有权限插入记录
- canDelete 是否有权限删除记录
- canWrite 是否有权限更新记录
- canRead 是否有权限读表数据
- changes 检测记录是否有变化
- isValid 当前表是否有效
- isValidField 当前字段是否有效
- operation 是滞更新，插入，删除操作
- setNewGuid 设置新的guid值
- setNewGuidValue 指定guid值


## Initialize

GlideRecord 对象是通过使用new关键字进行初始化（调用initialize()构造函数方法）并传入表名称字符串，定义GlideRecord对象要绑定的数据库表。    
示例用法
初始化一个新的事件表上的 GlideRecord 对象，并将其存储在gr变量中：

```
var grIncident = new GlideRecord('incident');
grIncident.addActiveQuery();
grIncident.query();
while (grIncident.next()) {
    //do something
}
```

## addQuery()

GlideRecord 类的`addQuery()`方法分为三种调用方式，分别提供一个、两个、三个参数。如果将一个参数传递给该`addQuery()`方法，那么它将假定该参数是一个编码查询。编码查询是一个字符串，表示单个查询（甚至多个查询）中的所有条件！

![查询](/img/server-side-glide-api/query.png)



这在功能上与`addEncodedQuery()`方法相似。

您将在下一节中看到这种单参数用法的示例。另一方面，如果传入两个参数，则假定第一个参数是字段名，第二个参数是是期望值。

最后，如果你使用了三个方法的参数`addQuery()`，您不仅可以指定字段和值，还可以指定它们之间的运算符。在这种情况下，第二个参数可以定义为包括一些具有明显含义的标准运算符，以及一些用于更高级功能的运算符。您将在下表中找到可用的运算符列表。

### 可用的查询运算符

下表显示查询运算符（可以在 GlideRecord` addQuery()`方法中指定为第二个参数）。这些操作符决定了数据库查询是如何在服务器上形成的。

| 操作员             | 意义                                                         |
| :----------------- | :----------------------------------------------------------- |
| `=`（等于）        | 字段（第一个参数）必须等于值（第三个参数）。                 |
| `>`（大于）        | 指定的字段必须包含一个大于第三个参数中指定的值的值。         |
| `<`（少于）        | 该字段必须包含一个小于指定值的值。                           |
| `>=`（大于或等于） | 第一个参数中的字段的值必须包含一个大于或等于第三个参数指定值。 |
| `<=`（小于或等于） | 该字段必须包含小于或等于指定值。                             |
| `STARTSWITH`       | 该字段必须包含一个值，该值以字符串指定的值开头。             |
| `CONTAINS`         | 该字段的值必须包含第三个参数中指定的字符串。                 |
| `DOES NOT CONTAIN` | 该字段的值不得包含指定的值。                                 |
| `IN`               | 如果这是调用`addQuery()`方法时的第二个参数，则第三个参数应包含一个或多个值的逗号分隔列表。第一个参数中指定的字段值必须是第三个参数中指定的值之一。例子：`gr.addQuery('state', 'IN', '1,3,5');` |
| `NOT IN`           | 第一个参数中指定的字段的值不存在于第三个参数中指定的以逗号分隔的值列表中。 |
| `ENDSWITH`         | 字段值必须以第三个参数中指定的值结尾。                       |
| `INSTANCEOF`       | 这是一个特殊的运算符，旨在用于基表或父表，以确定给定记录的表是否是给定子表的实例。在这种情况下，您将指定`sys_class_name`为第一个参数、`INSTANCEOF`第二个参数，以及另一个表名作为第三个参数。例如，您可能想要查询基本系统任务表，并且只查看请求。查询将如下所示： `gr.addQuery('sys_class_name', 'INSTANCEOF', 'sc_request');`。通过这种方式，你可以查看到所有继承自`sc_request`表的记录。 |

### 示例用法

有一个参数，假设参数是一个编码查询：

```
var gr = new GlideRecord('incident');
var encodedQuery = 'assigned_to.active=true^active=true^ORstateIN1,2,3';
gr.addQuery(encodedQuery);
```

有两个参数，假设第一个是字段名，第二个是一个值，用于比较该字段的值是否相等：

```
var gr = new GlideRecord('incident');
gr.addQuery('number', 'INC0012345');
```

使用三个参数，第一个假定为字段名称，第二个是运算符，第三个是用于比较第一个字段的属性值：

```
var gr = new GlideRecord('incident');
gr.addQuery('closed_at', '>', gs.nowDateTime());
```

或者您可以使用`CONTAINS`运算符：

```
var gr = new GlideRecord('incident');
gr.addQuery('number', 'CONTAINS', '123');
```

本章前面的表格中列出的任何运算符都可以用来代替`CONTAINS`前面的代码片段。



### addActiveQuery()

`addActiveQuery()`是一个不接受任何参数的API方法，表示添加一个要求**active**字段必须为**true**的查询，等同于`gr.addQuery('active', 'true')`。



### addNullQuery() 和 addNotNullQuery()

两者都接受一个字段名参数。

GlideRecord对象调用`addNullQuery()`方法时，它为您的查询添加了一个检索条件，查询结果的这个字段值一定为 null（空）。

另一方面，当`addNotNullQuery()`使用字段名称作为参数调用该方法时，会在查询中添加一个条件，查询结果中的这个字段值一定不为 null（空）。

#### 示例用法

在第二行和第三行，`addNullQuery()` 和 `addNotNullQuery()`对应不同的字段检索条件：

```
var gr = new GlideRecord('sc_request');
gr.addNullQuery('assigned_to');  
gr.addNotNullQuery('assignment_group');
gr.query();
while (gr.next()){
    //do something 
}
```

在这种情况下，我们的查询将返回`assignment_group`字段不为空但`assigned_to`字段为空的结果。



## canRead()、canWrite()、canCreate() 和 canDelete()

这四个都是无参方法，每个方法都返回一个布尔值`true`/`false`，表示当前用户是否可以读取、写入、创建或删除记录。

这些方法不带参数，因此它们适用于整个记录而不是给定字段。

### 示例用法

经常被用于条件判断；例如，确定 UI 操作可见性，您可以在示例代码片段之后的屏幕截图中看到：

```
current.canCreate() && gs.hasRole('itil')
```

![条件](/img/business-rules/condition.png)

这两个条件检查用户是否可以在该表中创建记录，以及用户是否具有`itil`角色。

### 提示

你也可以在上面添加注释，如下所示：

```
current.canRead() /*用户可以读取这条记录*/ && gs.hasRole('admin') /*并且用户具有管理员角色*/
```

## deleteRecord() 和 deleteMultiple()

这两个方法是删除一个或多个来自数据库的记录。

JET一般比较少用物理删除，推荐的方式是使用字段标识逻辑业务删除，便于查看历史数据。

`deleteRecord()`和`deleteMultiple()`不接受任何参数，因为两者都是特定 GlideRecord 对象的方法，但是两者的使用方式完全不同。

`deleteRecord()`用于从数据库中删除单个记录（在 GlideRecord 中）。`deleteMultiple()`用于删除与给定查询匹配的所有数据库记录。但是有一点很重要：`deleteMultiple()`不要在包含货币字段的表上使用，在这种情况下应该使用`deleteRecord()`。

### 示例用法

`deleteMultiple()`方法可以在`query()`之后调用，也可以不调用`query()`但同时不调用`next()`：

```
var query = 'active=false^closed_atRELATIVELT@year@ago@2'; //Tickets closed more than 2 years ago 
var gr = new GlideRecord('incident'); 
gr.addQuery(query); 
gr.deleteMultiple(); 
```

在这种情况下，我在**Incident**表上运行它，在我的实例中它没有任何货币字段。同样，由于 JET 的限制，不应在任何包含货币字段的表上调用此方法。它也不应该与`setLimit()`一同使用。如果出现上面的情况，请使用以下`deleteRecord()`语法：

```
var query = 'active=false^closed_atRELATIVELT@year@ago@2'; //Tickets closed more than 2 years ago 
var gr = new GlideRecord('incident'); 
gr.addQuery(query); 
gr.query(); 
while (gr.next()) { 
    gr.deleteRecord(); 
} 
```

这两个代码块都将删除查询匹配的所有记录。该查询会提取超过两年并且已经关闭的所有非活动事件列表并执行删除操作。



## get()

`get()`是一个非常简单的查询方法。它只会返回一条记录。此方法可以接受一个或两个参数。

如果提供了一个参数，则这个检索字段默认是sys_id，参数值是数据库中对应记录sys_id的值。

如果指定了两个参数，则预期第一个参数将是字段名称，第二个参数将是在该字段上过滤的值。您不能指定三个参数，这意味着不能使用任何特殊运算符，例如`CONTAINS`、`in`。

该`get()`方法将只返回一条记录，类似于 使用`gr.setLimit(1);`，或者只调用`gr.next();`一次。即使条件匹配了多条记录也只会返回第一条（两个参数的情况）。

调用 GlideRecord`get()`方法有效地结合了添加查询的步骤，调用 . `query()`, 并调用 . `next(),`一步到位。下面的示例演示了它是如何工作的。

### 示例用法

与一个参数，`get()`期望一个`sys_id`：

```
var recordSysID = '46f09e75a9fe198100f4ffd8d366d17b'; 
var gr = new GlideRecord('incident'); 
if (gr.get(recordSysID)) {
    //do something with the record 
} else {
    //do something when the record is not found
}
```

使用两个参数，`get()`期望第一个参数是字段名称，第二个参数是要与字段值进行比较的值。以下代码块将返回一条记录——与指定查询匹配的第一条记录：

```
var gr = new GlideRecord('incident'); 
if (gr.get('number', 'INC0000025')) {
    //do something with the record 
} else {
    //do something when the record is not found
}
```



## getDisplayValue()

正如我们在前面的章节中学习过，每个表都有一个值设置为显示值。这是在引用字段或系统中的其他位置引用记录时显示的值。该字段通常（但并非总是）是唯一的。可以通过更改表中某一列的字典记录来更改引用字段，但一次只能有一个字段作为给定表中的显示值。将新字段的**显示值**字段设置为**true**将自动将该字段在前一个显示值的字典条目中设置为**false**，从而确保一次只有一个显示值。

`getDisplayValue()`非常简单，因为它不接受任何参数，并返回设置为显示值的任何字段的值，用于调用它的记录。

### 示例用法

以下代码片段会将`displayVal`变量的值设置为`INC0010014`. `INC0010014`恰好是事件的**Number** [ `number`] 字段的值，因为 number 是**Incident**表中设置为显示值的字段：

```
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var displayVal = gr.getDisplayValue(); 
```

返回的值是一个**原始**值（一个字符串），因此该`displayVal`变量现在将包含一个事件编号字符串。



## getValue() and getUniqueValue()

`getValue()`和`getUniqueValue()`都返回当前GlideRecord对象的一个字段值。

`getUniqueValue()`没有参数，只返回记录的主键`sys_id`。

`getValue()`它接受一个字段名参数，并将返回其中的值。

### 示例用法

`getUniqueValue()`没有参数，这是一个快速简便地检索`sys_id`给定记录的方法。

`getValue()`返回特定字段或列的值。这可以是`sys_id`列，也可以是任何其他可能包含值的列；只需将字段名称传递给`getValue()`，它将返回与指定记录上字段内容匹配的原始（字符串）值：

```
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var sysIDVal = gr.getValue('sys_id'); 
var incNumber = gr.getValue('number'); 
```



## hasNext()

`hasNext()`是一个返回布尔值的简单方法，指示是否有更多从 GlideRecord 查询返回的记录，可以使用`.next()`.

例如，如果您的查询返回三条记录，您可以`.next()`在查询之后调用以获取第一条记录。此时，`hasNext()`将返回`true`。再次调用`next()`以获取第二条记录将给我们留下一条记录，因此再次`hasNext()`返回 true。但是，再调用`next()`一次会将第三条也是最后一条记录填充到 GlideRecord 对象中。没有更多的记录可以抓取，如果我们`hasNext()`此时再次调用，它将返回`false`。

### 示例用法

在这里，我们使用`hasNext()`作为循环条件，并`next()`在循环内调用：

```
var query = 'active=false^closed_atRELATIVELT@year@ago@2';  
var gr = new GlideRecord('incident'); 
gr.addQuery(query); 
gr.query(); 
while (gr.hasNext()) { 
    gr.next(); 
    //do something 
}
```

我们可以通过`.next()`在循环条件中使用来简化这个循环，因为`true`如果它成功地拉出一条新记录，它就会返回（当被调用时），`false`如果它不是。`next()`本章稍后会记录。有使用`hasNext()`配合的`next()`用例，但`next()`更为常见。



## initialize() and insert()

如果你想在数据库中创建一条新记录，您通常需要调用两个方法：`initialize()`和`insert()`，也可以只使用`insert()`.

这两种方法都不接受任何参数，并且它们的用途在功能上是相关的。

当你创建一个新的 GlideRecord，并且想为对象填充一些数据。你可以将此对象想象为一个空白电子表格。无论您是想从电子表格中读取数据，还是将数据保存到其中，您需要做的第一件事就是弄清楚标题列应该是什么。这本质上是`initialize()`API 的功能。

当你创建一个新的 GlideRecord、定义你的过滤器查询并运行该`.query()`方法时，它会与数据库交互并设置 GlideRecord 对象并保存字段属性值。

如果我们调用`next()`系统获取数据库中的下一条（或第一条）记录，并填充所有GlideRecord 对象属性（如列），其值对应于一条记录。如果我们使用`setValue()`修改这些值之一，然后调用`update(),`系统将获取修改后的数据并将其加载到现有的数据库中，覆盖旧值。

GlideRecord的`initialize()`方法（与脚本 include 的方法不同）的作用与`query()`相似，可以用来获取数据库表的基本结构。`insert()`可以将我们自定义填充的值推送到数据库，如果有一些字段值我们没有声明但是字段列定义了默认值则字段会被填充为默认值。

### 笔记

`initialize()`方法准备了一个可填充的GlideRecord对象，`insert()`方法将填充了数据的对象推送到数据库。

### 示例用法

我们首先声明我们的GlideRecord对象，在**Incident**表上对其进行初始化，并设置一些值。

当我们在*第 3 行*和*第 4 行*调用方法`setValue()`时，我们在 GlideRecord 对象中填充 GlideElement 对象属性，使用`setValue()`方法进行赋值：

```
var gr = new GlideRecord('incident'); 
gr.initialize(); 
gr.setValue('short_description', 'Example record'); 
gr.setValue('description', 'This is a test.'); 
gr.insert(); 
```

最后，在*第 5 行*，我们调用该`insert()`方法，该方法将 GlideRecord 对象推送到数据库，此时将使用指定的值创建一条新记录，如果未指定的字段值并且字段上声明了默认值则使用默认值进行填充。

也可以在现有的GlideRecord上插入，从而有效地创建副本。    

不使用`initialize()`API 的代码示例：    

```
var gr = new GlideRecord('incident');
gr.setLimit(1);
gr.addActiveQuery();
gr.query();
gr.next(); //Load the first active Incident from the database
gr.setValue('active', 'false'); //Set the active field to false
gr.insert(); //Insert a new record with our modification
```

首先，我们加载GlideRecord对象并绑定事件表，然后我们通过将`active`字段设置为`false`。注意，在我们调用`update()`API 之前，这些更改只存在于我们的本地 GlideRecord 变量 ( `gr`) 中, `update()`方法用于提交更新记录到数据库，`insert()`方法用于插入记录到数据库。


## next()

`next()`方法的使用位于`query()`方法之后. 用于获取下一行记录，并使用该记录/行中每个列/字段的值填充`GlideRecord`对象。`setValue()`可以修改这条记录属性并可以使用`update()`进行更新数据库，或者可以使用`getValue()`读取值。

`next()`方法不接受任何参数，返回一个布尔值，根据是否成功找到另一条记录判断`true/false`；因此，`next()`方法调用可以同时用作循环条件。

### 示例用法

在这里，我们使用`next()`方法既作为迭代器移动到数据库中的下一条记录，又作为 while 循环条件中的条件（因为它在成功时返回 true）：

```
var gr = new GlideRecord('incident'); 
gr.addQuery('active', 'true'); 
gr.query(); 
while (gr.next()) { 
    //do something 
}
```

### 提示

不要混淆`next()`, 和`hasNext()`方法的返回值。`next()`如果它在列表中找到与您的查询过滤器匹配的下一条记录，将返回 true，并且**已经**将其加载到 GlideRecord 对象中。另一方面，`hasNext()`如果数据库中存在下一条记录，则返回 true，但它不会加载它。前者告诉您当前（尝试）迭代到的记录，后者查看**下一个**记录以查看是否存在 - 但不会像`next()`那样移动到下一条记录上。



## orderBy() and orderByDesc()

`orderBy()`和`orderByDesc()`都在查询结果返回之前进行排序，保证使用`next()`方法遍历结果时，可以按照指定的顺序获得结果。

方法`orderBy()`都`orderByDesc()`接受一个参数：当前GlideRecord 对象中的字段名称。如果您使用该方法，该字段将按从低到高/a-to-z/过去到现在进行排序：正序`orderBy()`、倒序`orderByDesc()`.

### 示例用法

`orderBy()`和`orderByDesc()`调用方式相同，示例如下：

```
var gr = new GlideRecord('incident'); 
gr.addQuery('active', 'true'); 
gr.orderBy('priority'); 
gr.query(); 
while (gr.next()) { 
    //do something 
}
```

应该在调用`query()`方法之前调用这些API，因为数据库会在将查询结果返回到您的脚本之前对其进行排序。


## query()

GlideRecord 类中`query()`方法可以从数据库中检索数据(或者直接用`get()`来检索单个记录)。

当调用`query()`方法时，将查询提交到数据库，对于返回结果集可以使用`next()`方法遍历。

### 示例用法

排除使用`get()`或者定义了GlideRecord的业务规则之类的情况下。调用`query()`方法通常可以对数据库中已经存在的任何记录执行检索：

```
var gr = new GlideRecord('incident'); 
gr.addActiveQuery(); 
gr.orderBy('priority'); 
gr.query(); 
while (gr.next()) { 
    //do something 
}
```



## setAbortAction()

这种方法是最常用于业务规则中，用来终止正在执行的任何数据库操作。在业务规则中对对象调用此方法意味着`current`不会向数据库提交任何更改。

还有其他方法可以防止数据库操作（例如，数据策略和 ACL），但这允许您对它发生的方式和时间进行更细粒度的控制。例如，您可能会在业务规则中编写一个脚本，该脚本会检查多个条件——甚至是基于外部记录或系统属性的条件——并根据这些条件确定是否中止数据库操作。

`setAbortAction()`方法接受一个布尔值参数。无论脚本中`setAbortAction()`方法执行了多少次，都以最后一次该方法中传入的布尔值来判断数据库操作是否实际终止。

换句话说，先调用`setAbortAction(true)`，然后再调用`setAbortAction(false)`，将导致数据库操作不会被取消。

### 示例用法

可以设置一些条件来控制何时触发业务规则的执行：

```
if (current.getValue('active') === 'false') { 
    current.setAbortAction(true); 
} 
if (gs.hasRole('admin')) { 
    current.setAbortAction(false); 
}
```

您也可以默认中止该操作，后续根据条件控制是否执行。在以下示例中，仅当`current.getValue('active') === 'true'`表达式为 false 时才会终止数据库操作：

```
current.setAbortAction(true);
if (current.getValue('active') === 'true') {
    current.setAbortAction(false);
}
```



## setLimit()

GlideRecord类的`setLimit()`方法接受一个参数：控制返回的最大记录条数。可用于限制大型查询的潜在性能影响，或将其与`orderBy()`或者`orderByDesc()`结合使用，可以进一步扩展此方法的可用性。

### 示例用法

比如您希望脚本从数据库中提取最多 10 个事件，以便将它们分配给`assignee`变量中的特定用户。但是您不只是想从队列中分配 10 个随机事件；您可能希望确保首先获得最高优先级，因此我们按**Priority**字段排序：

```
var gr = new GlideRecord('incident'); 
gr.addQuery('assignment_group', assignmentGroup); 
gr.orderBy('priority'); 
gr.setLimit(10); 
gr.query(); 
while (gr.next()) {
    gr.setValue('assigned_to', assignee); 
    gr.update(); 
}
```



## setValue()

除了设置日志字段，`setValue()`用于更新任何GlideRecord对象中的字段值。更新字段值后，需要使用`insert()`、`update()`等方法将新数据提交到数据库。

`setValue()`接受两个参数：字段名称和该字段的新值。它不返回值。

相同的规则适用于该`getValue()`方法：几乎任何时候在需要从GlideReord字段中检索值都应该使用它。你应该养成使用`getValue()`和`setValue()`作为标准做法的习惯。

### 提示

您可以通过查看[pbr.sngeek.com](http://pbr.sngeek.com)上的文章了解更多关于使用`getValue()`和 `setValue()`的重要性以及此规则的一些例外情况。

### 示例用法

在这里，我们使用`get()`两个参数按数字检索 RITM（从`sc_req_item `表中），然后我们使用`setValue()`和`gs.getUserID()`将该`assigned_to`字段设置为当前用户。最后，我们使用`update()`方法将更改提交到数据库：

```
var ritmGR = new GlideRecord('sc_req_item'); 
ritmGR.get('number', 'RITM0010455'); 
ritmGR.setValue('assigned_to', gs.getUserID()); 
ritmGR.update(); 
```



## update() 和 updateMultiple()

`update()`和`updateMultiple()`都将导致数据库更新。主要区别是`update()`一次只对一条记录进行操作并且`update()`返回更新记录的系统ID，`updateMultiple()`则同时更新GlideRecord查询返回的所有记录，而不用使用`next()`去遍历每一个。

`update()`接受一个可选参数：更新的原因。此原因将记录在更新记录的审核历史记录中。

`updateMultiple()`在另一方面，不接受参数，并且不返回任何值。它不应与`setLimit()`共用。

### 提示

虽然可以使用`gr.ield_name = 'new_value';`修改字段的值，但这通常不建议这样使用。在使用`updateMultiple()`时需要改为使用`setValue()`，否则更新可能会忽略查询过滤器，从而更新表中的每条记录。    
数据库表中的每个字段都成为 GlideRecord 对象的一个元素（`gr`在前面的代码片段中）。这个元素不仅仅是一个字符串或数字，而是一个特定的 Object 类型，称为 GlideElement。本章稍后会详细介绍该 API。

### 示例用法

在这里，我们用`update()`修改单个记录：

```
var ritmGR = new GlideRecord('sc_req_item'); 
ritmGR.get('number', 'RITM0010455'); 
ritmGR.setValue('assigned_to', gs.getUserID()); 
ritmGR.update(); 
```

在以下示例中，我们将`updateMultiple()`通过将一个用户（存储在`assignee`变量中）分配所有未分配的事件：

```
var gr = new GlideRecord('incident');  
gr.addNullQuery('assigned_to'); //checks for blank assignee field
gr.query();  
gr.setValue('assigned_to', assignee);  
gr.updateMultiple(); 
```

请注意，我们这里根本不需要使用`next()`方法，因为我们正在使用`updateMultiple()`方法。并且需要使用`setValue()`，而不是直接将值设置为属性。