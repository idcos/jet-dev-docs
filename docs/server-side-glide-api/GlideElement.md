---
sidebar_position: 4
sidebar_label: 'GlideElement'
---

# GlideElement

GlideElement 类通常不会自行实例化，它用于GlideRecord 对象中的元素，并提供与它们交互的方法。元素一般是指记录中的数据列，例如任务记录上的`number` 字段。您可以从服务器端 GlideRecord 对象访问 GlideElement （例如，通过使用`gr.number`），或使用GlideRecord 对象的方法`getElement`。

如果想获取或设置字段的值，最佳实践要求您**始终**使用 getter 和 setter 函数：`getValue()`，`setValue()`；如果您想直接访问服务器端`GlideElement`属性。将为您提供GlideElement 对象的引用。

例如，假设我们有一个包含名为 的列的表`short_description`。当我们查询这个表时，我们得到一个填充了各种属性的 GlideRecord 对象。表中的每个字段都成为 GlideRecord 对象中的一个属性，这些属性不仅仅是字符串和数字以及其他基本数据类型，对应于记录中一个字段的每个属性都是一个对象，它都是 GlideElement 类的一个实例。

这意味着字段/属性 ,`gr.short_description`对应于**GlideElement**对象，具有您接下来将看到的所有方法，我们将在这里介绍开发人员可能会相对频繁使用的最有用的方法。

事实上GlideRecord 的properties是**对象类型**而不是**原始类型**。也许您听说过 JavaScript 被称为**松散类型**语言。这不仅仅是一个口语术语。这意味着 JavaScript 中的数据类型是流动的。在许多其他语言中，当您声明一个变量并对其进行初始化时，您本质上将它链接到给定的数据类型：字符串、整数、浮点数、布尔值等。但是，在 JavaScript 中，您可以声明一个 `var`，将其设置为**字符串**值，然后修改其值，使其包含**数字**或任何其他类型——包括**原始类型**和**对象类型**。

这在某些情况下很有用: 例如，您可以`short_description`使用一行代码设置字段的值，例如`gr.short_description = 'New short description.";`. 然而，这实际上可能是一件坏事，因为您正在覆写曾经位于父 GlideElement 对象`short_description`属性中的 GlideElement 对象。但是，使用 GlideRecord 的` setValue()`方法可以正确设置 GlideElement 对象中的值。

出于同样的原因，由于 JavaScript 的**引用传递**特性，您不应使用以下语法设置变量：

```
var userReference = gr.assigned_to;
```

上面的代码行并不会将`userReference`变量设置为字段的值，而是会将变量设置为`gr.assigned_to`所指向的对象的引用。

想象一下，我们在变量 中有一个 GlideRecord 对象`gr`，带有一个应该给我们两个事件的查询： 首先 `INC0000001`，然后`INC0000002`。你认为最后一行会记录什么？

```
var incNumber; //declare above loop to prevent repeat declaration
while (gr.next()) { //Iterate over the two returned Incidents
    //If the number is INC0000001
    if(gr.getValue('number') === 'INC0000001') { 
        //set the variable to the "number" property of gr.
        incNumber = gr.number; 
    }
gs.log(incNumber);
```

这里说明一下上面这个脚本实际上并不会打印出字符串值`‘INC0000001’`，这里正确的赋值方法应该是`incNumber = gr.getValue('number');`。因为`gr.number`拿到的其实是对象的引用，而不是像字符串这样的原始值，所以对变量`incNumber`赋值为`gr.number`对象的引用。并且当 while 循环完成时，`gr.number`包含一个具有 value 的对象`INC0000002`，所以即使我们设置了当`gr.number`值为`INC0000001`时对变量进行赋值，但是当最后一行运行时，同一个对象`gr.number`仍旧包含不同的值，因此对包含在变量中的对象的引用也应该包含不同的值：`INC0000002`。

以下是GlideElement API的一些常用的方法:
- deleteMultiple	按条件删除
- udpateMultiple	按条件更新
- getGlideRecord	获取glideRecord记录
- getName	获取字段名
- getValue	获取字段值
- setValue	设置字段值
- getLabel	获取字段标签
- getReferenceTable	获取引用表名
- getRefRecord	获取引用记录
- nil	是否为空
- setQueryReferences	设置是否用引用值来做条件查询
- getDisplayValue	获取引用值
- getED	获取元素描述
- getAttribute	获取字段属性
- changes	检测记录是否有变化
- changesTo	检测记录是否变化为某个值
- canCreate	是否有权限插入记录 - 已定义返回true
- canDelete	是否有权限删除记录 - 已定义返回true
- canWrite	是否有权限更新记录 - 已定义返回true
- canRead	是否有权限读表数据 - 已定义返回true

## changes(), changesFrom() and changesTo()--待实现

GlideElement 类的`changes()`、`changesFrom()`和`changesTo()`方法通常是从之前运行的业务规则中调用或在进行给定的数据库更新之后。这些方法分别返回一个布尔值，指示字段是否已更改，是否已从指定值更改，或者是否已更改为指定值。

`changes()`方法_不接受任何论据。不需要，因为它是从对应字段的 GlideElement 中调用的，所以它已经知道在检查哪个字段。

`changesFrom()``changesTo()`然而，两者拿一个参数——一个值分别比较字段的旧值或新值。

这三个方法都返回一个布尔值`true`/`false`指示表达式是否准确，如您在以下示例用法中所见。

### 示例用法

在这里，想象我们有一条记录，其中活动字段设置为，`false`因为工单已关闭一段时间。通常情况下，会有如果我们不希望用户重新打开此类工单，请制定 ACL 或数据策略，但想象一下，在这种情况下，我们没有这样的事情。相反，只是为了这个演示，我们使用业务规则来做出决定，因为业务规则可以编写脚本并使用更复杂的逻辑：

