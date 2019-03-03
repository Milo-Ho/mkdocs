终端可以说是在Linux的使用中最常用到的工具了，通过设置PS1,PS2,PS3,PS4这四个环境变量，我们就可以个性化定制终端的提示符。

## 一、PS1：默认的交互提示符

### 1. 了解PS1

PS1是Linux终端用户的一个环境变量，用来定义命令行提示符的参数。

在终端输入命令 **echo $PS1** ，可得到当前PS1的定义值：
```
$ echo $PS1
\e[36;1m\u\e[0m@\e[33;1m\h\e[0m \e[35;1m\t\e[0m:\e[31;1m\w\e[0m\n$
```

#### PS1的常用参数以及含义:

参数 | 含义
:---:|:---|
\d | 代表日期，格式为weekday month date，例如："Mon Aug 1"
\H | 完整的主机名称
\h | 仅取主机名中的第一个名字
\t | 显示时间为24小时格式，如：HH：MM：SS
\T | 显示时间为12小时格式
\A | 显示时间为24小时格式：HH：MM
\u | 当前用户的账号名称
\v | BASH的版本信息
\w | 完整的工作目录名称
\W | 利用basename取得工作目录名称，只显示最后一个目录名
\\# | 下达的第几个命令
\\$ | 提示字符，如果是root用户，提示符为 # ，普通用户则为 $

**PS1='\u@\h \t:\w\n$ '** 的意思就是：  
```bash
# [当前用户的账号名称]@[主机名的第一个名字] [24小时格式的时间]:[完整的工作目录名称][换行][$]
milo@AppledeMacBook-Pro 14:36:54:~
$
```


### 2. 颜色设置参数

在PS1中设置字符颜色的格式为：**\e[F;Bm** ；

其中"F"为字体颜色，编号为30-37，"B"为背景颜色，编号为40-47；

当"B"编号为0-8时有特殊含义；

\e也可以用\033代替；

#### 颜色对照表：

 F  | B  | 颜色
:--:|:--:|:--:
 30 | 40 | 黑色
 31 | 41 | 红色
 32 | 42 | 绿色
 33 | 43 | 黄色
 34 | 44 | 蓝色
 35 | 45 | 紫红色
 36 | 46 | 青蓝色
 37 | 47 | 白色
 /  | 0  | OFF
 /  | 1  | 高亮显示
 /  | 4  | underline
 /  | 5  | 闪烁
 /  | 7  | 反白显示
 /  | 8  | 不可见

<br>

以下是我个人的格式

    PS1="\e[36;1m\u\e[0m@\e[33;1m\h\e[0m \e[35;1m\t\e[0m:\e[31;1m\w\e[0m\n$ "


### 3. 修改.bashrc文件，永久保存命令行样式

编辑.bashrc：

    vim ~/.bashrc

在文件末尾加入：

    export PS1="\e[36;1m\u\e[0m@\e[33;1m\h\e[0m \e[35;1m\t\e[0m:\e[31;1m\w\e[0m\n$ "
保存退出  

重新加载bash配置文件:

    source ~/.bashrc
就可以永久生效


## 二、PS2：多行交互提示符

前面我们讲了很多关于PS1变量的属性，我们在大部分的时间都是和默认的命令行提示符打交道的。现在讲讲使用较少的PS2变量。  
当我们的输入一个超过一行的命令时，我们可以使用“\”符号将命令分割成几行来执行，当使用了‘\’之后在第二行会出现一个新的提示符，如下
```bash
sunshine@0101 ~ 11:09 $ sudo apt-get install build-essential \
> libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg62-dev \
> libtiff4-dev cmake libswscale-dev libjasper-dev;
```
第二行开头的’>’就是多行提示符（Continuation Interactive Prompt），这个提示符就是由PS2变量设置的。
```bash
sunshine@0101 ~ 11:09 $ export PS2="continue-> "
sunshine@0101 ~ 11:09 $ sudo apt-get install build-essential \
continue-> libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg62-dev \
continue-> libtiff4-dev cmake libswscale-dev libjasper-dev;
```
==PS2变量也支持PS1中的属性（\h,\t等）以及各种颜色设置。==


## 三、PS3：select命令使用的提示符
```bash
# select命令是bash shell提供的一种循环,语法如下：
select varName in list
do
    command1
    command2
    ....
    ......
    commandN
done

# 同时也可以结合case命令：

select varName in list
do
    case $varName in
        pattern1)
            command1;;
        pattern2)
            command2;;
        pattern1)
            command3;;
        *)
            echo "Error select option 1..3";;
    esac            
done
```
有关select详细的语法解释请见 [select loop](http://bash.cyberciti.biz/guide/Select_loop)  
先来看一个简单的示例：
```bash
#!/bin/bash
select i in mon tue wed exit
do
    case $i in
        mon) echo "Monday";;
        tue) echo "Tuesday";;
        wed) echo "Wednesday";;
        exit) exit;;
    esac
done

保存到ps3.sh，运行一下:
sunshine@0101 ~ 14:44 $ ./ps3.sh
1) mon
2) tue
3) wed
4) exit
#? 1
Monday
```
可以看到提示我们选择的提示符号是#?，而这个提示符正是通过PS3(默认为#?)这个变量来设置的:
```bash
#!/bin/bash
PS3="Select a day (1-4): "
select i in mon tue wed exit
do
    case $i in
        mon) echo "Monday";;
        tue) echo "Tuesday";;
        wed) echo "Wednesday";;
        exit) exit;;
    esac
done

保存到ps3.sh中，运行
sunshine@0101 ~ 14:44:34 $ ./ps3.sh
1) mon
2) tue
3) wed
4) exit
select a day(1-4): 1
Monday
```
==PS3不支持PS1变量中的各种属性以及颜色的设置。==


## 四、PS4：调试输出追踪

PS4变量定义的是在调试状态下执行shell脚本时的提示符，如下
```bash
sunshine@0101 ~ 14:44:34 $ cat >ps4.sh
set -x
echo "Demo script"
ls -l | wc -l
df -h ./
sunshine@0101 ~ 14:44:34 $ ./ps4.sh
++ echo 'Demo script'
Demo script
++ ls -l /etc/
++ wc -l
244
++ df -h ./
Filesystem            Size  Used Avail Use% Mounted on
10.0.0.3:/export/home/sunshine
                      499G  300G  199G  60% /home/sunhine
[当使用set -x追踪输出时，这里显示的是默认的"++"]
```
通过设置PS4变量就可以改变默认的”++”,输出一些对脚本调试有用的信息。下面的shell脚本中的变量：  
$0:脚本的名字  
$LINENO:显示当前命令在脚本中的行数
```bash
unshine@0101 ~ 14:44:34 $ cat >ps4.sh
export PS4="$0 $LINENO+: "
set -x
echo "Demo script"
ls -l | wc -l
df -h ./
sunshine@0101 ~ 14:44:34 $ ./ps4.sh
../02.ps4.sh 3+: echo 'Demo script'
Demo script
../02.ps4.sh 4+: ls -l /etc/
../02.ps4.sh 4+: wc -l
244
../02.ps4.sh 5+: df -h ./
Filesystem            Size  Used Avail Use% Mounted on
10.0.0.3:/export/home/sunshine
                      811G  490G  279G  64% /home/sunshine
[可以看到之前的"++"已经变成了"../02.ps4.sh 5+:",即{脚本名}.{行数}]
```

---
作者：echo42  
来源：CSDN  
原文：https://blog.csdn.net/echo42/article/details/29654939  
版权声明：本文为博主原创文章，转载请附上博文链接！ 


