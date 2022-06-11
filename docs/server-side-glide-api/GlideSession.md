---
sidebar_position: 6
sidebar_label: 'GlideSession'
---

# GlideSession

GlideSession 类有没有构造函数，但它是从`getSession()`方法返回的GlideSystem API `gs.getSession()`。此 API 允许您获取和设置与当前会话相关的数据。

如下是GlideSession API的一些常用的方法：
- getClientData	获取用户在当前会话保存的数据
- putClientData	保存数据到用户当前会话
- getClientIP	获取当前用户IP
- getCurrentApplicationId	获取会话当前应用ID
- getSessionToken	获取会话Token
- isLoggedIn	用户是否登录
- getRoles	获取用户角色
- getTimeZoneName	获取会话时区 - 待开发
- getUrlOnStack	获取堆栈中的URL,最近一次访问的页面 - 待开发
- isInteractive  是否为交互式会话 - 待开发
- isImpersonating  是否在模拟其他用户 - 待开发
- getCurrentDomainID	获取会话当前域ID - 待开发
- getLanguage	获取会话语言 - 待开发



## getClientData() and putClientData()

和方法允许您在用户会话中`getClientData()`从`putClientData()`服务器端脚本使用`putClientData()`, 然后检索该数据使用`getClientData()`来自客户端脚本。

此功能对于在服务器和客户端之间来回传递数据非常有用，无需耗费性能的同步查询或耗时的 Ajax 调用。通常显示业务规则和使用`g_scratchpad`会更有意义，但这些 API 也有很多用例。

### 示例用法

在业务规则中在**Incident**表上，我们可以将一些数据放在用户的客户端会话中，如下所示：

```javascript
var userSession = gs.getSession(); 
session.putClientData('ticket_number', current.getValue('number')); 
```

然后在客户端脚本，您可以像这样访问这些数据：

```javascript
var ticketNumber = g_user.getClientData('ticket_number'); 
```