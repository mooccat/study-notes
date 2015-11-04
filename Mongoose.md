# Mongoose
## 简介
MongoDB是一个开源的NoSQL数据库，相比MySQL那样的关系型数据库，它更显得轻巧、灵活，非常适合在数据规模很大、事务性不强的场合下使用。同时它也是一个对象数据库，没有表、行等概念，也没有固定的模式和结构，所有的数据以文档的形式存储(文档，就是一个关联数组式的对象，它的内部由属性组成，一个属性对应的值可能是一个数、字符串、日期、数组，甚至是一个嵌套的文档。)，数据格式就是JSON。

### Mongoose是什么？
Mongoose是MongoDB的一个对象模型工具，是基于node-mongodb-native开发的MongoDB nodejs驱动，可以在异步的环境下执行。同时它也是针对MongoDB操作的一个对象模型库，封装了MongoDB对文档的的一些增删改查等常用方法，让NodeJS操作Mongodb数据库变得更加灵活简单。
### Mongoose能做什么？
Mongoose，因为封装了对MongoDB对文档操作的常用处理方法，让NodeJS操作Mongodb数据库变得简单
## 安装
1. 安装mongoose：`npm install mongoose`
2. 引用mongoose：`var mongoose = require("mongoose");`
3. 使用"mongoose"连接数据库：`var db = mongoose.connect("mongodb://user:pass@localhost:port/database");`
4. 执行下面代码检查默认数据库test，是否可以正常连接成功?
```
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
db.connection.on("error", function (error) {
    console.log("数据库连接失败：" + error);
});
db.connection.on("open", function () {
    console.log("------数据库连接成功！------");
});
```

## 集合
### 了解集合
MongoDB —— 是一个对象数据库，没有表、行等概念，也没有固定的模式和结构，所有的数据以Document(以下简称文档)的形式存储(Document，就是一个关联数组式的对象，它的内部由属性组成，一个属性对应的值可能是一个数、字符串、日期、数组，甚至是一个嵌套的文档。)
+ 文档 —— 是MongoDB的核心概念，是键值对的一个有序集，在JavaScript里文档被表示成对象。同时它也是MongoDB中数据的基本单元，非常类似于关系型数据库管理系统中的行，但更具表现力。
+ 集合 —— 由一组文档组成，如果将MongoDB中的一个文档比喻成关系型数据库中的一行，那么一个集合就相当于一张表。Mongoose

### Schema简述
Schema —— 一种以文件形式存储的数据库模型骨架，无法直接通往数据库端，也就是说它不具备对数据库的操作能力，仅仅只是数据库模型在程序片段中的一种表现，可以说是数据属性模型(传统意义的表结构)，又或着是“集合”的模型骨架。
> 定义Schema
```
var mongoose = require("mongoose");
var TestSchema = new mongoose.Schema({
    name : { type:String },//属性name,类型为String
    age  : { type:Number, default:0 },//属性age,类型为Number,默认为0
    time : { type:Date, default:Date.now },
    email: { type:String,default:''}
});
//基本属性类型有：字符串、日期型、数值型、布尔型(Boolean)、null、数组、内嵌文档等。
```

###Model简述
Model —— 由Schema构造生成的模型，除了Schema定义的数据库骨架以外，还具有数据库操作的行为，类似于管理数据库属性、行为的类。
> 通过Schema来创建Model
```
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
// 创建Model
var TestModel = db.model("test1", TestSchema);
//test1：数据库中的集合名称,当我们对其添加数据时如果test1已经存在，则会保存到其目录下，如果未存在，则会创建test1集合，然后在保存数据。
```

###Entity简述
> Entity —— 由Model创建的实体，使用save方法保存数据，Model和Entity都有能影响数据库的操作，但Model比Entity更具操作性。
> 使用Model创建Entity
```
var TestEntity = new TestModel({
       name : "Lenka",
       age  : 36,
       email: "lenka@qq.com"
});
console.log(TestEntity.name); // Lenka
console.log(TestEntity.age); // 36
```

