---
sidebar_position: 4
sidebar_label: 'GlideElement'
---

# GlideElement

GlideElement 类通常不会自行实例化，但它用于GlideRecord 对象中的元素，并提供与它们交互的方法。元素一般是指记录中的数据列，例如任务记录上的数字字段，或者记录的`sys_updated_on`字段。您可以从服务器端 GlideRecord 对象访问 GlideElement（例如，通过使用`gr.number`），或使用GlideRecord 对象的方法`getElement`。

最佳实践要求您**始终**使用 getter 和 setter 函数：`getValue()`，`setValue()`如果您想获取或设置字段的值。但是，直接访问服务器端`GlideElement`属性将为您提供对 GlideElement 对象的引用。

例如，假设我们有一个包含名为 的列的表`short_description`。当我们查询这个表时，我们得到一个填充了各种属性的 GlideRecord 对象。表中的每个字段都成为 GlideRecord 对象中的一个属性，这些属性不仅仅是字符串和数字以及其他基本/原始数据类型，对应于记录中一个字段的每个属性都是一个对象，它都是 GlideElement 类的一个实例。

这意味着字段/属性 ,`gr.short_description`对应于**GlideElement**对象，具有您接下来将看到的所有方法，等等。我们将在这里介绍开发人员可能会相对频繁使用的最有用的方法。

事实上GlideRecord 的properties是**对象类型**而不是**原始类型**。也许您听说过 JavaScript 被称为**松散类型**语言。这不仅仅是一个口语术语。这意味着 JavaScript 中的数据类型是流动的。在许多其他语言中，当您声明一个变量并对其进行初始化时，您本质上将它链接到给定的数据类型：字符串、整数、浮点数、布尔值等。但是，在 JavaScript 中，您可以声明一个 `var`，将其设置为**字符串**值，然后修改其值，使其包含**数字**或任何其他类型——包括**原始类型**和**对象类型**。

这在某些情况下很有用；例如，您可以`short_description`使用一行代码设置字段的值，例如`gr.short_description = 'New short description.";`. 然而，正如我们在本章中多次提到的那样，这实际上可能是一件坏事，因为您正在覆盖曾经位于父 GlideElement 对象`short_description`属性中的 GlideElement 对象。但是，使用 GlideRecord 的` setValue()`方法可以正确设置 GlideElement 对象中的值。

出于同样的原因，由于 JavaScript 的**引用传递**特性，您不应使用以下语法设置变量：

```
var userReference = gr.assigned_to;
```

上面的代码行并不会将`userReference`变量设置为字段的值，而是会将变量设置为`gr.assigned_to`所指向的对象的引用。

想象一下，我们在变量 中有一个 GlideRecord 对象`gr`，带有一个应该给我们两个事件的查询： 首先 `INC0000001`，然后`INC0000002`。你认为最后一行会记录什么？

```
var incNumber; //声明上面的循环以防止重复声明
while (gr.next()) { //迭代两个返回的事件
    //如果数字是INC0000001
    if(gr.getValue('number') === 'INC0000001') {
        //将变量设置为gr的“数字”属性。
        incNumber = gr.number;
    }
gs.log(incNumber);
```

这里说明一下上面这个脚本实际上并不会打印出字符串值`‘INC0000001’`，这里正确的赋值方法应该是`incNumber = gr.getValue('number');`。因为`gr.number`拿到的其实是对象的引用，而不是像字符串这样的原始值，所以对变量`incNumber`赋值为`gr.number`对象的引用。并且当 while 循环完成时，`gr.number`包含一个具有 value 的对象`INC0000002`，所以即使我们设置了当`gr.number`值为`INC0000001`时对变量进行赋值，但是当最后一行运行时，同一个对象`gr.number`仍旧包含不同的值，因此对包含在变量中的对象的引用也应该包含不同的值：`INC0000002`。

如下是GlideElement API的一些常用的方法:
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
- canCreate	是否有权限插入记录
- canDelete	是否有权限删除记录
- canWrite	是否有权限更新记录
- canRead	是否有权限读表数据
- changes	检测记录是否有变化
- changesTo	检测记录是否变化为某个值