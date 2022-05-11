---
sidebar_position: 3
sidebar_label: 'GlideForm(g_form)'
---
GlideForm API在客户端脚本中作为对象变量g_form公开。此API提供了与表单和字段交互、为用户添加消息、从下拉字段中添加和删除选项等方法。
下面，我们将讨论GlideForm API的一些更常用和有用的方法，并了解如何使用它们。
### 在表单和字段上设置/清除消息
有多种方法可以设置和清除表单中的消息，这两种方法都位于用户界面的顶部（类似于服务器端 API 的行为：gs.addInfoMessage（） 和 gs.addErrorMessage（）），以及针对特定字段。
这些方法是：showFieldMsg（）、hideFieldMsg（）、showErrorBox（） 和 hideErrorBox（）。
g_form API 的这些方法对于从客户端脚本或 UI 策略向用户传达信息（如字段是否满足某些验证条件）非常有用。
GlideForm （g_form） API 的一组字段级方法是 showFieldMsg（） 和 hideFieldMsg（），以及几乎相同的 showErrorBox（） 和 hideErrorBox（）。这些 API 非常适合在 onChange 客户端脚本中使用，如以下示例所示：
```
var now = new Date();  
now.setHours(0, 0, 0, 0); //beginning of today
var closedDate = new Date(g_form.getValue('closed_at'));  
if (closedDate > now) {  
    g_form.hideFieldMsg('closed_at', true);  
    g_form.showFieldMsg('closed_at', 'Are you a time traveler?');  
} else {  
    g_form.hideFieldMsg('closed_at', true);  
}
```
在前面的示例中，我们首先声明一个新的 JavaScript Date 对象，该对象现在会自动初始化为日期和时间。
接下来，我们声明一个新的 Date 对象，并使用 closed_at 字段的当前值对其进行初始化。然后，我们调用 setHours()方法并传入 0，0，0，0，这会将 closedDate 变量设置为所选日期的 12:00 AM（00:00 小时），以便在第 4 行上比较两个声明的 Date 对象。如果closed_at字段的值晚于今天，我们将清除任何其他字段消息（以避免重复消息），并显示一个字段消息，询问用户是否是时间旅行者。否则，我们会清除任何现有的字段消息。
请注意，在第 5 行，我们在添加字段消息之前调用 hideFieldMsg（另一个g_form API）。这是因为否则，如果用户一遍又一遍地修改字段，他们将获得越来越多的字段消息堆叠在一起。
### 处理下拉列表字段
表单中的下拉字段，类似于引用字段，可以同时具有值和显示值。例如，基于任务的表表单上的状态字段（如事件表单）有一组状态选项，名称如New、In进行中和Resolved。
然而，与这些选择标签关联的实际值是一个整数。事实上，状态字段是一个特定的数字类型字段。这意味着，尽管下拉菜单中显示标签/显示值，但如果我们调用g_form.getValue('state')，我们将得到一个数字值，而不是我们在表单中看到的标签。
您可以通过右键单击字段标签并单击显示选择列表（当然是管理员用户）来查看可能选择列表及其相应的值。
#### 提示
每当您为下拉字段创建新的可选值时，最好在您创建的值和上一个值之间留出一些数值空间。这既适用于订单字段，也适用于数字字段的值。例如，如果最高编号值为10，请考虑将新选项的值定为15甚至20。
#### 备注
在顺序或值数字段中可以进入的值几乎没有限制，因此在值之间留出大约10甚至100的间隙是完全可以的。例如，我想在列表中显示的第一个项目可能有10个顺序，第二个项目有20个顺序，以此类的顺序。
有多种GlideForm（g_form）方法用于处理下拉列表字段：addOption()、removeOption()和clearOptions()。
addOption()方法可以用三个参数调用，第四个可选参数：

- 包含字段名称的字符串（不要与字段标签混淆）。
- 选择值，这是存储在数据库中的实际值。在前面的状态字段示例中，这将是实际存储在数据库中的整数值，而不是选择标签，选择标签将是下拉列表中显示给用户的状态名称。

注：在前面的状态字段示例中，这将是实际存储在数据库中的整数值，而不是选择标签，选择标签将是下拉列表中显示给用户的状态名称。

- 选择标签，这是在下拉列表中向用户显示选项的标签。
- 选择索引[可选]，指示列表中选择的顺序。如果没有指定，新的列表选项将简单地显示在末尾，在其他选项之后。

请注意，此选择索引与顺序不同，顺序决定了字段最初在加载时的显示方式，另一方面，removeOption()方法只需要两个参数：

