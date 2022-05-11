---
sidebar_position: 3 
sidebar_label: 'GlideRecord'
---

# GlideRecord

GlideRecord 类是最普遍和有用的类之一在 JET中。它的主要功能是查询数据库表，并显示该表中与给定查询匹配的每条记录对应的值。它还可用于添加、修改或删除记录。GlideRecord 对象由属性组成，其名称对应于表中的每个字段。在客户端
Glide API 中，这些属性通常包含字符串，而在服务器端 API 中，这些属性包含具有自己的方法和属性的 GlideElement JavaScript 对象。

使用GlideRecord 查询可以有两种方式：1.使用sys_id直接查询 2. 根据field查询条件进行查询。   
示例代码：
```
    // 根据 sys_id 进行查询
    var sys_id = 'sys_id';
    var gr1 = new GlideRecord('project');
    // 添加sys_id查询条件
    gr1.get(sys_id);
    gs.print(gr1.getValue('project_name'));
    
    // 根据字段查询条件进行查询
    var gr2 = new GlideRecord('x_yunji_jet_05_offer_info');
    // 定义查询条件
    gr2.addQuery('sys_id', sys_id);
    // 执行
    gr2.query();
    gs.print('查询结果集数目：' + gr2.resultSet.total);
    // 遍历结果集
    while(gr2.hasNext()) {
          var dto = gr2.next();
          // 获取属性并打印
          gs.print(dto.project_name);
    }

```
如下是GlideRecord API的一些常用的方法:
- query 查询记录
- getValue 获取字段值
- setValue 设置字段值
- hasNext 是否有下一条记录
- next 获取下一条记录
- insert 插入记录
- update 更新记录
- addActiveQuery 查询active记录列表
- addInactiveQuery 查询active记录为false列表
- addNotNullQuery 非空条件查询
- addNullQuery 空条件查询
- addQuery 添加查询条件
- deleteMultiple 按条件删除
- udpateMultiple 按条件更新
- deleteRecord 删除记录
- getRowCount 获取行数
- orderBy 升序排序
- orderByDesc 降序排序
- setLimit 限定条数
- setAbortAction 设置中断标记
- getRecordClassName 获取当前记录的表名
- get 获取满足某字段条件的记录
- updateWithReferences 更新引用表记录值
- setQueryReferences 设置是否用引用值来做条件查询
- setDisplayValue 设置引用值
- insertWithReferences 带引用值插入
- getDisplayValue 获取引用值
- addJoinQuery 添加join条件
- getED 获取元素描述
- getElement 获取指定的element
- getEncodedQuery 获取编码后的查询条件
- addEncodedQuery 已编码查询条件查询
- getAttribute 获取字段属性
- getTableName 获取表名
- isValidRecord 当前记录是否有效
- isNewRecord 是否新记录
- getFields 获取字段列表
- getLabel 获取字段标签
- canCreate 是否有权限插入记录
- canDelete 是否有权限删除记录
- canWrite 是否有权限更新记录
- canRead 是否有权限读表数据
- changes 检测记录是否有变化
- isValid 当前表是否有效
- isValidField 当前字段是否有效
- operation 是滞更新，插入，删除操作
- setNewGuid 设置新的guid值
- setNewGuidValue 指定guid值