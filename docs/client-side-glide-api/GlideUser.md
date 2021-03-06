---
sidebar_position: 4
sidebar_label: 'GlideUser(g_user)'
---
***注：putClientData暂未实现***  
GlideUser 对象作为 g_user 对象提供给**客户端脚本**，该对象初始化为已登录的当前用户。
GlideUser 客户端 API 使您可以轻松访问以确定有关用户的某些详细信息，**包括用户名、角色、sys_id 以及通过服务器上的 putClientData() API 访问客户端数据集的方法**。
GlideUser(g_user)API 非常实用，因为它无需执行客户端 GlideRecord 查询来检索此信息，这些信息在性能方面成本高昂，执行时间缓慢。
### 获取用户详细信息
可以从 GlideUser(g_user) API 中收集相当数量的用户配置文件详细信息，因为它们在加载页面时被缓存。有一种方法可以访问用户的全名：getFullName()，此方法不接受参数，但返回一个包含当前用户全名的字符串，用户的名字和姓氏可以通过 g_user 对象的属性获得，无需调用方法即可**直接访问**属性。  
例如：
```
var userFirstName = g_user.firstName;
```
通过 g_user 对象直接访问的其他属性包括用户的姓氏 (lastName)、sys_id(userID) 和用户的用户名 (userName)。任何这些属性都可以直接从 g_user 对象访问。
### 检查用户权限
用户的权限通常由分配给他们的角色决定，JET 提供了 g_user 对象的几种方法来确定用户是否具有指定的角色或一组角色。
要确定用户是否具有特定角色，例如 itil 角色，您可以调用 hasRole()方法，此方法接受一个参数为您正在检查的角色的名称，它返回一个布尔值，指示用户是否具有该角色。
```
if (g_user.hasRole('itil')) {  
    //do something  
} 
```
如果用户具有 admin 角色，此检查将**始终返回 true**（除非在检查 thesecuritysecurity_admin 角色时可能），然而有一种方法可以检查用户是否具有该角色，同时忽略管理员重写功能：g_user 对象的 **hasRoleExactly()**方法。只有当用户指定了确切的角色时，此方法才会返回 true，而不是检查用户是否具有该角色或管理员角色。
```
if (g_user.hasRoleExactly('itil')) {  
    //do something  
} else if (g_user.hasRole('admin')) {  
    //do something different
} 
```
这些方法很有用，但一次检查一个角色列表会很不方便。幸运的是，有一种方法可以从逗号分隔的列表中一次检查多个角色：**g_user.hasRoleFromList() 检查多个角色**。注意，此方法**仅接受逗号分隔的字符串，不接受数组**，您可以声明角色数组，但必须使用 .join(',') 将它们组合成逗号分隔的字符串，如下所示：
```
var rolesList = [
    'catalog_admin', 
    'catalog_editor', 
    'catalog_item_designer', 
    'catalog_manager'
];  
if (g_user.hasRoleFromList(rolesList.join(','))) {  
    //Do something catalog-related  
} 
```
### 获取客户端数据(暂未实现)
正如我们在上一章中了解到的，服务器端脚本可以使用 GlideSession API 的 **putClientData()** 方法（可通过使用 **gs.getSession()** 访问）将值与用户的会话相关联，以便可以使用 g_user.getClientData() 从客户端脚本访问数据。此方法接受一个参数：密钥（必须与在当前页面加载之前运行的业务规则或其他服务器端代码中与用户会话关联的某些数据的密钥匹配），getClientData() 返回与该键关联的字符串值。
```
var clientDataValue = g_user.getClientData('key');
```