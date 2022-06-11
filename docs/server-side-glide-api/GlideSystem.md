---
sidebar_position: 7
sidebar_label: 'GlideSystem'
---

# GlideSystem

GlideSystem类在服务端不是使用new关键字构造的，而是通过可用于所有服务器端脚本gs。此API允许我们获取有关系统、用户会话、当前日期/时间（待实现）或其他有用信息的各种有用信息。它还允许您通过显示信息或错误消息与用户进行交互。   

示例代码:
```
  var dto = {name:'王杰', age:'42'};
  if (!gs.nil(dto)) {
      gs.print(dto.name);
  }
```

如下是GlideSystem API的一些常用的方法:
- print	输出信息到 syslog
- nil	对象是否为空
- include	引入
- getDisplayColumn	获取当前表标识为display的字段
- getDisplayValueFor	获取引用值
- flushMessages	清空当前会话消息
- addMessage	添加消息
- addInfoMessage	添加info消息
- addErrorMessage	添加错误消息
- getErrorMessages	获取错误消息
- getInfoMessages	获取info消息
- getMessage	获取消息
- getProperty	获取属性值
- getNewAppScopeCompanyPrefix 获取AppScope前缀，生成规则：x_{公司code}_
- getCurrentApplicationScope	获取当前应用Scope(sys_app中scope字段)
- getCurrentApplicationId 	获取当前应用id
- getPreference	获取引用值
- setProperty	设置属性值
- setRedirect	设置重定向
- setReturn	设置返回url
- tableExists	表是否存在
- userID	获取etynid
- getUserID	获取用户id
- getUserName	获取用户名
- debug	返回debug日志信息
- error	返回error日志信息
- info	返回日志信息
- warn	返回warn日志信息
- getSession	获取会话
- getSessionID	获取会话id
- getSessionToken 获取当前Token
- getUser	获取用户
- getUserDisplayName	获取用户展示名
- getUserNameByUserID	通过用户id获取用户名
- hasRole	是否有某个角色
- hasRoleInGroup	判断用户是否有指定用户组的相应角色


## addErrorMessage() and addInfoMessage()

`addErrorMessage()`方法将显示红色错误消息在浏览器窗口。同样的功能也可以通过客户端脚本使用`g_form`API ( `g_form.addErrorMessage()`) 来实现。

![错误提示](/img/business-rules/errorMsg.png)

`addInfoMessage()`使用方式相同，但它显示蓝色消息而不是红色错误消息。

### 示例用法

以下脚本可以在一个内部业务规则或其他服务器端脚本运行，并显示一条消息，提示该操作是否被允许：

```
if (current.active.changesFrom(false)) { 
    if (gs.hasRole('admin')) { 
        gs.addInfoMessage('This record has been re-activated'); 
    } else { 
        current.setAbortAction(true); 
        gs.addErrorMessage('This record can only be re-opened by an Administrator.'); 
    } 
}
```
## Debug()

`debug()`方法允许您将调试消息写入系统日志。此方法使用标准 `JavaMessageFormat`占位符替换图案。这意味着除了第一个参数（调试消息）之外，您最多可以指定五个附加参数，然后这些参数将用于替换由 {1}、{2} 等指示的消息中的变量。

### 示例用法

以下代码将如下所示的字符串写入调试日志：

**当前事件 INC0001234 的简短描述为“示例事件”，处于已解决状态。**

```
gs.debug(
    'The current incident, {0}, with a Short Description "{1}", is in a state of {2}.', 
    current.getValue('number'),    
    current.getValue('short_description'),
    current.state.getDisplayValue()
); 
```



## eventQueue()

GlideSystem ( `gs`) API的`eventQueue()`方法可以接受多个参数：首先，包含一个要触发的事件的字符串。可以在**系统策略**|**事件**| **注册表**中创建事件，事件的名称应在第一个参数中指定。

第二个参数是事件应该运行的上下文。大多数事件针对特定记录运行，因此该参数应该是一个 GlideRecord 对象，其中包含该事件对应的运行记录。

第三个和第四个参数是事件参数。事件参数可用于触发脚本或操作。例如，触发发送电子邮件通知的事件中的第一个或第二个参数可能包含收件人的电子邮件或用户`sys_id`。

此方法不返回值。

### 示例用法

以下`eventQueue()`方法调用将触发`incident.commented`从事件注册表调用的事件。该事件将在`current`中记录的上下文中触发，并且该事件`parameter1`将是当前用户的`sys_id`.

此事件可以使用提供给方法调用的其他参数触发其他操作，例如通知或者业务规则：

```
gs.eventQueue('incident.commented', current, gs.getUserID(), gs.getUserName());
```



## getProperty()

该方法可以访问系统属性（存储在`sys_properties`表中）以确定JET中的行为或功能。您甚至可以创建自己的自定义属性来控制自定义功能。

GlideSystem API的方法`getProperty()`，`gs`接受一个强制参数和一个可选参数：分别是属性名称，以及在未找到该属性时返回的默认值。它返回包含在属性中的值，如果未找到该属性，则返回第二个参数的值。如果未指定第二个参数且未找到该属性，则返回 null。

### 示例用法

在这里，我们根据`true`/`false`属性返回的值判断是否显示消息：

```
if (gs.getProperty('custom_app.module.show_message') === 'true') {
    gs.addInfoMessage('一些消息');
}
```

在前面的代码片段中，我们严格将属性值与字符串进行比较`true`，因为该`getProperty()`方法返回一个字符串值。



## getUser()

_GlideSystem API的`getUser()`方法不接受任何参数，返回当前登录用户对应的GlideUser对象的引用。

### 示例用法

在这里，我们得到当前登录用户的GlideUser对象，然后打印出用户的`sys_id`：

```
var currentUser = gs.getUser();
gs.info(currentUser.getID());
```


## getUserID()

GlideSystem 类的`getUserID()`方法不接受任何参数，并且简单地返回当前登录用户的`sys_id`.

### 示例用法

```
var gr = new GlideRecord('sys_user');
gr.get(gs.getUserID());
gs.info('当前用户所在公司是' + gr.getValue('company'));
```


## hasRole()

`hasRole()`方法用于检查当前登录的用户是否具有特定角色。它接受一个参数（用于检查用户是否拥有的角色名称，以及返回一个布尔值，判断用户是否具有指定的角色）。

### 示例用法

在这里，我们看到一个用户如果没有管理员角色，则执行业务规则脚本终止数据库操作：

```
if (!gs.hasRole('admin')) {
current.setAbortAction(true);
gs.addErrorMessage('Only Admins can perform this action.');
} 
```