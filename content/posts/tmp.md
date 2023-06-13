---
title: "记录一次hacking gpu的过程"
date: 2023-06-13T04:36:41-04:00
draft: false
tag: ["linux"]
---
# 前言
背景：  
员工使用的本地主机没有GPU，正常的工作流程是使用本地主机连接Login Node主机编写代码。GPU cluster位于Work Node主机，
员工通过登录Login Node主机，在公司提供的登录页面上申请Work Node主机上的GPU使用。  
这样带来的不便在于：
1. 有时申请不到GPU使用
2. 网页登录只能使用jupyter notebook/lab，没有vscode的插件顺手，要切换回vscode更改代码然后用网页运行，来回切换十分麻烦  
   
此外，虽然公司提供一个远程桌面的vscode，但是因为延时卡顿严重，编码体验极差。因此诞生了在本地直接使用Work Node上GPU的需求。  
操作系统: 本地为Window，Login Node和Work Node均为Linux
# Part One
问题：  
本地主机（Local computer）不可以通过ssh直连Work Node主机  
只能通过公司提供的，运行在Login Node主机上的web页面建立链接

解决方案：  
经过测试Login Node可以使用ssh连接Work Node节点，所以使用  
```ssh username@work_node -J username@login_node:<port number>```
# Part Two
至此，成功实现了本地连接GPU，可喜可贺。但是问题还没有完全解决，虽然可以稍微训练一些epoch，但是在经过一段时间后，notebook kernel会自动crash。

通过 ```strace -e trace=exit,exit_group -p PID```
发现问题症结在于，训练进程运行一段时间后会收到root用户发出SIGTERM信号，因此终止进程  
Output of strace:  
```--- SIGTERM {si_signo=SIGTERM, si_code=SI_USER, si_pid=PID, si_uid=0} ---``` 
GPT的解释：  
>This message is a signal sent to a process to request its termination. It contains information about the signal, including the signal number (si_signo), the process ID of the process that sent the signal (si_pid), and the user ID of the process that sent the signal (si_uid).  
In Unix-like operating systems, a SIGTERM signal is usually sent by the operating system to request a graceful termination of a process. This means that the process should perform any necessary cleanup and then terminate itself.

解决方案：  
In Python  
```
import signal
def ignore_signal(signum, frame):
    print("Received signal", signum)
signal.signal(signal.SIGTERM, ignore_signal)
```
让GPT解释一下这一段代码：
> The code sets the signal.SIGTERM signal to use the "ignore_signal" function as its handler. This means that when the program receives a SIGTERM signal (typically sent by the operating system to instruct a program to terminate), it will invoke the "ignore_signal" function instead of the default termination behavior.
# 总结
至此，成功在本地训练完了一整个模型，而且带来了以下好处：
1. 绕过了公司的登录界面，不受申请GPU的active time限制
2. 可以访问一个Work Node主机上的全部GPU

因为可以访问一个主机上的所有GPU，顺带发现了GPU明明还有空余但是申请失败的情况。

在解决问题的过程中也走了许多弯路，回头看来解决问题首先要定位问题所在，而不是依靠猜测胡乱尝试，期待瞎猫碰见死耗子。
解决问题是简单的，发现问题是困难的。这大约也是工学（Engineering）的特征所在。
# ps: 解决方案的提升空间
应该在训练结束后恢复正常的SIGTERM响应，不然只能用SIGKILL杀死进程，不够优雅。