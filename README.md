# lnmp-dockerfiles

搭建lnmp环境

## 简介
用docker容器服务的方式搭建lnmp环境，易于维护、升级。使用前需了解Docker的基本概念，常用基本命令。
可以一条条命令执行docker命令来构建镜像，容器。这里推荐使用docker-compose来管理，执行项目，下面是使用流程。

相关软件版本：
- PHP 7.2
- MySQL 5.7
- Nginx 1.12
- Redis 4.0
- Memcached 1.5

用到的PHP扩展
- redis 4.0.0
- swoole latest
- memcached 3.0.4

> 注意:标注完全版的，通过切换full分支获得文件才能安装



#### 1.下载需要的拓展包
先下载好要使用的拓展包，如果编译出错要多次构建容器就可以省掉下载时间。
```
wget https://pecl.php.net/get/redis-4.0.0.tgz -O php/pkg/redis.tgz  
wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz -O php/pkg/libmemcached.tar.gz  
wget https://pecl.php.net/get/memcached-3.0.4.tgz -O php/pkg/memcached.tgz  
```

#### 2.docker-compose构建项目
进行docker-compose.yml所在文件夹：
执行命令：
```
docker-compose up
```  

如果没问题，下次启动时可以以守护模式启用，所有容器将后台运行：  
```
docker-compose up -d
``` 
