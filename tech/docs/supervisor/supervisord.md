title: supervisord
authors: milo

# supervisord {:# data-toc-label='supervisord'}

## 1. Introduction 简介 {:#introduction}
supervisor 的服务端叫作 supervisord ，它在自己的调用中启动子程序、响应客户端的命令、重启崩溃或结束的子进程、记录子进程的输出 `stdout` 和 `stderr`、生成和处理子进程生命周期中各个事件相应的 events 。<br/>

服务端进程需要一个配置文件，通常，配置文件的文件路径是 `/etc/supervisord.conf` 。该配置文件是一个 `Windows-INI` 风格的配置文件。由于==它可能包含了明文的用户名和密码==，因此需要给它设置一个恰当的权限以保证文件的安全性。<br/>


## 2. Running supervisord 运行 {:#running}

### 2-1. supervisord Command-Line Options 命令行选项 {:#command-line-options}
options                             |   meanings
:-                                  |   :-
-c FILE, --configuration=FILE       |   指定配置文件的路径
-n, --nodaemon                      |   以非守护进程的方式运行
-h, --help                          |   显示 supervisord 命令的帮助
-u USER, --user=USER                |   以指定用户的身份运行
-m OCTAL, --umask=OCTAL             |   指定子进程的缺省权限
-v, --version                       |   显示 supervisord 的版本
-d PATH, --directory=PATH           |   在以守护进程方式启动 supervisord 之前，把工作目录切换到该目录
-l FILE, --logfile=FILE             |   指定日志文件的路径
-y BYTES, --logfile_maxbytes=BYTES  |   指定单个日志文件最大的文件大小，超出之后会自动分文件备份。"1"表示1字节，可以写成"1MB", "1GB"的形式
-z NUM, --logfile_backups=NUM       |   指定日志文件最大的数量，超出之后会覆盖最早的那个。
-e LEVEL, --loglevel=LEVEL          |   指定输出的日志等级。
-j FILE, --pidfile=FILE             |   指定 supervisord 的 pid 文件路径
-i STRING, --identifier=STRING      |   identifier used for this instance of supervisord
-q PATH, --childlogdir=PATH         |   指定子进程的日志文件目录
-k, --nocleanup                     |   启动时不清除旧的 `AUTO` 的日志文件
-a NUM, --minfds=NUM                |   the minimum number of file descriptors for start success
-t, --strip_ansi                    |   strip ansi escape codes from process output
--profile_options=LIST              |   run supervisord under profiler and output results based on OPTIONS, which  is a comma-sep'd list of 'cumulative', 'calls', and/or 'callers', e.g. 'cumulative,callers')
--minprocs=NUM	                    |   the minimum number of processes available for start success
