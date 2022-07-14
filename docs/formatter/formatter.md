---
sidebar_position: 1
sidebar_label: 'Formatter 简介'
---

# Formatter

## 什么是Formatter
Formatter 是用来展示记录字段之外的一些记录相关信息的表单元素，也就是说，formatter 并不是一个字段类型，也不存在于逻辑表的字段中。我们可以通过为表单配置 formatter 来展示想要的元素。
平台中的一些 Formatters 例子：

| Formatter | 描述 | 备注 |
| --- | --- | --- |
| Process flow formatter | 在记录上方展示一个表示不同阶段的步骤条 | 见示例 |
| Activity formatter | 在任务表单中展示活动列表、历史列表 | 暂无示例 |
| Parent breadcrumbs formatter | 以面包屑的形式显示当前任务记录的父子关联关系 | 暂无示例 |
| Approval summarizer formatter | 显示已审批请求的动态概要信息 | 暂无示例 |
| CI relations formatter | 在 CI 表单中，展示一个工具栏来查看当前 CI 和相关联 CI 的关系 | 暂无示例 |

> 需要注意的是，当把表单导出为 PDF 时，不会导出表单中的任何 formatters。


想要创建一个自定义的 formatter，首先要创建一个 UI Macro 来渲染 formatter 的主体，然后创建一个 formatter 和对应的 UI Macro 进行关联，最后才可以把 formatter 应用到 form 中。


## UI Macro
UI Macro (UI 宏)，计算机科学里的宏是一种抽象的,根据一系列预定义的规则替换一定的文本模式，解释器或编译器在遇到宏时会自动进行这一模式替换。对于编译语言，宏展开在编译时发生，进行宏展的工具常被称为宏展开器。宏这一术语也常常被用于许多类似的环境中，它们是源自宏展开的概念，这包括键盘宏和宏语言。绝大多数情况下，“宏”这个词的使用暗示着将小命令或动作转化为一系列指令。
	JET 平台中使用的宏，是使用 Jelly 模板语言来以 XML 格式编写的页面模板，然后以 UI Macro 的形式提供给 Formatter 使用，可以认为 Macro 定义了 Formatter 的内容，而基于 Jelly 编写的 XML 模板定义了 Macro 的内容。Jelly 是一个用来将 XML 转换成可执行代码的工具，如果你对于模板语言（模板引擎）如 JSP (.jsp)、Handlebars (.hbs)、Djongo 等有过了解，那么就能够理解这么做的好处。
	JET 平台界面中使用了 Angular 框架来构建视图，开发者编写 XML 然后经过 Jelly 转换成具体的 HTML 的片段，最后通过 ng 的 injector 来将计算后的 HTML 片段插入到指定的 DOM 区域，然后渲染到 UI 上。利用了 Angular 的 injector 机制配合 Angular 的 Dynamic HTML Template，就能够很方便的实现 HTML 注入了。
	到这里我们了解到了几个问题：

- Jelly 是较早的 Apache 开源的模板引擎工具，已经长时间没有维护。
- JET 使用的 UI 框架为 React，使用 Jelly 转换的 HTML 并不能直接注入，除非没有动态内容的纯 HTML，JET平台引入了AngularJS，搭建了一套中间层AngularJS的渲染组件，让JET平台能够渲染Angular Template HTML，以及其脚本的执行
:::

## 关于Jelly
Jelly 是一个基于 XML 的脚本引擎。基本思想是 XML 元素可以绑定到 Java_标记_，Java 标记是执行某些功能的 Java bean。

Jelly 完全可通过自定义操作（以类似于 JSP 自定义标签的方式）以及与 Jexl、Velocity、pnuts、beanshell 等脚本语言以及通过 JavaScript 和 JPython 等 BSF（Bean 脚本框架）语言完全集成

[了解更多请参考jelly官方文档](https://commons.apache.org/proper/commons-jelly/overview.html)

### Java服务端样例代码

1. pom.xml引入commons jelly三方库
```xml
<!-- https://mvnrepository.com/artifact/commons-jelly/commons-jelly -->
        <dependency>
            <groupId>commons-jelly</groupId>
            <artifactId>commons-jelly</artifactId>
            <version>1.0.1</version>
        </dependency>
```

2. 样例代码
```java
import org.apache.commons.jelly.JellyContext
import org.apache.commons.jelly.XMLOutput

OutputStream output = new FileOutputStream("demo-jelly-page.html");
JellyContext context = new JellyContext();

context.setVariable("name", "张三");
context.setVariable("background", "#FF0000");
context.setVariable("url", "https://commons.apache.org/proper/commons-jelly/images/logo.jpg");
// Set the hobby list
context.setVariable("hobbies", ['钓鱼','玩游戏']);

XMLOutput xmlOutput = XMLOutput.createXMLOutput(output);
context.runScript("demo-jelly.xml" , xmlOutput );
xmlOutput.flush();
```

3. 样例jelly文档
```xml
<?xml version="1.0"?>
   <j:jelly trim="false" xmlns:j="jelly:core" xmlns:x="jelly:xml" xmlns:html="jelly:html">
     <html>
       <head>
         <title>${name}'s Page</title>
       </head>
       <body bgcolor="${background}" text="#FFFFFF">
         <h1>${name}'s Homepage</h1>
         <img src="${url}"/>
         <h2>My Hobbies</h2>
         <ul>
           <j:forEach items="${hobbies}" var="i">
             <li>${i}</li>
           </j:forEach>
         </ul>
       </body>
     </html>
   </j:jelly>
```

4. 样例输出文档
```html

<html>
  <head>
    <title>张三's Page</title>
  </head>
  <body bgcolor="#FF0000" text="#FFFFFF">
    <h1>张三's Homepage</h1>
    <img src="https://commons.apache.org/proper/commons-jelly/images/logo.jpg"></img>
  <h2>My Hobbies</h2>
  <ul>
    
    <li>钓鱼</li>
    
    <li>玩游戏</li>
    
  </ul>
  </body>
</html>
   
```