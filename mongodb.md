# mongodb学习
## MongoDB安装（ubuntu）

+ 安装命令：sudo apt-get install mongodb
+ 关闭/启动服务：sudo service mongodb stop/sudo service mongodb start

## 	MongoDB常用命令

+ 启动 mongo
+ 帮助 help
+ 数据库操作详细的帮助命令 db.help()
+ 对指定数据库的集合进行操作、管理和监控 db.users.help()
+ 查看现有数据库 show dbs
+ 创建数据库和切换数据库命令 use
>  命令"use"，同时也是数据库的新建命令，如果在使用了use "数据库名"这个命令后，如果进行相应的新建集合等实际操作后，这个数据库就会真实存在；否则如果用了这个命令，什么实际的操作都没有那这个命令也不会真正的新建一个数据库。

+ 数据库服务器状态命令 db.serverStatus()
+ 数据库统计信息命令 db.stats()
+ 显示数据集命令 db.getCollectionNames()
+ 获取当前数据库名称 db.getName()
+ 删除数据库命令 db.dropDatabase()
+ 添加用户 db.addUser("用户名","密码")
+ 删除用户 db.dropUser("用户名");
+ 关闭数据库命令
>- 终止数据库服务器进程 db.shutdownServer()
>- 在Linux下直接使用kill命令，杀掉mongod进程
>- 一般情况下，我们只是退出的话使用quit()，就可以了。
>- 如果你当前不是工作在admin数据库上，那无法终止数据库服务器进程。下面的命令来切换至admin上：use admin

## 监控与分析