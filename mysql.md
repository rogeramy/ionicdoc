### 安装

MySQL8.0.16版本，下载windows安装包

依赖 NET4.5.2

只需要安装MySQL服务



### 规则，知识点

##### 关于bool类型

MySQL 使用 tinyint(1)，spring-boot JPA 对应为 Boolean



### 问题

##### 不能远程连接

1. 打开mysql控制台，输入：use mysql;
2. 输入：show tables;
3. 输入：select host from user;
4. 输入：update user set host ='%' where user ='root';
5. 重启 MySQL服务

##### 字符集

utf8	utf8_general-ci

utf8mb4	utf8mb4_general_ci	-- 最佳



##### You are using safe update mode

出错原因：Error Code: 1175
You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column。
经过翻译是当前模式为安全更新模式，不能使用非主键列更新数据表。

解决方法：将数据库安全模式修改字段修改为1。
语句：SET SQL_SAFE_UPDATES=0;
注意：SQL_SAFE_UPDATES与=，以及=与0之间不能用空格。否则会更新出错。



### 建模工具 MYSQL Workbench

安装与 MySQL 对应版本 windows 有安装包

