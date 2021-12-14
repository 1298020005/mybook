### gin技术文档

>https://cloud.tencent.com/developer/article/1610678

#### quick start



##### 软删除

>https://gorm.io/zh_CN/docs/delete.html   中间有个软输出目录

如果您的模型包含一个`gorm.DeletedAt`字段（包含在 中`gorm.Model`），它将自动获得软删除能力！

调用 时`Delete`，记录不会从数据库中删除，但 GORM 会将`DeletedAt`的值设置为当前时间，并且不再使用正常的 Query 方法查找数据。
