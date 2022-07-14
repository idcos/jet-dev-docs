---
sidebar_position: 2
sidebar_label: '如何使用Formatter'
---

# 如何使用Formatter

### 1.新建逻辑表
![image.png](/img/formatter/how-to-use/add-table.png)

### 2.新建UI宏

UI宏的xml里面可以直接写html结构、css样式、js脚本，同时也支持cdn的形式引入其他css、js文件，JET平台中已经内置了angular、jQuery框架库等，我们可以直接使用window.angular、window.jQuery进行使用（“window.”可以省略）

下面我们来实现一个简单的手动加减的计数器：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<j:jelly trim="false" xmlns:j="jelly:core" xmlns:g="glide" xmlns:j2="null" xmlns:g2="null">
  <style>
     .macro-content {
       padding: 20px 100px;
     }
  </style>
  <div class="macro-content">
   <div class="count">0</div>
    <button class="btn-add">增加</button>
    <button class="btn-decrease">减少</button>
  </div>
  <script>
   jQuery(".macro-content .btn-add").on("click", function() {
     var $count = jQuery(this).siblings(".count");
     var count = Number($count.text().trim());
    count = isNaN(count) ? 0 : count;
     $count.text(count + 1);
   })
    jQuery(".macro-content .btn-decrease").on("click", function() {
     var $count = jQuery(this).siblings(".count");
     var count = Number($count.text().trim());
    count = isNaN(count) ? 0 : count;
     $count.text(count - 1);
   })
  </script>

</j:jelly>
```
![image.png](/img/formatter/how-to-use/add-macro.png)

### 3.新建formatter，并引用第2步新建的UI宏
![image.png](/img/formatter/how-to-use/add-formatter.png)

### 4.在逻辑表form design中拖拽formatter使其在视图中显示
![image.png](/img/formatter/how-to-use/form-design-add-formatter.png)

### 5.formatter的渲染效果
![image.png](/img/formatter/how-to-use/render-result.gif)


