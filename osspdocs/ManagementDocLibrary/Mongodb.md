#create db
use DATABASE_NAME
db.DATABASE_NAME.insert({"name":"菜鸟教程"})

#show dbs

show dbs

#drop database
db.dropDatabase()

#insert collection
db.COLLECTION_NAME.insert(document) or db.COLLECTION_NAME.save(document)

#update
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)


    query : update的查询条件，类似sql update查询内where后面的。
    update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
    upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
    multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
    writeConcern :可选，抛出异常的级别。

or 

db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)

    document : 文档数据。
    writeConcern :可选，抛出异常的级别。

# remove

db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)

    query :（可选）删除的文档的条件。
    justOne : （可选）如果设为 true 或 1，则只删除一个文档。
    writeConcern :（可选）抛出异常的级别。

# find

db.COLLECTION_NAME.find() 

or 

db.col.find().pretty() #format


等于 		{<key>:<value>} 	db.col.find({"by":"菜鸟教程"}).pretty() 	where by = '菜鸟教程'
小于 		{<key>:{$lt:<value>}} 	db.col.find({"likes":{$lt:50}}).pretty() 	where likes < 50
小于或等于 	{<key>:{$lte:<value>}} 	db.col.find({"likes":{$lte:50}}).pretty() 	where likes <= 50
大于 		{<key>:{$gt:<value>}} 	db.col.find({"likes":{$gt:50}}).pretty() 	where likes > 50
大于或等于 	{<key>:{$gte:<value>}} 	db.col.find({"likes":{$gte:50}}).pretty() 	where likes >= 50
不等于 		{<key>:{$ne:<value>}} 	db.col.find({"likes":{$ne:50}}).pretty() 	where likes != 50

#find and
>db.col.find({key1:value1, key2:value2}).pretty()
#find or
db.col.find(
   {
      $or: [
	     {key1: value1}, {key2:value2}
      ]
   }
).pretty()

# $type 
db.col.find({"title" : {$type : 2}})

	Double 	1 	 
	String 	2 	 
	Object 	3 	 
	Array 	4 	 
	Binary data 	5 	 
	Undefined 	6 	已废弃。
	Object id 	7 	 
	Boolean 	8 	 
	Date 	9 	 
	Null 	10 	 
	Regular Expression 	11 	 
	JavaScript 	13 	 
	Symbol 	14 	 
	JavaScript (with scope) 	15 	 
	32-bit integer 	16 	 
	Timestamp 	17 	 
	64-bit integer 	18 	 
	Min key 	255 	Query with -1.
	Max key 	127 	 


#limit & skip
db.COLLECTION_NAME.find().limit(NUMBER)

db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)

#sort asc:1 desc: -1
db.COLLECTION_NAME.find().sort({KEY:1})

#index
db.COLLECTION_NAME.ensureIndex({KEY:1}{opts:value...}) 
#语法中 Key 值为你要创建的索引字段，1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可。
db.col.ensureIndex({"title":1,"description":-1})

	background		Boolean	建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为false。
	unique			Boolean	建立的索引是否唯一。指定为true创建唯一索引。默认值为false.
	name			string	索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。
	dropDups		Boolean	在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 false.
	sparse			Boolean	对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 false.
	expireAfterSeconds	integer	指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。
	v			index version	索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。
	weights	document	索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。
	default_language	string	对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语
	language_override	string	对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language


#aggregate
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)

下表展示了一些聚合的表达式:
	$sum		计算总和。	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])
	$avg		计算平均值	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])
	$min		获取集合中所有文档对应值得最小值。	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])
	$max		获取集合中所有文档对应值得最大值。	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])
	$push		在结果文档中插入值到一个数组中。	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])
	$addToSet	在结果文档中插入值到一个数组中，但不创建副本。	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])
	$first		根据资源文档的排序获取第一个文档数据。	db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])
	$last		根据资源文档的排序获取最后一个文档数据	db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])


管道的概念

    $project：	修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。 exclude:0  include:1
    $match：	用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
    $limit：	用来限制MongoDB聚合管道返回的文档数。
    $skip：	在聚合管道中跳过指定数量的文档，并返回余下的文档。
    $unwind：	将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
    $group：	将集合中的文档分组，可用于统计结果。
    $sort：	将输入文档排序后输出。
    $geoNear：	输出接近某一地理位置的有序文档。






