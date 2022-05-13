---
sidebar_position: 8
sidebar_label: 'GlideUser'
---

# GlideUser

JET中的 GlideUser API类提供了允许您获取当前用户信息的方法、他们的角色和权限以及他们的其他属性(如偏好)；所有这些都不需要依赖慢得多的 GlideRecord查询。
GlideUser类没有构造方法，一般是通过调用`GlideSystem`(即gs)方法来声明的，使用gs.getUser()方法返回GlideUser对象。

![我的任务业务规则](/img/business-rules/mine-tasks.png)

客户端脚本也可以通过g_user对象来使用GlideUser，初始化为当前登录的用户；GlideUser 客户端 API 使您可以轻松访问确定有关用户的某些详细信息，包括用户名、角色，以及通过服务器上的 API`sys_id`访问客户端数据集的方法。`putClientData()`

GlideUser ( `g_user`) API 特别有用，因为它消除了执行客户端 GlideRecord 查询来检索此信息的需要，这些查询在性能方面成本高昂或执行时间很慢。



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



## getPreference() 和 savePreference()--待实现

`getPreference()`方法_GlideUser 类允许您检索用户的用户偏好之一的值。用户首选项存储在`sys_user_preference`表中，包括给定列表中每页显示多少条记录，或者当前选择了哪些更新集。

此方法接受一个参数：包含要检索的首选项名称的字符串。

该`getPreference()`方法返回一个包含所请求首选项值的字符串，如果未定义此类首选项，则返回 null。

`savePreference()`在另一方面，接受两个参数：首选项的名称和设置它的新值。`savePreference()`不返回值。

### 示例用法

在这里，我们检查一个用户偏好以查看他们是否打开了注释。如果他们这样做，我们会在屏幕上显示一条附加消息：

```javascript
var currentUser = gs.getUser(); 
if (currentUser.getPreference('glide.ui.show_annotations') === 'true') { 
    gs.addInfoessage('This form is for [purpose]. You can do [instructions].'); 
} 
Next, we have a business rule that we can imagine runs on the sys_user table, and checks a custom field, u_show_annotations. When this field is toggled off, this business rule sets the user's preference the same way:

var currentUser = gs.getUser(); 
if (current.u_show_annotations.changesTo('false')) { 
    currentUser.savePreference('glide.ui.show_annotations', 'false'); 
} 
```



## hasRole()

这方法与其他共享其名称的方法一样，只是返回`true`或`false`指示用户是否具有指定的角色。它接受一个参数：要检查的角色的名称，并返回一个值：一个布尔值，指示用户是否具有该角色。

### 示例用法

这里有一个简单的例如，我们记录真假，取决于用户是否具有管理员角色：

```javascript
var currentUser = gs.getUser();
gs.info(currentUser.hasRole('admin')); 
```



## isMemberOf()

`isMemberOf()`几乎是功能与`hasRole(),`in相同它接受一个参数，并返回一个布尔值，指示用户是否具有该参数的成员资格。但是，不是检查用户是否具有指定的角色，而是检查用户`isMemberOf()`是否是特定组的成员。

这对于您希望确保只有某些组成员可以执行某些操作但（出于某种原因）您没有专门与之关联的角色的情况非常有用。有关这方面的示例，请参阅以下代码片段。

### 提示

请注意，除非将角色配置为提升的权限，否则如果您检查管理员是否具有给定角色，该`hasRole()`方法将始终返回。`true`但是，对于`isMemberOf()`. 管理员角色不会覆盖组成员资格查找。要在检查角色时避免这种行为，请使用`gs.hasRoleExactly()`.

### 示例用法

这里我们有一个检查用户是否是工单分配组成员的业务规则，如果不是，则拒绝更新。

```javascript
var currentUser = gs.getUser(); 
if (!currentUser.isMemberOf(current.assignment_group.getDisplayValue()) {
    current.setAbortAction(true);
}
```

请记住，业务规则并不是拒绝用户更新的最佳方式；我们只是用这个作为一个简单的例子。执行此功能的更好方法是通过 ACL 或可能用户界面/数据策略。

此外，如果我们编写此代码是为了在实际场景中使用，我们可能希望添加一条消息，指出更新被拒绝的原因，以避免糟糕的用户体验。