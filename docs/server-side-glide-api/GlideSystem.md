---
sidebar_position: 7
sidebar_label: 'GlideSystem'
---

# GlideSystem

GlideSystem 类在服务端不是使用new关键字构造的，而是通过变量可用于所有服务器端脚本gs。此 API 允许我们获取有关系统、用户会话、当前日期/时间或其他有用信息的各种有用信息。它还允许您通过显示信息或错误消息与用户进行交互。   

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