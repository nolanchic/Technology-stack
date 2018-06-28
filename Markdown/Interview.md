# Interview

## PHP
```
php代码解释过程
跨域问题 

PHPUnit单元测试  是否连接数据库进行测试  对桩件（stub）和仿件对象（Mock）的理解
HTTP/1.0 中，状态码 101 200 301 304 403 404 500 502 504 的含义 502、504错误的处理

CGI是为了保证web server传递过来的数据是标准格式的，它是一个协议
Fastcgi是CGI的更高级的一种方式，是用来提高CGI程序性能的。
PHP-FPM又是什么呢？它是一个实现了Fastcgi协议的程序,用来管理Fastcgi起的进程的,即能够调度php-cgi进程的程序

swoole之进程模型  Master-Manager-Worker
对于多线程的Master进程而言，想要多Worker进程就必须fork操作，但是fork操作是不安全的，所以，在swoole中，有一个专职的Manager进程，Manager进程就专门负责worker/task进程的fork操作和管理
SWOOLE信号
SIGTERM，一种优雅的终止信号，会待进程执行完当前程序之后中断，而不是直接干掉进程
SIGUSR1，将平稳的重启所有的Worker进程
SIGUSR2，将平稳的重启所有的Task进程

swoole粘包问题
socket有缓冲区buffer的概念，每个TCP socket在内核中都有一个发送缓冲区和一个接收缓冲区。客户端send操作仅仅是把数据拷贝到buffer中，也就是说send完成了，数据并不代表已经发送到服务端了，之后才由TCP协议从buffer中发送到服务端。此时服务端的接收缓冲区被TCP缓存网络上来的数据，而后server才从buffer中读取数据。
```

## 操作系统
```
多进程和多线程还有协程的理解
```

## MYSQL
```
数据库事务的四大特性以及事务的隔离级别
1.原子性,原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚
2.一致性
假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性
3.持久性
持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作

第1级别：Read Uncommitted(读取未提交内容)  脏读
第2级别：Read Committed(读取提交内容)  不可重复读
第3级别：Repeatable Read(可重读)  MySQL的默认事务隔离级别 幻读
第4级别：Serializable(可串行化)

```


## NOSQL


## LINUX
```
软链接：软链接不占用磁盘空间，源文件删除则软链接失效。
硬链接：硬链接只能链接普通文件，不能链接目录。
ln 源文件 链接文件
ln -s 源文件 链接文件
修改文件权限：chmod
修改文件所有者：chown
修改文件所属组：chgrp
查看进程信息：ps
动态显示进程：top
检测磁盘空间：df
检测目录所占磁盘空间：du
终止进程：kill
1) SIGHUP    2) SIGINT   3) SIGQUIT  4) SIGILL   5) SIGTRAP
 6) SIGABRT  7) SIGBUS   8) SIGFPE   9) SIGKILL 10) SIGUSR1
11) SIGSEGV 12) SIGUSR2 13) SIGPIPE 14) SIGALRM 15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD 18) SIGCONT 19) SIGSTOP 20) SIGTSTP
21) SIGTTIN 22) SIGTTOU 23) SIGURG  24) SIGXCPU 25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF 28) SIGWINCH    29) SIGIO   30) SIGPWR
31) SIGSYS  34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
2号 SIGINT：中断信号 CTRL+C
9号 SIGKILL：杀死信号
11号 SIGSEGV：段错误信号
19号 SIGSTOP：暂停信号（如使用control ＋ z操作）
18号 SIGCONT：继续信号（对应19号暂停信号）


```

## WEB SERVER
```
nginx负载均衡和反向代理
php-fpm能代理其他端口吗？除了默认9000
nginx配置php-fpm的时候走的是什么协议？还可以走其他协议吗？
```


## 版本控制

## 架构
``` 
设计模式，实际用例
工厂模式：定义一个标准，用到的类可以按这个标准实现相应功能
单例模式：防止重复实例化，减少资源调用
数据映射：数据库ORM应用
适配器模式：兼容老数据，多态的应用

分布式与集群的

高并发
300W PV  每天80%的pv在20%的时间内访问
80% * 3百万  = 2400000，24小时的20%  =   4.8
也就是   4.8小时内  会有2400000的pv浏览
那么   2400000 / (4.8*60*60 ) =139
网络延迟按300ms  1000/300 =3.3  139/3.3=40并发
```
## 其他
```
自己最满意的代码
```



### 参考文档
```
http://www.jb51.net/article/116477.htm
https://www.cnblogs.com/snsdzjlz320/p/5761387.html
```