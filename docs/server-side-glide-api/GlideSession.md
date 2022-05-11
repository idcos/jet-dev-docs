---
sidebar_position: 6
sidebar_label: 'GlideSession'
---

# GlideSession

GlideSession 类有没有构造函数，但它是从`getSession()`方法返回的GlideSystem API `gs.getSession()`。此 API 允许您获取和设置与当前会话相关的数据。

如下是GlideSession API的一些常用的方法:
- getClientData	获取用户在当前会话保存的数据
- getClientIP	获取当前用户IP
- getCurrentApplicationId	获取会话当前应用ID
- getCurrentDomainID	获取会话当前域ID
- getLanguage	获取会话语言
- getSessionToken	获取会话Token
- getSessionRecord	查询Session对象
- getSessionData	查询Session中保存的数据
- putSessionData	保存数据到Session中
- getTimeZoneName	获取会话时区
- getUrlOnStack	获取堆栈中的URL,估计是最近一次访问的页面
- isLoggedIn	用户是否登录
- putClientData	保存数据到用户当前会话
- putCurrentData	保存会话当前状态的一些属性,像当前应用,时区,领域,语言等
- getRoles	获取用户角色