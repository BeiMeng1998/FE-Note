# MongoDB

# 安装MongoDB

1.安装
2.配置环境变量
3.C盘根目录创建data文件夹，data文件夹中创建一个文件夹db
4.打开cmd输入mongod启动mongodb服务器
5.再打开一个cmd输入mongo启动客户端连接mongodb服务器

自定义服务器地址和端口号
mongod --dbpath XXXX --port XXXX

# 将MongoDB设置为系统服务

1.在data文件下创建log文件夹
2.在mongoDB路径下添加配置文件mongod.cfg
systemLog:
	destination: file
	path: c:\data\log\mongod.log
storage:
	dbpath: c:\data\db
3.以管理员身份打开cmd
4.在bin目录下执行如下的命令
mongod.exe --logpath c:\data\log\mongodb.log --logappend --dbpath c:\data\db --directoryperdb --bind_ip 0.0.0.0 --serviceName MongoDB --install
5.启动服务 net start MongoDB

# 基本概念

数据库，集合，文档
在MongoDB中数据库和集合不需要手动创建

# 基本指令

show dbs  显示当前所有数据库
use 数据库名  进入指定的数据库中
db  当前所处的数据库
show collections  显示数据库中所有的集合

# 数据库的增删改查(CRUD)

## 1.向数据库中增加文档

插入一个
db.collection.insert(doc)

插入多个
db.collection.insert([doc1, doc2, doc3, doc4])

增加20000个文档
let arr = []
for(let i = 1; i <= 20000; i++) {
    arr.push({num:i})
}
db.nums.insert(arr)

## 2.查询当前集合中的文档

db.collection.find()

find()可以接收一个对象作为条件参数
返回所有符合条件的文档数组
.count()查询返回结果的数量

findOne()查询返回符合条件的第一个文档对象

MongoDB支持通过内嵌文档的属性进行查询
查询内嵌文档时，此时属性名必须使用引号
比如 {name:'Tom',age:18,hobbies:{movie:['abc', 'bbc']}}
db.collection.find({'hobbies.movie':'abc'})

$eq, $gt, $gte, $lt, $lte, $or
num大于5000的文档
db.nums.find({num:{$gt:5000}})

num大于500小于600的文档
db.nums.find({num:{$gt:500, $lt:600}})

limit()限制查询上限
查询集合中的前10条文档
db.collection.find().limit(10)

skip()跳过查询
查询集合中的第11~20条文档
db.collection.find().skip(10).limit(10)

查询集合中的num小于10和num大于20的文档
db.collection.find({$or:[{num:{$lt:10}},{num:{$gt:20}}]})

## 3.修改文档

db.collection.update()
update默认用新对象替换旧对象，默认只改一个

如果修改指定属性而非替换整个对象需要使用修改操作符

$set 可以用来修改文档中的指定属性
$unset 可以用来删除文档中的指定属性
db.collection.update(查询条件对象，{$set:需要修改的字段对象}，{multi: true修改多个})

修改一个
db.collection.updateOne()

修改多个
db.collection.updateMany()

MongoDB的属性值也可以是文档，我们叫内嵌文档

$push, $addToSet, $pop, $inc(自增)

sort()
sort({xxx:1})升序
sort({xxx:-1})降序

投影：只显示查询文档的部分属性
find({查询属性}, {想要显示的属性:1, 不想显示ID:0})

## 4.删除文档

db.collection.remove()
db.collection.remove(匹配对象)
会删除所有匹配到的文档
第二个参数传true只删一个

db.collection.deleteOne()
db.collection..deleteMany()

## 5.删除集合

db.collection.drop()

## 6.删除数据库

db.dropDatabase()

# 文档间的关系

## 一对一

在MongoDB中可以通过内嵌文档的形式体现一对一的关系


## 一对多或多对一

通过ID等属性映射

## 多对多

通过ID等数组映射

# Mongoose

Mongoose是一个让我们可以通过Node来操作MongoDB的模块
Mogoose是一个 对象文档模型库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能

Mongoose的好处

1.可以为文档创建一个模式解构
2.可以对模型中的对象/文档进行验证
3.数据可以通过类型转换转换为对象模型
4.可以使用中间件来应用业务逻辑挂钩
5.比Node原生的MongoDB驱动更容易

Mongoose提供了几个新的对象

## Schema(模式对象)

Schema对象定义约束了数据库中的文档结构

## Model

Model第项作为集合中的所有文档的表示，相当于MongoDB数据库中的集合collection

## Document

Document表示集合中的具体文档，相当于集合中的一个具体文档

## 连接数据库

```
// 引入mongoose
const mongoose = require('mongoose')

// 连接数据库(一般只需连接一次不再断开)
mongoose.connect('mongodb://localhost:27017/test')

// 监听是否连接成功
mongoose.connection.once('open', () => {
    console.log('数据库连接成功')
})
```

## 创建Schema模式对象

```
// 创建Schema(模式对象)
const Schema = mongoose.Schema
const studentSchema = new Schema({
    name: String,
    number: Number,
    age: Number,
    gender: {
        type: String,
        default: 'male'
    },
    address: String
})
```

## 创建Model

```
// 通过Schema创建Model，相当于数据库中的collection
// mongoose.model(modelName, schema)

const StudentsModel = mongoose.model('students', studentSchema)
```

## Model的方法

Model的方法

create()
create(doc(s), [callback])
doc(s)可以是一个文档也可以是一个文档数组
向数据库中插入一个文档
Model.create(doc(s), (err) => {})
例如：
StudentsModel.create({
    name: 'Luncy',
    number: 2,
    age: 16,
    gender: 'female',
    address: '纽约'
}, (err) => {
   if (!err) {
        console.log('插入文档成功');
    } else {
        console.log(`插入文档失败：${err}`)
    }
 })

find()
find(conditions, [projection], [options], callback)
findById(id, [projection], [options], callback)
findOne(conditions, [projection], [options], callback)
conditions 查询条件
projection 投影 例：{name: 1, _id: 0}或'name -_id'
options 查询选项(skip, limit) 例：{limit: 10}
callback 查询结果通过callback返回，find返回数组，findOne,findById返回具体的文档
注意：通过callback返回的对象就是Document文档对象，文档对象是Model的实例

例如：
StudentsModel.find({age: 18}, {name: 1, _id: 0}, {limit: 1}, (err, docs) => {
    console.log(docs)
})

update()
update(conditions, doc, [options], callback) 默认一个 options为{multi: true}修改多个
updateOne(conditions, doc, [options], callback)
updateMany(conditions, doc, [options], callback)

remove()
remove(conditions, [callback])
deleteOne(conditions, [callback])
deleteMany(conditions, [callback])

count()
count(conditions, [callback])
callback参数为err,count

## Document方法


Document方法

创建一个Document
let stu = new StudentsModel({
    name: 'Lucy',
    number: 5,
    age: 15,
    gender: 'female',
    address: '华盛顿'
})

save([options], [fn])
保存文档
stu.save()
update(update, [options], [callback])
remove([callback])
get(name)获取指定属性名的属性值
set(oldName, newName)设置指定属性值