```javascript
if (current.active.changesFrom(false) && !gs.hasRole('admin')) { 
    gs.addErrorMessage('Non-admins cannot re-open closed tickets after they've been deactivated.'); 
    current.setAbortAction(true); 
} 
```

正如你在在前面的示例中，此脚本检查两个条件：`active`字段从 更改`false`，并且用户没有管理员角色。如果这个条件的计算结果为`true`，那么我们中止操作并向用户显示我们为什么这样做的消息；否则，我们只需不做任何事情就可以让它通过。

换句话说，我们允许管理员重新激活记录，但不允许其他人重新激活。

在`if`块的条件中，我们还可以`changes()`显式使用并将新值与 true 进行比较，或者我们可以使用`changesTo()`代替`changesFrom()`，如您在以下条件中所见：

```javascript
current.active.changesTo(true) && !gs.hasRole('admin')
```



## getDisplayValue()

GlideRecord 和GlideElement 类包含一个`getDisplayValue()`方法。虽然 GlideRecord 对象 ( `gr.getDisplayValue()`) 的方法根据字段字典中标记为显示值的字段获取记录的显示值，`getDisplayValue()`但 GlideElement 类 ( `gr.field_name`. `getDisplayValue()`) 的方法为您提供为字段的显示值价值。

您可能认为字段的值和显示值相同，但某些字段类型可能在实际值和显示值之间存在差异。例如，下拉字段可以有标签也可以有值。它是存储在实际数据库中的值。**任务**表上的**State**字段是另一个示例，因为该值是整数，但显示值是其他值，例如**Draft**或**Pending**。类似地，引用字段既有显示值（通常是被引用表中记录的显示值），也有实际值（被引用记录的值）。如果您调用并传入参考字段，您将获得记录，但如果您使用`sys_id` `getValue()` `sys_id` `getDisplayValue()`在该字段的 GlideElement 上，您将获得显示值——通常是任务记录的数字字段。

### 示例用法

在这里，我们得到了票指定事件记录中父任务的编号，并将其保存到`parentTask`变量中：

```javascript
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var parentTaskNumber = gr.parent.getDisplayValue(); 
```

您也可以使用**dot-walk**到引用的记录来获取数字字段的值`gr.parent.number`，但上面的示例实际上更有效，因为它不需要服务器执行查询来查找引用的记录来获取该字段的值。

## getED()

`getED()`方法_GlideElement 类的唯一方法是获得GlideElementDescriptor 对象，它提供有关特定字段的信息，而不是这些字段内的数据。

GlideElementDescriptor 类提供了促进独特功能的方法，例如，`getEncryptionType()`如果在字段上启用了边缘加密，则`getAttachmentEncryptionType()`使用获取元素获取的字段中使用的加密类型。`getED()`您还可以使用`isEdgeEncrypted()`来检查是否为该字段启用了边缘加密。

也可以使用`getLabel()`or`getName()`分别获取元素的字段标签或字段名称，以及`getLength()`获取元素的字符数限制。

### 示例用法

以下脚本获取`short_description`字段的元素描述符，然后记录一些有关它的信息：

```javascript
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var element = gr.short_description.getED(); 
var description = 'The ' + element.getLabel() + ' field has the name: ' + element.getName() + '. '; 
if (element.isEdgeEncrypted()) { 
    description += 'It is ' + element.getEncryptionType() + ' encrypted.'; 
} 
gs.info(description); 
```

此代码将返回`The Short description field has the name: short_description.`——如果它已加密，它还将添加一个说明加密类型的句子。



## getReferenceTable() 和 getRefRecord()

这两个GlideElement 类的方法存在以检索信息关于参考字段和他们的数据参考。

两种方法都不接受参数（它们不需要，因为它们是从包含参考信息的 GlideElement 中调用的）。

该`getReferenceTable()`方法返回给定引用元素所引用的表的名称。`getRefRecord()`另一方面，该方法返回一个预先填充了记录的 GlideRecord 对象，该记录`sys_id`是字段的值。换句话说，返回该引用字段中引用的记录。

### 示例用法

在这里，我们打印出一些关于给定事件记录的父字段：

```javascript
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var tableName = gr.parent.getReferenceTable(); 
var parentGR = gr.parent.getRefRecord(); 
gs.info('The Parent field references the ' + tableName +  
' table, and currently contains the ' + parentGR.getDisplayValue()    + ' record.'); 
```



## nil()

该`nil()`方法确定如果字段为空或为空。它通常是与比较调用和传入字段名称的结果的布尔表达式相同的结果`getValue()`，带有一个空白字符串。

`nil()`返回一个布尔值`true`——如果该字段为空或空，并且`false`它包含一个值。

### 示例用法

以下片段检查指定的事件是否未分配给任何人。如果未分配，脚本会在系统日志中记录一条消息：

```javascript
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
if (gr.assigned_to.nil()) { 
    gs.info('Please assign ' + gr.getValue('number') + ' to a user.'); 
}
```



## toString()

GlideElement 类的`toString()`方法返回的值是一个字符串。`getValue()`在大多数情况下，这与调用GlideRecord 对象并将字段名称作为参数传入的结果基本相同。

如果一个字段有一个**value**和一个**display value**，例如在引用字段的情况下，这将返回**value**。

### 示例用法

在这里，我们记录一个`sys_id`与事件呼叫者的消息：

```javascript
var gr = new GlideRecord('incident'); 
gr.get('46f09e75a9fe198100f4ffd8d366d17b'); 
var callerSysID = gr.caller_id.toString(); 
gs.info('The caller's sys_id is ' + callerSysID); 
```