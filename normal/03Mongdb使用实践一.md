# Mongdb使用实践一

- [Mongdb使用实践一](#mongdb使用实践一)
    - [参考链接](#参考链接)
    - [使用过的一些sql](#使用过的一些sql)
    - [数据类型对应表参考](#数据类型对应表参考)

### 参考链接

- [MongoDB实战(5.x版本)](https://blog.csdn.net/ctflq/article/details/123944758)
- [Mongdb官网curd参考](https://www.mongodb.com/docs/manual/crud/)

### 使用过的一些sql

```sql
// 查看mongdb 的版本
db.version();

// 删除集合wx_center_resource_unit中的tenant_id字段
db.wx_center_resource_unit.update({},{$unset:{tenant_id:""}},{multi: true})

// 给集合wx_center_resource_unit添加字段的tenant_id，默认类型为long
db.wx_center_resource_unit.update({}, {$set: {tenant_id:NumberLong(0)}}, {multi: true})

// 查看集合wx_center_resource_unit 中字段tenant_id 的数据类型为2-字符串 的文档数量
db.wx_center_resource_unit.find({"tenant_id":{$type:2}}).count();

// 查看集合wx_center_resource_unit 中字段tenant_id 的数据类型为18-long 的文档数量
db.wx_center_resource_unit.find({"tenant_id":{$type:18}}).count();
db.wx_center_resource_unit.find({"tenant_id":{$type: "long"}}).count();

// 将集合wx_center_resource_unit 中字段tenant_id 的数据类型由字符串转换成long类型
// **测试未成功**
db.wx_center_resource_unit.find({"tenant_id":{$type:2}}).forEach(
function(x){
 x.tenant_id = NumberLong(x.tenant_id);
 db.wx_center_resource_unit.save(×)
});

```

### 数据类型对应表参考

<div align='center'><img src=./images/03Mongdb使用实践一/03Mongdb使用实践一_2022-12-14-14-15-58.png width='100%'/></div><br/>
