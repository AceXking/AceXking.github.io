---
layout: post
title: mysql.zip版本的安装教程
categories: mysql
description: mysql
keywords: mysql
---


## 一、官网下载

>在MySQL官网上 <http://dev.mysql.com/downloads/mysql/> 下载免安装版的zip文件

## 二、增添配置文件

>新建一个配置文件（my.ini）用于配置字符集、端口等信息，用以覆盖原始的配置文件（my-default.ini），当然也可以修改这个默认的配置文件。新建文件夹data存放MySQL数据

>将以下内容复制到新建的配置文件中，其中basedir和datadir设置mysql文件夹路径

````
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
[mysqld]
#设置3306端口
port = 3306 
# 设置mysql的安装目录

basedir=D:\\softwares\\mysql-5.7.14-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\\softwares\\mysql-5.7.14-winx64\\data

# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
````

## 三、初始化安装

### 1.在mysql目录下执行mysqld --initialize 初始化方法
### 2.执行mysqld --initialize命令后再执行mysqld install，再执行net start mysql启动MySQL

## 四、初始化密码

>执行`mysqladmin -u root password 密码`设置初始密码，设置ok后执行`mysql -u root -p`回车然后输入密码，即可登录`mysql`


## 五、进入mysql

>mysql中bin目录设置到环境变量path中，启动cmd输入mysql -u root -p密码即可登录mysql，而不用切换到mysql安装目录的bin目录下