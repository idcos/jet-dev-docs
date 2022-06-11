---
sidebar_position: 2
sidebar_label: 'script-includes'
---

## script-includes(服务端脚本)

**执行于：服务器**

​		script-includes 是一个可重用的代码存储库。它在服务器上执行，不应用于页面或加载到表单中。任何在**服务器端**执行并与脚本包含在同一范围内的代码，都可以像调用系统中的其他 API一样调用它。

​		script-includes 不会像业务规则那样由数据库操作触发，也不会像客户端脚本那样由事件触发。相反，script-includes脚本存储在于服务器上并通过其他服务器端脚本调用。您不能直接从客户端脚本调用script-includes ，但您可以通过扩展 GlideAjax 类使script-includes 从客户端脚本（尽管它仍然在服务器上执行）可访问。更多关于 GlideAjax以及如何使用它在以后的章节中会讲到。现在，只需要清楚script-includes 在服务端执行，通常由另一个服务器端脚本调用使用。

![script-includes示例](/img/server-side-scripting/script-includes示例.png)

​		每个script-includes在 JET 中定义一个类或函数，可在其范围内的任何服务端脚本中访问。脚本包含记录本身的名称必须与`Script`记录字段中的函数或类的名称匹配，因为这也是 API 名称。

服务端脚本可以在应用程序导航器中的**系统定义**下找到，其默认结构如下所示：

```javascript
var ExampleScriptInclude = Class.create(); 
ExampleScriptInclude.prototype = { 
    initialize: function() { 
        //Constructor 
    }, 
    aMethod: function() { 
        //Do some stuff 
    }, 
    anotherMethod: function() { 
        //Do some other stuff 
    }, 
    type: 'ExampleScriptInclude' 
}; 
```

​		如果我们在`initialize()`方法中接受参数，我们就可以在构造对象时指定参数。例如，我们可以将初始化函数描述更改为如下所示：

```
initialize: function(constructorArg) { 
    this.constructorArg = constructorArg; 
} 
```

现在，当从这个类实例化一个对象时，我们可以指定一个构造函数参数，如下所示：

```
var esi = new ExampleScriptInclude('arg value');
```

使用前面的代码，局部变量`constructorArg`将被设置为`'arg value'`，并且可用于类中的任何其他函数`this.constructorArg`。

尽可能不要修改开箱即用的 JET 服务端脚本。相反，创建一个副本并在其中修订您的更改，添加功能而不是去修改它，您还可以通过更改类似于以下代码突出显示部分的声明来扩展现有的服务端脚本，这扩展了 OOB（out-of-the-box开箱即用）`Cart`script-includes：

```javascript
var test = Class.create(); 
test.prototype = Object.extendsObject(Cart, { 
customMethod: function() { 
//Your code here 
}, 
    type: 'test' 
}); 
```

### 提示

最好避免对 OOB script-includes以及任何 OOB 记录进行更改。

- 修改 OOB 记录意味着未来的升级将无法更新它们。例如，如果您将实例升级，它可能会更改`cart`的某些功能。但是，如果`cart`服务端脚本已被您的实例中的某个人修改而不是被扩展，则升级过程将无法修改该服务端脚本，并且您将需要手动解决升级错误。
- 升级错误，也称为冲突，在升级过程中很常见，并不是什么大问题，但您希望尽可能避免，因为冲突会随着时间的推移而增加，并会在升级过程浪费大量时间和精力。
- 如修改服务端脚本现有记录，则产生的更改的会导致引用或直接使用 OOB 脚本的任何其他脚本发生中断。