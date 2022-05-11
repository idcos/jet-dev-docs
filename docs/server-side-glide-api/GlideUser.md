---
sidebar_position: 8
sidebar_label: 'GlideUser'
---

# GlideUser

JET中的 GlideUser API类提供了允许您获取当前用户信息的方法、他们的角色和权限以及他们的偏好；所有这些都不需要依赖慢得多的 GlideRecord查询。
GlideUser类没有构造方法，一般是通过调用`GlideSystem`(gs)方法来声明的，使用gs.getUser()方法返回GlideUser对象。

![我的任务业务规则](/img/business-rules/mine-tasks.png)

如下是GlideUser API的一些常用的方法:
- getCompanyID	获取当前用户的公司ID
- getDisplayName	获取当前用户的显示名称
- getDomainID	获取当前用户现在的域ID   --待实现
- getEmail	获取当前用户的邮箱
- getFirstName	获取当前用户的FirstName
- getID	获取当前用户的SYS_ID
- getLastName	获取当前用户的LastName
- getName	获取当前用户的登录名
- getPreference	获取当前用户偏好设置
- getRoles	获取当前用户所有的角色名称(含用户组的角色,继承的角色和角色中包含的)
- getUserRoles	获取分配给当前用户的角色,不含用户组和角色中包含的角色
- getUserByID	查询指定的用户并返回
- hasRole	当前用户是否有指定的角色,逗号分隔多个角色名称
- isMemberOf	当前用户是否是指定的用户组成员
- savePreference	保存当前用户的偏好设置
- getMyGroups	获取当前用户的用户组ID列表