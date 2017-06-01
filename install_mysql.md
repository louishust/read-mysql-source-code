# MySQL 5.7.18源码安装


## 下载源码包

```
➜  src pwd
/home/vagrant/src

➜  src wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-boost-5.7.18.tar.gz

➜  src tar -xzvf mysql-boost-5.7.18.tar.gz

➜  src ln -s mysql-5.7.18 mysql

➜  src cd mysql

➜  mysql mkdir debug
```

## 安装依赖

```
➜  debug pwd
/home/vagrant/src/mysql/debug

➜  debug sudo apt-get install -y cmake bison libncurse g++ libncurses5-dev

➜  debug wget https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz

➜  debug tar -xzvf boost_1_59_0.tar.gz
```


## 编译DEBUG版本

```
➜  debug pwd
/home/vagrant/src/mysql/debug

➜  debug cmake -DWITH_DEBUG=1 -DCMAKE_INSTALL_PREFIX=./mysql -DWITH_BOOST=./boost_1_59_0 -DCURSES_NEED_NCURSES=1 ..

➜  debug make -j4 && make install
```

需要指定*CURSES_NEED_NCURSES*,不然CMAKE会找不到curse库。


## 初始化数据库

```
➜  mysql pwd
/home/vagrant/src/mysql/debug/mysql

➜  mysql bin/mysqld --initialize-insecure --datadir=./data --basedir=./ --user=mysql
```

## 手动生成配置文件

```
➜  mysql pwd
/home/vagrant/src/mysql/debug/mysql
➜  mysql cat my.cnf
[mysqld]
basedir = /home/vagrant/src/mysql/debug/mysql
datadir =/home/vagrant/src/mysql/debug/mysql/data
gdb
port=3306
```

## 调试MySQL

```
➜  mysql pwd
/home/vagrant/src/mysql/debug/mysql

➜  mysql gdb --args bin/mysqld --defaults-file=my.cnf
GNU gdb (Ubuntu 7.7.1-0ubuntu5~14.04.2) 7.7.1
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from bin/mysqld...done.
(gdb) b main
Breakpoint 1 at 0xe8a83c: file /home/vagrant/src/mysql/sql/main.cc, line 25.
(gdb) r
Starting program: /home/vagrant/src/mysql-5.7.18/debug/mysql/bin/mysqld --defaults-file=my.cnf
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, main (argc=2, argv=0x7fffffffe408) at /home/vagrant/src/mysql/sql/main.cc:25
25    return mysqld_main(argc, argv);
(gdb)
```


## 参考文档

[https://dev.mysql.com/doc/refman/5.7/en/source-installation.html](https://dev.mysql.com/doc/refman/5.7/en/source-installation.html)

[https://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization-mysqld.html](https://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization-mysqld.html)