## 增删改查
###查询
+ .find查询： obj.find(查询条件,callback);
```
Model.find({},function(error,docs){
   //若没有向find传递参数，默认的是显示所有文档
});
Model.find({ "age": 28 }, function (error, docs) {
  if(error){
    console.log("error :" + error);
  }else{
    console.log(docs); //docs: age为28的所有文档
  }
});
```

###保存
+ Model保存方法： Model.create(文档数据, callback))
```
Model.create({ name:"model_create", age:26}, function(error,doc){
    if(error) {
        console.log(error);
    } else {
        console.log(doc);
    }
});
```

+ entity保存方法:Entity.save(文档数据, callback)
```
var Entity = new Model({name:"entity_save",age: 27});
Entity.save(function(error,doc) {
    if(error) {
        console.log(error);
    } else {
        console.log(doc);
    }
});
```

### 数据更新
+ obj.update(查询条件,更新对象,callback);
```
var conditions = {name : 'test_update'};
var update = {$set : { age : 16 }};
TestModel.update(conditions, update, function(error){
    if(error) {
        console.log(error);
    } else {
        console.log('Update success!');
    }
});
```

###删除数据
+ obj.remove(查询条件,callback);
```
var conditions = { name: 'tom' };
TestModel.remove(conditions, function(error){
    if(error) {
        console.log(error);
    } else {
        console.log('Delete success!');
    }
});
```

## 简单查询
查询就是返回一个集合中的文档的子集，Mongoose 模型提供了find、findOne、和findById方法用于文档查询
+ find 过滤查询：属性过滤 find(Conditions,field,callback);field省略或为Null，则返回所有属性
```
//返回只包含一个键值name、age的所有记录
Model.find({},{name:1, age:1, _id:0}，function(err,docs){
   //docs 查询结果集
})
```

+ 单条数据 findOne(Conditions,callback):与find相同，但只返回单个文档，也就说当查询到即一个符合条件的数据时，将停止继续查询，并返回查询结果。
```
TestModel.findOne({ age: 27}, function (err, doc){
   // 查询符合age等于27的第一条数据
   // doc是查询结果
});
//findOne方法，只返回第一个符合条件的文档数据
```

+ 单条数据 findById(_id, callback)：与findOne相同，但它只接收文档的_id作为参数，返回单个文档。
```
TestModel.findById('obj._id', function (err, doc){
 //doc 查询结果文档
});
```

## 高级查询
### 条件查询
+ 使用$gt(>)、$lt(<)、$lte(<=)、$gte(>=)操作符进行排除性的查询
```
Model.find({"age":{"$gt":18}},function(error,docs){
   //查询所有nage大于18的数据
});
Model.find({"age":{"$lt":60}},function(error,docs){
   //查询所有nage小于60的数据
});
Model.find({"age":{"$gt":18,"$lt":60}},function(error,docs){
   //查询所有nage大于18小于60的数据
});
```

+ $ne(!=)操作符的含义相当于不等于、不包含，查询时我们可通过它进行条件判定
```
Model.find({ age:{ $ne:24}},function(error,docs){
    //查询age不等于24的所有数据
});
Model.find({name:{$ne:"tom"},age:{$gte:18}},function(error,docs){
  //查询name不等于tom、age>=18的所有数据
});
```

+ 和$ne操作符相反，$in相当于包含、等于，查询时查找包含于指定字段条件的数据
```
Model.find({ age:{ $in: 20}},function(error,docs){
   //查询age等于20的所有数据
});
Model.find({ age:{$in:[20,30]}},function(error,docs){
  //可以把多个值组织成一个数组
});
```

+ $or操作符，可以查询多个键值的任意给定值，只要满足其中一个就可返回，用于存在多个条件判定的情况下使用
```
Model.find({"$or":[{"name":"yaya"},{"age":28}]},function(error,docs){
  //查询name为yaya或age为28的全部文档
});
```

+ $exists操作符，可用于判断某些关键字段是否存在来进行条件查询
```
Model.find({name: {$exists: true}},function(error,docs){
  //查询所有存在name属性的文档
});
Model.find({telephone: {$exists: false}},function(error,docs){
  //查询所有不存在telephone属性的文档
});
```

