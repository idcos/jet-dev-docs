---
sidebar_position: 2
sidebar_label: 'GlideAjax'
---
GlideAjax 类允许客户端脚本对服务器进行 AJAX（异步 JavaScript 和 XML）调用，包含执行服务器端脚本，甚至返回值。GlideAjax可能是ServiceNow中最不了解的客户端API之一，部分原因是它既有客户端，也有服务器端组件。

首先，让我们讨论一下服务器端组件。来自客户端脚本的 GlideAjax 调用最终将执行一个脚本包含（正如我们在上一章中了解到的那样，它是一个服务器端脚本），因此让我们为 GlideAjax 脚本创建一个脚本包含以进行通信。

对于我们的示例，我们将创建一个简单的脚本包含，该脚本包含将返回我们指定的系统属性的值，因此我们将创建一个名为 GetPropertyAjax 的脚本包含。输入此名称，然后从“名称”字段中跳出选项卡后，脚本包含的默认基架将填充到“脚本”字段中。

例如，我们将创建一个简单的脚本包含，该脚本将返回我们指定的系统属性的值，因此我们将创建一个名为GetPropertyAjax的脚本包含。一旦我们输入此名称，然后从名称字段中退出选项卡，脚本包含的默认脚手架将填充到脚本字段中。
不过，为了使此脚本包括通过GlideAjax从客户端脚本访问，我们需要检查客户端可调用的勾选框。这将更改脚本字段的内容，以便我们的脚本包括扩展AbstractAjaxProcessor类。它还删除了初始化方法，因为我们不会从服务器初始化该类。
接下来，我们将定义GlideAjax脚本的方法，包括检索系统属性的值并将其返回给客户端的工作。
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
在上一个代码的getProp方法中，您会注意到我们正在调用 this.getParameter()。这是一种特定于AbstractAjaxProcessor类的方法，我们正在扩展。这意味着AbstractAjaxProcessor类的所有方法现在也是我们创建的GetPropertyAjax类的成员方法；因此，当我们访问类内this对象的方法时，我们也能够访问这些方法。特别是，这种方法检索我们从客户端脚本调用此GlideAjax脚本时发送的sysparm的值。
说到我们的客户端脚本，让我们构建它。首先，我们需要实例化客户端上的GlideAjax类实例，将GlideAjax脚本包含名称传递到客户端构造函数中；之后，我们需要使用addParam()方法来指定两个参数：sysparm_name和sysparm_prop_name。下面是这个样子：
```
var ga = new GlideAjax('GetPropertyAjax');  
ga.addParam('sysparm_name', 'getProp');  
ga.addParam('sysparm_prop_name', 'glide.servlet.uri'); 
```
每当我们从客户端使用GlideAjax时，sysparm_name参数都是必需的：它告诉服务器上的GlideAjax脚本我们要运行哪种方法。但是，sysparm_prop_name参数，我们已经自定义了。它只是我们同意在服务器端脚本上期望包含的参数的名称，并从客户端脚本中提供;就像函数参数变量名称一样，除了我们必须在服务器上使用 this.getParameter（） 手动检索它。
不过，还有更多；当GlideAjax脚本返回值时，它不会像正常函数一样返回。例如，你不能只做以下事情：
```
var answer = new GlideAjax('MyGlideAjax').myMethod(); 
```
相反，在指定所有必要的参数后，我们在客户端脚本中调用GlideAjax类的另一个方法：getXMLAnswer()。此方法接受一个参数：回调函数，如以下代码所示：
```
var ga = new GlideAjax('GetPropertyAjax'); //Pass in Script Include name  
ga.addParam('sysparm_name', 'getProp'); //Method name
ga.addParam('sysparm_prop_name', 'glide.servlet.uri'); //Name of the property to retrieve
ga.getXMLAnswer(ajaxCallback); 
 
function ajaxCallback(answer) {  
    console.log('The base instance URI property is ' + answer);  
} 
```
在前面的示例中，我们将对ajaxCallback函数的引用传递给getXMLAnswer()方法。完成后，调用ajaxCallback函数。回调函数在客户端上调用一个参数：从sysparm_name参数中指定的方法返回的值转换为字符串。如果返回对象，则传递到回调函数中的参数将是JSON格式的字符串。然后，您可以使用JSON.parse()将字符串解析回JSON对象。
还有一件事需要注意，getXMLWait()方法异步运行。这意味着，您无法确定执行顺序（即何时会与您的代码的其余部分一起运行）。请考虑以下示例：
正如您在上一个屏幕截图中看到的，最后一行代码在顶部回调函数中的console.log()行之前执行。
