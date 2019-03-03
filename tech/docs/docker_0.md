# Docker在当前环境下的简单使用

## 场景

- python库存在基于**ftncl**函数的文件加锁调用，导致在**windows**中构建项目失败；
- 共享的开发环境不便于进行频繁的调试
- 保证开发与生产环境的完全一致，隔绝平台和网络的差异

## Docker与虚拟机的选择

- Docker启动速度优于虚拟机
- Docker镜像安装便于虚拟机的应用程序安装，不需要编译安装的同时，摆脱应用程序下各种依赖的处理,资源相对丰富，存在各种完备的应用程序（如nginx、mysql、redis、mongodb...）
- Docker中各个应用程序之间完全独立，拥有各种的轻量级的内核系统

## Docker存在的部分问题

- Docker在window10中会自动选择[**Hyper-V**](https://baike.baidu.com/item/Hyper-V/10508230?fr=aladdin)，但是在该模式下虚拟机的使用（如安卓模拟器）会直接造成电脑蓝屏，需要在关闭Hyper-V选项后，**重启**才能运行虚拟机（安卓模拟器）
- Docker镜像源地址在海外，需要翻墙（可以通过国内网易、阿里、网易镜像源解决）
- Docker在windows下的GUI也需要访问Docker海外网站，所以更多的是通过命令去执行Docker操作，对新手不友好
 
## Docker安装
- [windows安装方式](https://docs.docker.com/docker-for-windows/install/)
- [mac安装方式](https://docs.docker.com/docker-for-mac/)
- [linux安装方式](https://docs.docker.com/install/linux/docker-ce/centos/)

## Docker操作
### 一、Docker运行镜像
#### 常见命令
```
docker images        显示本机的镜像文件
docker search IMAGE  仓库搜索镜像文件
docker pull IMAGE    拉取镜像文件
docker stop IMAGE    停止运行中的容器
docker rm IMAGE      移除容器
docker ps -a         显示当前加载中的所有容器（-a 表示包括运行中和停止状态）
docker inspect       显示容器详细的配置信息

docker run
  -i 允许你对容器内的标准输入 (STDIN) 进行交互。 
  -t 在新容器内指定一个伪终端或终端。
  -p 端口指定映射
  -d daemon后台运行
  -v 本地挂载文件
  -P 内部端口随机映射到本机
  -e 设置环境变量
  --link 连接外部容器（如mysql、redis、nginx等）
```

### 二、制作项目镜像
这里将mysql redis mongodb nginx分开，不采用多个应用程序共用同一个容器，容器之间通过--link命令通信，各自完全独立

通过docker pull 命令安装mysql redis mongodb

这里以pyq_web为例，创建并运行该项目

> docker-compose.yml

```yml
version: "3"

services:
  web:
    build: ./pyq_web
    # links: 
    # - mysql
    # - redis
    #ports:
    # - "8000:8000"
    # volumes:
    # - ./pyq_web:/data
    command:
    - cd /data && python manage.py run server 8000
```

> dockerfile

```dockerfile
#基础镜像
FROM daocloud.io/python:2.7

#app所在目录
WORKDIR /data

#将目录加到工作目录中
ADD . /data

#暴露端口
EXPOSE 8000

#安装app所需依赖
RUN pip install --no-cache-dir -r requirements.txt -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
RUN mkdir -p /data/logs
```

> 打包

```bash
docker-compose build
```

> 运行

```bash
# 启动mysql
docker run --name mysql -p 3306:3306 -p 33060:33060 -d -e MYSQL_ALLOW_EMPTY_PASSWORD=true  mysql 

# 启动python程序
# docker_web是我制作的镜像名
# -p -v 可以在yml中固定，也可以在命令行中修改
# -v相当于共享本地的源码文件夹
# /bin/bash 是运行终端命令
docker run -it --link mysql:mysql -p 8000:8000 -v ./pyq_web:/data docker_web /bin/bash

# 进入容器后默认在/data目录
python manage.py makemigrations
python manage.py migrations
python manage.py runserver 8000
```
这样就可以快乐的在windows上修改代码，在docker容器中运行项目debug了

> 问题

1. 连接数据库失败 
    ```bpymysql.err.OperationalError: (2003, “Can’t connect to MySQL server on ‘127.0.0.1’”)```
    
- 方案：数据库ip地址错误,将settings.py中ip地址设置成本机ip
```python
# # makemigrations
# # https://docs.djangoproject.com/en/1.6/ref/settings/#databases
# DB_HOST = '192.168.100.22'
# DB_PASSWORD = 'bigworld'
# DB_PORT = '3306'
# DB_USER = 'vpoker'
# DB_HOST = '127.0.0.1'
DB_HOST = '192.168.100.162'
DB_PASSWORD = ''
# DB_PASSWORD = '123456'
DB_PORT = '3306'
DB_USER = 'root'
```

2. 连接数据库报错
```  
File "/usr/local/lib/python2.7/site-packages/pymysql/connections.py", line 1233, in _get_server_information
    self.server_charset = charset_by_id(lang).name
File "/usr/local/lib/python2.7/site-packages/pymysql/charset.py", line 34, in by_id
    return self._by_id[id]
KeyError: 255
```
- 方案：主要原因是MySQL8.0更新了很多字符集，但是这些字符集长度超过255了，所以旧版的PyMySQL不支持长度超过255的字符
    +  pip install --upgrade PyMySQL
    +  使用制定版本的mysql容器

3. 数据库不存在

+ 方案：create database **DATABASE**