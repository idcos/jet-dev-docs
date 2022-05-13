---
sidebar_position: 9
sidebar_label: 'GlideQueryCondition'
---
# GlideQueryCondition

GlideQueryCondition API主要是配合GlideRecord API进行数据查询时使用的，是对`query()`检索条件的增强。
作用是在当前查询上添加and或or查询条件，构造复杂的查询语句时使用。

GlideQueryCondition类下目前有两个方法：
- addCondition	添加and复合条件
- addOrCondition	添加or复合条件

代码示例：
```
var gr = new GlideRecord('offer');
var condition = gr.addQuery('office_location', '上海');
// 查询Offer状态为已发放并且Base地址在上海/杭州的简历
condition.addOrCondition('office_location', '杭州');
condition.addCondition('offer_status', '已发放');
gr.query();
while(gr.next()) {
    gs.print(gr.getValue('user_name')
    + '，年龄' + gr.getValue('age')
    + '，base地址' + gr.getValue('office_location'));
}
```
![查询示例](/img/server-side-glide-api/GlideQueryCondition-query.png)
