---
sidebar_position: 3
sidebar_label: 'compatibility'
---
JET几乎可以从任何具有符合现代标准的 Web 浏览器和有效的 Internet 连接的系统访问它，但是每个浏览器在执行客户端代码方面依然有细微的差别和障碍，出于这个原因，大多数 Web 应用程序（例如JET平台）都会推荐一种特定的 Web 浏览器，该浏览器可以最有效地呈现应用程序，并且最符合现代 Web 标准。对于 JET（与大多数 Web 应用程序一样），推荐的 Web 浏览器是 Google Chrome。

虽然曾经有一段时间，许多 Web 应用程序都需要 Internet Explorer，但现在几乎没有充分的理由必须使用它，除非您的 Web 应用程序需要一些专有且可能不安全的插件，例如 ActiveX。由于 Chrome 是 JET 的推荐浏览器，如果您在使用其他浏览器（尤其是 Internet Explorer）时发现任何错误，请尝试在 Chrome 或 Firefox 中加载站点/页面。知道问题的根源后，您可以排除故障或将问题报告给 JET，虽然 JET 确实推荐 Chrome，但我们确实希望该平台能够在每个现代浏览器中正常运行。

**Note**

为了解决与 Internet Explorer 的兼容性问题，您可以尝试禁用任何浏览器加载项，这可以通过运行命令来完成iexplore -extoff。您也可以尝试启用或禁用 Internet Explorer 的兼容模式，Chrome 的隐身模式也允许您在没有大多数扩展程序的情况下运行浏览器，这同样有助于解决兼容性问题。

兼容性也是避免在您的客户端脚本中使用直接文档对象模型( DOM )的另一个好理由,客户端 Glide API 尝试处理每个浏览器中的差异，以便自动处理所有的兼容性问题。