---
layout: post
title: "php ActiveRecord"
category: php
comments: true
date:   2014-10-17 14:50:51
---

[Active  Record（AR）](http://www.phpactiverecord.org/)是一种流行的对象关系映射（ORM）技术。每个AR类代表一个数据表（或视图），其字段作为AR类的属性，一个AR实例代表在表中的  一行。常见的CRUD操作被作为AR类的方法执行。 于是，我们可以使用更面向对象的方法处理我们的数据。

ActiveRecord的主要思想是：

1. 每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录；通常表的每个字段在类中都有相应的Field；

2. ActiveRecord同时负责把自己持久化，在ActiveRecord中封装了对数据库的访问，即CURD;；

3. ActiveRecord是一种领域模型(Domain Model)，封装了部分业务逻辑；

ActiveRecord比较适用于：

1. 业务逻辑比较简单，当你的类基本上和数据库中的表一一对应时, ActiveRecord是非常方便的，即你的业务逻辑大多数是对单表操作；

2. 当发生跨表的操作时, 往往会配合使用事务脚本(Transaction Script)，把跨表事务提升到事务脚本中；

3. ActiveRecord最大优点是简单, 直观。 一个类就包括了数据访问和业务逻辑. 如果配合代码生成器使用就更方便了；

这些优点使ActiveRecord特别适合WEB快速开发。

ActiveRecord不适合于:

1. ActiveRecord虽然有业务逻辑, 但基本上都是基于单表的. 跨表逻辑一般会放到当发生跨表的操作时, 往往会配合使用事务脚本(Transaction Script)中. 如果对象间的关联越来越多, 你的事务脚本越来越庞大, 重复的代码越来越多, 你就要考虑Domain Model + O/R Mapper了;

2. ActiveRecord保存了数据, 使它有时候看上去像数据传输对象(DTO). 但是ActiveRecord有数据库访问能力, 不要把它当DTO用. 尤其在跨越进程边界调用的时候, 不能传递ActiveRecord对象;

著名的 Active Record 驱动框架:

- SQLObject (Python)
- Ruby on Rails ActiveRecord (Ruby)
- Yii Framework ActiveRecord (PHP)

ActiveRecord不是什么:

1. Row Data Gateway
Row Data Gateway模式中每个对象也封装了数据库记录的状态和持久化到数据库的访问方法; 这两个有时候很难区分. 细微的区别在于Row Data Gateway不封装任何业务逻辑;

2. TableGateway
是一种数据访问模式, 对每个表有一个类, 类的方法封装了对单个表的数据操作, 如CRUD; 方法的接受表字段的值作为参数;
比如说对表Person有DAOPerson, 有以下方法:
int Create(string name, bool isMale)
DataSet Find(int personId)
void Delete(int personId)
void Update(int personId, string name, bool isMale)
微软的很多代码示例中使用了此模式。
区别在于ActiveRecord的对象中保持了记录的值, 是有状态的, 而TableGateway是没有状态的, 只是一系列数据库访问方法的集合;

3. Table Module
Table Module是一种领域逻辑模式, 一个类对应于数据库中的一个表; Table Module通常和Table Gateway合作, 前者负责基本的业务逻辑, 后者负责数据库访问, 以达到逻辑层和持久化层的隔离; 微软的实例代码经常使用这两者, 如对表Person, 通常会定义两个类, PersonBL和PersonDB, 在PersonBL中处理验证等逻辑, 并调用PersonDB访问数据库, 层间调用使用DataSet或自定义数据传输对象传输数据

在业务逻辑比较简单并且有和表的一一对应时, ActiveRecord相对来说更简单, 因为它在一个类中包括了业务逻辑对象和数据访问, 而且不需要数据传输对象, 减少了维护的工作量;
和Table Module比较起来, ActiveRecord与数据库耦合更紧;

