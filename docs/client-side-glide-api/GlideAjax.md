---
sidebar_position: 2
sidebar_label: 'GlideAjax'
---
***注：getXMLAnswer方法暂未实现***  
GlideAjax 类允许客户端脚本对服务器进行 **AJAX（异步 JavaScript 和 XML）调用**，包含执行服务器端脚本，甚至返回值。GlideAjax 可能是 JET 中最不容易理解的客户端 API 之一，部分原因是它**既包含客户端，也包含服务器端**。

首先，让我们讨论一下**服务器端脚本**。来自客户端脚本的 GlideAjax 调用最终将执行一个 **script Includes**（正如我们在上一章中了解到的那样，它是一个服务器端脚本），因此让我们为 GlideAjax 脚本创建一个 script Includes以进行通信。
例如，我们将创建一个简单的 script Includes，该脚本将返回我们指定的系统属性的值，因此我们将创建一个名为 GetPropertyAjax 的 script Includes。一旦我们输入此名称，然后提交 name 这个标签，script Includes的默认基架将填充到 Script 字段中。
但是，要使此 script Includes可以通过 GlideAjax 从客户端脚本访问，我们需要选中客户端可调用复选框。这将改变脚本字段的内容，以便我们的 script Includes扩展抽象 AjaxProcessor 类，它还删除了初始化的方法，因为我们不会从服务器对这个类进行初始化。  
![GlideAjax](/img/client-side-glide-api/GlideAjax.png)  
接下来，我们将定义 GlideAjax script Includes的方法，该方法将执行检索系统属性的值并将其返回给客户端的工作。
```
var GetPropertyAjax = Class.create(); 
GetPropertyAjax.prototype = Object.extendsObject(AbstractAjaxProcessor, {  
    getProp: function() {  
        var propName = this.getParameter('sysparm_prop_name');  
        return(gs.getProperty(propName));  
    },   
    type: 'GetPropertyAjax'  
}); 
```
在上面代码的 getProp 方法中，您会注意到我们调用 this.getParameter()。这是一个特定于 AbstractAjaxProcessor 类的方法，我们正在对其进行扩展。这意味着抽象 AjaxProcessor 类的所有方法现在也是我们创建的 GetPropertyAjax 类的成员方法；因此，当我们在类中访问 this 对象的方法时，我们也能够访问这些方法。特别的是，它检索当我们从客户端脚本调用此 GlideAjax script Includes时，我们将发送的 sysparm 的值。

说到我们的**客户端脚本**，让我们来构建它。对于初学者，我们需要在客户端上实例化 GlideAjax 类的实例，将 GlideAjax script Includes名称传递到客户端构造函数之后，我们需要使用 addParam() 方法指定两个参数：sysparm_name 和 sysparm_prop_name。这可能是下面这样：
```
var ga = new GlideAjax('GetPropertyAjax');  
ga.addParam('sysparm_name', 'getProp');  
ga.addParam('sysparm_prop_name', 'glide.servlet.uri'); 
```
每当我们从客户端使用 GlideAjax 时，sysparm_name 参数都是必需的，它告诉服务器上的 GlideAjax 脚本我们要运行哪种方法，但是对于 sysparm_prop_name 参数我们已经自定义了，它只是我们同意在服务器端script Includes上期望的参数的名称，并从客户端脚本提供，就像函数参数变量名称一样，除了我们必须在服务器上使用 this.getParameter() 手动检索它。
不过当 GlideAjax 脚本返回一个值时，它不会像普通函数那样返回该值，例如，你不能只做如下的事情：
```
var answer = new GlideAjax('MyGlideAjax').myMethod(); 
```
相反，在指定了所有必要的参数之后，我们在客户端脚本中调用了 GlideAjax 类的另一个方法：getXMLAnswer()。此方法接受一个参数：回调函数，如下面的代码所示：
```
var ga = new GlideAjax('GetPropertyAjax'); //Pass in Script Include name  
ga.addParam('sysparm_name', 'getProp'); //Method name
ga.addParam('sysparm_prop_name', 'glide.servlet.uri'); //Name of the property to retrieve
ga.getXMLAnswer(ajaxCallback); 
 
function ajaxCallback(answer) {  
    console.log('The base instance URI property is ' + answer);  
} 
```
在前面的示例中，我们将对 ajaxCallback 函数的引用传递给 getXMLAnswer() 方法。完成后，调用 ajaxCallback 函数。回调函数在客户端上调用一个参数：从 sysparm_name 参数中指定的方法返回的值转换为字符串。如果返回对象，则传递到回调函数中的参数将是 JSON 格式的字符串。然后，您可以使用JSON.parse() 将字符串解析回 JSON 对象。
还有一件事需要注意，**getXMLWait() 方法异步运行**，这意味着，您无法确定执行顺序（即何时会与您的代码的其余部分一起运行）。

#### 参考示例：
```
// 弹出 SayHello 对话框，确认后调用接口，接收返回值，
function hello() {
  // 点击对话框确认按钮时的回调
  var callback = function() {
    var username = g_user.getUsername();
    
    var helloFn = new GlideAjax("HelloWorld"); //构造器参数为脚本库名称
    helloFn.addParam("sysparm_name", "sayHello"); //调用的方法
    helloFn.addParam("sysparm_user_name", username);//其他参数
    
    var done = function(response) {
     	alert(JSON.stringify(response));
    	reloadWindow();
    }
    
    helloFn.getJson(done.bind(this));
  }
  
  // 构造对话框
  var dlg = new GlideModal();
  dlg.setTitle(`Say Hello`);
  dlg.setOnOk(callback.bind(this));
  dlg.render();
}
```
