title: supervisorctl
authors: milo

# supervisorctl {:# data-toc-label='supervisorctl'}

## 1. Introduction 简介 {:#introduction}
supervisor 的服务端叫作 supervisord ，它在自己的调用中启动子程序、响应客户端的命令、重启崩溃或结束的子进程、记录子进程的输出 `stdout` 和 `stderr`、生成和处理子进程生命周期中各个事件相应的 events 。<br/>

服务端进程需要一个配置文件，通常，配置文件的文件路径是 `/etc/supervisord.conf` 。该配置文件是一个 `Windows-INI` 风格的配置文件。由于==它可能包含了明文的用户名和密码==，因此需要给它设置一个恰当的权限以保证文件的安全性。<br/>


## 2. Running supervisorctl 运行 {:#running}
> supervisorctl [options] [action [arguments]]

### 2-1. supervisorctl Command-Line Options 命令行选项 {:#options}
options                             |   meanings
:-                                  |   :-
-c FILE, --configuration=FILE       |   指定配置文件的路径
-h, --help                          |   显示命令的用法
-i, --interactive                   |   执行完命令之后启动交互式shellstart an interactive shell after executing commands
-s/--serverurl URL                  |   URL on which supervisord server is listening (default "http://localhost:9001").
-u/--username USERNAME              |   username to use for authentication with server
-p/--password PASSWORD              |   password to use for authentication with server
-r/--history-file                   |   keep a readline history (if readline is available)
