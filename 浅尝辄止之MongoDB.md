###1 简介
	MongoDB是一个介于关系数据库和非关系数据库之间的产品，基于分布式文件存储的数据库，旨在为WEB应用提供可扩展的高性能数据存储
	解决方案。MongoDB将数据存储为一个文档，数据结构由键值对组成。类似于JSON对象，字段值可以包含其他文档，数组及文档数组。
	eg：
	{
		name: "lc",
		age: 26,
		status: "A",
		groups: ["news","sports"]
	}
	对比MySql：
	1.更高的写入负载
	默认情况下，MongoDB更侧重高数据写入性能，而非事务安全，MongoDB很适合业务系统中有大量“低价值”数据的场景。但是应当避免在高
	事务安全性的系统中使用MongoDB，除非能从架构设计上保证事务安全。
	2.高可用性
	MongoDB的复副集(Master-Slave)配置非常简洁方便，此外，MongoDB可以快速响应的处理单节点故障，自动、安全的完成故障转移。这
	些特性使得MongoDB能在一个相对不稳定（如云主机）的环境中，保持高可用性。
	3.便于数据扩展
	在一些传统RDBMS中，增加一个字段会锁住整个数据库/表，或者在执行一个重负载的请求时会明显造成其它请求的性能降级。通常发生在
	数据表大于1G的时候（当大于1TB时更甚）。 因MongoDB是文档型数据库，为非结构货的文档增加一个新字段是很快速的操作，并且不会
	影响到已有数据。另外一个好处当业务数据发生变化时，是将不在需要由DBA修改表结构。
	4.基于位置的数据查询
	MongoDB支持二维空间索引，因此可以快速及精确的从指定位置获取数据。

###2 安装
	略，参照最新MongoDB教程安装指南即可。
###3 体验
	切换数据库——
	> use tutorial
	创建待插入数据——
	> post={"title":"My Blog Post",
	... "content":"Here's my blog post",
	... "date":new Date()}
	创建数据结果——
	{
        "title" : "My Blog Post",
        "content" : "Here's my blog post",
        "date" : ISODate("2018-10-18T06:02:27.030Z")
	}
	插入数据——
	> db.blog.insert(post)
	插入数据结果——
	WriteResult({ "nInserted" : 1 })
	查询数据——
	> db.blog.find()
	查询结果——
	{ "_id" : ObjectId("5bc821fd20fe9678fbf036a0"), "title" : "My Blog Post", "content" : "Here's my blog post", "date" : ISODate("2018-10-18T06:02:27.030Z") }
	按条件查询——
	> db.blog.findOne({"title":"My Blog Post"})
	按条件查询结果——
	{
        "_id" : ObjectId("5bc821fd20fe9678fbf036a0"),
        "title" : "My Blog Post",
        "content" : "Here's my blog post",
        "date" : ISODate("2018-10-18T06:02:27.030Z")
	}
	更新数据——
	> post.comment=[]
	> db.blog.update({title:"My Blog Post"},post}
	> db.blog.find()
	更新结果——
	{ "_id" : ObjectId("5bc821fd20fe9678fbf036a0"), "title" : "My Blog Post", "content" : "Here's my blog post", "date" : ISODate("2018-10-18T06:02:27.030Z"), "comment" : [ ] }
	删除——
	> db.blog.remove({title:"My Blog Post"})
	删除查询——
	db.blog.find()
	查询结果为空——
######End