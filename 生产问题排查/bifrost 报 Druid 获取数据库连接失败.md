生产环境存在 260 台 tiangong-api，在批量导入系统字段后，260 台 tiangong-api 会更新缓存，每个系统字段都需要 RPC 调用 bifrost 查询系统字段。

当前生产环境中有 4 台 bifrost，每台 bifrost 的 Druid 的最大连接数为 200，最小为 30，每台 bifrost 都有报 Druid 获取数据库连接失败的错误。并且 tiangong-api 有报 获取 dubbo 连接失败

## 疑问点

虽然批量导入了多个字段，但是每台 tiangong-api 都是单线程 rpc 调用 bifrost，对于 4 台 bifrost 来说，单点压力并不是很大呀。而已 bifrost 中的最大连接数是 200，数据库总的连接数也有很大的空余，为什么还会出现 bifrost Druid 获取数据库连接失败？



rpc 调用 bifrost 的查询逻辑也很简单，只涉及到一个查询数据库的操作，而且查询的表是一个数据量在 3 千多的小表