- 包含字段名称的字符串
- 要删除的选择值（不要与标签混淆）
### 设置文件可见
可能希望控制表单中字段或部分的可见性的原因有很多。也许给定字段仅在另一个字段中选择某个值时才适用；例如，许多公司有一个Close notes字段，只有当State字段设置为关闭值之一时才适用。
然而，在决定是否创建具有多个字段的表单视图并依赖客户端逻辑来隐藏它们时，而不是简单地对不同情况使用不同的视图时，重要的是要明智。这是因为每个字段，即使被客户端逻辑隐藏，也必须加载表单。这增加了用户网络和浏览器的负载。对于少数几个领域来说，这几乎是不值得注意的。但是，如果您有40个字段加载一个表单，但除少数情况外，所有字段都隐藏，这可能是糟糕的做法。
JET 中的客户端脚本上有两种g_form API 的方法，我们将在本章中讨论：setVisibile()和 setDisplay()。
g_form对象的setVisible()方法接受两个参数：字段名，后跟一个布尔值，该值指示字段是否应可见。如果第二个参数为真，则该字段将变成或保持可见。如果false，它会变成或保持隐形。一旦看不见，字段曾经所在的块仍然是空的，在表单中留下一个空隙。
g_form.setVisible('cmdb_ci', false); //使字段不可见
setDisplay()方法的工作方式完全相同，但它没有留下间隙，而是将包含字段的元素的displayCSS属性设置为none。这意味着它不会留下空白区域，并重新流化周围的字段布局，因此没有间隙：
g_form.setDisplay('cmdb_ci', false); //完全隐藏字段和块
字段可见性可以从客户端脚本和UI策略中控制。也就是说，只要有可能，最好使用UI策略来控制字段可见性，而不是客户端脚本。这是因为UI策略对性能的影响略低，也更优化。
### 获取和设置表单上的值
从在表单上运行的客户端脚本中获取和设置值可以使用g_form类的几种不同方法来完成，每种方法都有不同的目的：
getValue()方法接受一个参数：字段的name（不要与标签混淆）。它只是返回字段包含的值。如果该字段是引用字段，则值将是sys_id。如果是像状态字段这样的整数下拉列表，您将获得与当前选择相对应的数字，始终转换为字符串。
示例：
g_form.getValue('assigned_to'); //返回所分配用户的sys_id
正如您所料，g_form类的setValue()方法与getValue()方法正好相反。它接受两个参数而不是一个参数（字段名称，后跟新字段值），并将字段设置为表单中的新值。
示例：
g_form.setValue('state', '7'); // 将状态字段设置为关闭
请注意，虽然用户可以立即看到此更改，但在用户保存或提交表单之前，它不会更新服务器上的记录。
最后，我们有getReference()，这是接受回调函数的特殊方法之一，我们在本章开头对此进行了讨论。
getReference()方法接受引用字段的名称和回调函数的引用。向服务器发送AJAX查询以检索记录详细信息后，它会返回一个与引用字段中引用的记录相对应的GlideRecord对象。
此AJAX查询如果同步完成，将暂时锁定用户的浏览器，并导致糟糕的用户体验。然而，幸运的是，getReference()方法接受回调函数的事实意味着这可以异步完成。在下面的示例中，我们指定了一个匿名内联回调函数，而不是引用我们在其他地方声明的函数，但行为是相同的。
```
g_form.getReference('cmdb_ci', function(grCI) {  
    var category = g_form.getValue('category');  
    var grClass = grCI.getValue('sys_class_name');  
    if (grClass == 'cmdb_ci_server' && category !== 'hardware') {  
        g_form.setValue('category', 'hardware');  
    }  
}); 
```
在前面的示例中，我们从cmdb_ci字段获取引用记录。然后，我们以这种形式获得记录的类别，以及引用的CI中的sys_class_name。
最后，我们检查CI是否是服务器。如果是，但表单上选择的类别不是硬件，我们确保将其设置为该类别。这样，如果您选择的CI是服务器，则必须选择硬件作为类别。像这样的脚本在更改客户端脚本中最有效，每当cmdb_ci字段更新时都会运行。
### 设置必填字段和只读字段
控制用户是否可以修改字段值，以及是否需要字段值，可以分别使用GlideForm（g_form）API的两种方法来完成：setReadOnly()和setMandatory()。就像现场可见性一样，
setReadOnly()和setMandatory()都接受两个参数：字段名称和指示字段是否应为强制/只读的布尔值。
在以下示例中，我们将业务服务字段设置为只读，将配置项字段设置为必填项：
g_form.setReadOnly('business_service', true);g_form.setMandatory('cmdb_ci', true);
#### 备注
默认情况下，当表单上的引用字段只读时，引用图标会消失。不过，可以通过修改系统属性来逆转此行为：glide.ui.reference.readonly.clickthrough。
这些方法都没有返回值。
### 提交客户端表单
基于表单的客户端脚本可以使用两种GlideForm（g_form）方法之一提交表单（以及当时包含的任何更改）：save()和submit()。
save()方法将保存表单中存在的记录。它不接受参数，也不返回值。保存后，用户将被重定向回他们刚刚查看的相同表单，并加载相同的新记录或更新记录。
客户端GlideForm（g_form）对象的submit()方法的工作方式完全相同，只是它将将用户重定向到他们所处的上一页，而不是在保存后在表单中重新加载记录。您可以通过在UI操作脚本或客户端脚本中使用action.setRedirectUrl()来控制用户重定向到哪个页面。
#### 备注
请记住，即使字段不可见，它们仍然可以包含一个值。保存或提交时，隐藏字段的值也会提交！因此，在用于隐藏的相同脚本中清除字段通常是一个好主意。
### 禁用检查必填字段
g_form对象有很多有用的方法，但由于它们与GlideRecord API非常相似，因此我们将避免遍历每一个方法。有 getValue()、setValue() 等等。但是，它提供的唯一方法之一允许您禁用表单上的必填字段。
虽然这很少用于生产代码，但当您需要生成或保存记录以供测试时，这是一个很好的故障排除工具，但它对保存或进度记录之前需要填写哪些字段有复杂而烦人的规则（我正在看着您，更改请求！）
此属性非常简单，您只需编写g_form.checkMandatory = false;