## 游标
数据库使用游标返回find的执行结果。客户端对游标的实现通常能够对最终结果进行有效的控制。可以限制结果的数量，略过部分结果，根据任意键按任意顺序的组合对结果进行各种排序，或者是执行其他一些强的操作。
> 最常用的查询选项就是限制返回结果的数量(limit函数)、忽略一点数量的结果(skip函数)以及排序(sort函数)。所有这些选项一定要在查询被发送到服务器之前指定。

+ limit函数的基本用法：限制数量：find(Conditions,fields,options,callback);
```
Model.find({},null,{limit:20},function(err,docs){
    console.log(docs);
});
//如果匹配的结果不到20个，则返回匹配数量的结果，也就是说limit函数指定的是上限而非下限
```

+ skip函数的基本用法：跳过数量：find(Conditions,fields,options,callback);
```
Model.find({},null,{skip:4},function(err,docs){
    console.log(docs);
});
```

+ sort函数的基本用法:（sort函数可以将查询结果数据进行排序操作，该函数的参数是一个或多个键/值对，键代表要排序的键名，值代表排序的方向，1是升序，-1是降序。）结果排序：find(Conditions,fields,options,callback);
```
Model.find({},null,{sort:{age:-1}},function(err,docs){
  //查询所有数据，并按照age降序顺序返回数据docs
});
```

## 属性方法
### ObjectId简述
存储在mongodb集合中的每个文档（document）都有一个默认的主键_id，这个主键名称是固定的，它可以是mongodb支持的任何数据类型，默认是ObjectId。该类型的值由系统自己生成，从某种意义上几乎不会重复，生成过程比较复杂
使用过MySQL等关系型数据库的友友们都知道，主键都是设置成自增的。但在分布式环境下，这种方法就不可行了，会产生冲突。为此，MongoDB采用了一个称之为ObjectId的类型来做主键
**ObjectId是一个12字节的 BSON 类型字符串。按照字节顺序，一次代表：**
+ 4字节：UNIX时间戳
+ 3字节：表示运行MongoDB的机器
+ 2字节：表示生成此_id的进程
+ 3字节：由一个随机数开始的计数器生成的值

### Schema添加属性值
我们已经知道了如何定义一个Schema并赋予某些属性值,那能不能先定义后添加属性呢，答案是可以的
```
var mongoose = require('mongoose');
var TempSchema = new mongoose.Schema;
TempSchema.add({ name: 'String', email: 'String', age: 'Number' });
```
### 实例方法
创造的Schema不仅要为后面的Model和Entity提供公共的属性，还要提供公共的方法.
```
var mongoose = require('mongoose');
var saySchema = new mongoose.Schema({name : String});
saySchema.method('say', function () {
  console.log('Trouble Is A Friend');
})
var say = mongoose.model('say', saySchema);
var lenka = new say();
lenka.say(); //Trouble Is A Friend
```

### Schema静态方法
为Schema创建静态方法。如下示例：
```
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestSchema = new mongoose.Schema({
    name : { type:String },
    age  : { type:Number, default:0 },
    email: { type:String, default:"" },
    time : { type:Date, default:Date.now }
});
TestSchema.static('findByName', function (name, callback) {
    return this.find({ name: name }, callback);
});
var TestModel = db.model("test1", TestSchema );
TestModel.findByName('tom', function (err, docs) {
 //docs所有名字叫tom的文档结果集
});
```

### Schema追加方法
为Schema模型追加方法，为Schema模型追加speak方法，如下示例：
```
var mongoose = require("mongoose");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
var TestSchema = new mongoose.Schema({
    name : { type:String },
    age  : { type:Number, default:0 },
    email: { type:String, default:"" },
    time : { type:Date, default:Date.now }
});
TestSchema.methods.speak = function(){
  console.log('我的名字叫'+this.name);
}
var TestModel = db.model('test1',TestSchema);
var TestEntity = new TestModel({name:'Lenka'});
TestEntity.speak();//我的名字叫Lenka
```


