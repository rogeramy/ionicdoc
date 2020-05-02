### 安装

MySQL8.0.16版本，下载windows安装包

依赖 NET4.5.2

只需要安装MySQL服务



### 问题

##### 不能远程连接

1. 打开mysql控制台，输入：use mysql;
2. 输入：show tables;
3. 输入：select host from user;
4. 输入：update user set host ='%' where user ='root';
5. 重启 MySQL服务

##### 字符集

utf8	utf8_general-ci

