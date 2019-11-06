title: Supervisor
author: Milo

# Supervisor

## Document 文档

[官方文档](http://supervisord.org/)<br/>
[配置文件详解](supervisor_conf.md#details)<br/>


## Install 安装

### Install with Pip
*点击[ 这里 ](../python/pip.md)查看如何安装pip*
> pip install supervisor


## Configure 配置

### Configuration File Default Path 配置文件的默认路径
Supervisor的配置文件通常命名为 `supervisord.conf` 。<br/>
**supervisord** 和 **supervisorctl** 都使用这个配置文件。<br/>
如果没有用 `-c` 选项指定配置文件，那就会按顺序在以下路径里寻找。<br/>
如果存在多个配置文件，会使用第一个匹配的文件路径。<br/>

1. `$CWD/supervisord.conf`
2. `$CWD/etc/supervisord.conf`
3. `supervisord.conf`
4. `etc/supervisord.conf`
5. `/etc/supervisord.conf`
6. `/etc/supervisor/supervisord.conf`

*配置文件可能不存在，通过运行[ echo_supervisord_conf ](supervisor_conf.md#configuration-template)打印出一个配置文件的样本。*<br/>


### Create a Configuration File 创建配置文件
> echo_supervisord_conf > /etc/supervisord.conf


## Usage 用法

### Add a Program 添加程序
添加一个程序到 Supervisor 中的常用配置：<br/>
```
;[program:theprogramname]
;command=/bin/cat              ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;autostart=true                ; start at supervisord start (default: true)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;environment=A="1",B="2"       ; process environment additions (def no adds)
```
点击[ 这里 ](supervisor_conf.md#program)查看完整配置<br/>

例如：<br/>
如果我需要让 Supervisor 管理我的 mkdocs 进程，那么我的配置如下：<br/>
```
[program:my_first_mkdocs]
; 执行命令/usr/local/bin/mkdocs serve -a 0.0.0.0:9001
command=/usr/local/bin/mkdocs serve -a 0.0.0.0:9001
; 在/root/my_first_mkdocs目录下执行命令
directory=/root/my_first_mkdocs
; 以root用户的身份执行命令
user=root
; 该进程随着Supervisor的启动而启动
autostart=true
; 标准输出stdout输出到/root/my_first_mkdocs/stdout.log文件
stdout_logfile=/root/my_first_mkdocs/stdout.log
; 把错误输出stderr重定向到标准输出stdout
redirect_stderr=true
```


### Run supervisord 启动服务端
启动命令：
> supervisord

等同于

> supervisord -c /etc/supervisord.conf

使用 -c 指定配置文件的路径，如果不指定路径，那么supervisord会在[ 默认路径 ](#configuration-file-default-path)中搜索配置文件。<br/>


### Run supervisorctl 启动客户端
启动命令：
> supervisorctl

启动进程：
> start &lt;name&gt;

停止进程：
> stop &lt;name&gt;

重启进程：
> restart &lt;name&gt;
