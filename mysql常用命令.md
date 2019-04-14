# MySQL常用命令

## 数据备份

### 1.mysqldump命令备份

> mysqldump命令会先查出需要备份的表的结构，再在文本文件中生成一个CREATE语句。然后，将表中的所有记录转换成一条INSERT语句。然后通过这些语句，就能够创建表并插入数据。

#### 备份单个数据库

`mysqldump -u username -p dbname [table1 table2 ...] > backup_name.dump`  
dbname：数据库的名称  
table1、table2：可选参数，表示需要备份的具体表的名称，为空则整个数据库备份
backup_name.dump：备份文件，文件名前面可以加上路径  
mysqldump -u root -p test person > D:\backup.sql

#### 备份多个数据库

`mysqldump -u username -p --databases dbname2 dbname2 > Backup.sql`  如果数据库不存在会自动创建
`mysqldump -uroot -p --databases dbname2 --add-drop-database > xxxx.dump`这个会包含drop database命令
加上了--databases选项，然后后面跟多个数据库

mysqldump -u root -p --databases test mysql > D:\backup.sql

#### 备份所有数据库

`mysqldump -u username -p -all-databases > BackupName.sql`  
例如：`mysqldump -u -root -p -all-databases > D:\all.sql`

### 2.直接复制整个数据库目录来实现备份

MySQL有一种非常简单的备份方法，就是将MySQL中的数据库文件直接复制出来。这是最简单，速度最快的方法。  
不过在此之前，要先将服务器停止，这样才可以保证在复制期间数据库的数据不会发生变化。如果在复制数据库的过程中还有数据写入，就会造成数据不一致。这种情况在开发环境可以，但是在生产环境中很难允许备份服务器。  
**注意：这种方法不适用于InnoDB存储引擎的表，而对于MyISAM存储引擎的表很方便。同时，还原时MySQL的版本最好相同。**

### 3.使用mysqlhotcopy工具快速备份

## 数据还原

1. 还原使用mysqldump命令备份的数据库的语法如下：  
`mysql -u root -p [dbname] < backup.dump`  
示例：`mysql -u root -p < C:\backup.dump`
2. 还原直接复制目录的备份  
通过这种方式还原时，必须保证两个MySQL数据库的版本号是相同的。MyISAM类型的表有效，对于InnoDB类型的表不可用，InnoDB表的表空间不能直接复制。