## 绿色版 mysql-5.7.19-winx64.zip 的安装 - Windows 平台

1. 下载安装 [Microsoft Visual C++ 2013 Redistributable Package](https://www.microsoft.com/zh-CN/download/details.aspx?id=40784)
2. 下载绿色版 [mysql-5.7.19-winx64.zip](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip)
3. 解压 `mysql-5.7.19-winx64.zip`，以下假设为 `D:/mysql-5.7.19-winx64`，作为 mysql 的安装目录
4. 在 `%WINDIR%` 目录下创建 `my.ini` 文件，内容如下：
    ```
    [mysqld]
    # set basedir to your installation path
    basedir=D:/mysql-5.7.19-winx64
    # set datadir to the location of your data directory
    datadir=D:/mysql-5.7.19-winx64/data

    # see http://www.cnblogs.com/abclife/p/5051861.html
    explicit_defaults_for_timestamp=true
    # set default time zone
    default-time-zone='+8:00'
    ```
    注：主要是设置 `basedir` 和 `datadir` 目录，指向实际的安装位置，路径分隔符使用 `/` 或 `\\`。这两个值按照自己的实际情况配置好即可。
5. 使用如下任一命令初始化数据库：（须在管理员模式下运行）
    ```
    > mysqld --initialize --console           <- 默认生成随机并标记为已过期的 root 密码
    > mysqld --initialize-insecure --console  <- 不会生成 root 密码，需要另行设置
    ```
    注：此命令执行完毕后，会自动创建 `datadir` 目录来初始化数据库，如果该目录非空就会收到 `[ERROR] --initialize specified but the data directory has files in it. Aborting.` 错误，导致初始化失败。这时就需要手动删除该目录后重新执行初始化。如果使用 `--initialize` 参数初始化，控制台回输出自动生成的随机密码。如：
    ```
    [Note] A temporary password is generated for root@localhost: #dydrg21ji#C
    ```
    如果没有使用 `--console` 参数，则控制台不会输出信息，请打开 `data/xxx.err` 文件查看。
6. 启动 mysql server：
    ```
    > mysqld --console
    ````
    启动成功后，会看到类似如下的信息：
    ```
    [Note] Server hostname (bind-address): '*'; port: 3306
    ```
7. 登陆 mysql 修改 root 账号密码：
    ```
    C:\Users\X>mysql -u root -p
    Enter password: ********
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 7
    Server version: 5.7.19 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> alter user 'root'@'localhost' identified by 'password';
    Query OK, 0 rows affected (0.01 sec)
    ````
    注：如果是使用 `--initialize-insecure` 初始化数据库的，则使用 '`>mysql -u root`' 登陆。
8. 查看现有的账号看看密码是否为空：
    ````
    mysql> select user, host, hex(authentication_string) from mysql.user;
    ````
9. 停止 mysql server：
    ```
    > mysqladmin -u root shutdown -p
    ````

## 常用命令

- \> mysqlshow -u root -p 《 数据库列表
- \> mysqlshow mysql 《 列出数据库 mysql 的所有 table
- \> mysql -e "select * from t" test 《 对 test 数据库直接执行 sql 并输出结果


## 参考

- [Mysql 脚本收集](http://rongjih.blog.163.com/blog/static/3357446120114623715117)
- [Mysql 官方安装教程（绿色版）](https://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html)
- [Mysql 时区设置](https://stackoverflow.com/questions/930900/how-do-i-set-the-time-zone-of-mysql)