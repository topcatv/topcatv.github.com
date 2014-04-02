title: how to config runlevel of ubuntu12.04
date: 2014-04-02 13:11:58
categories: [ubuntu]
tags: [ubuntu]
---

ubuntu 12.04默认的开机会进入一个图形界面，用命令pstree可以看到图形界面所在的进程树

首先要做的，就是阻止这个lightdm的进程开机启动。 做法：
- 一、查看文件/etc/init/rc-sysinit.conf，在第14行附近：确认"env DEFAULT_RUNLEVEL=2"。2是新装系统默认的，确保不被修改。

- 二、编辑文件 /etc/init/lightdm.conf，在第12行附近，原句"and runlevel [!06]” 改为“ and runlevel [!026]"。

解释：linux系统都有一个运行级别(runlevel)的概念，不同的运行级别配置将导致系统的启动过程有很大差异，比如当配置 runlevel 为 1 是，是不进入图形界面的。系统启动过程中会有一个init进程来拉起许多其他进程(各种系统服务，窗口界面)。在ubuntu上(11.10,12.04是这样，其他版本或其他linux发行版不确定)init会执行两个目录下的脚本，一个是/etc/init/下的，另一个是/etc/rc?.d/下的，问号可能是0~6的其中一个数字，代表运行级别。接下来，讲解一下流程以加深理解。

在ubuntu上，init进程首先执行/etc/init/目录下的rc-sysinit.conf，这个文件指明了本次启动的默认运行级别。这是上面第一步的意义：确保默认运行级别是2。接下来目录/etc/init下的其他脚本的执行都会根据不同的运行级别做出不同的动作，比如lightdm会判断运行级别是否处于1,2,3,4,5中的一个，是则启动lightdm，不是则不启动lightdm。这便是上面第二步的意义，修改 lightdm.conf ，把“2”加入到判断语句，使得lightdm在运行级别2的时候不要启动。明白了这些，你就可以灵活一点，例如把默认级别设置为3，而把3加入那个判断语句，也可以达到阻止lightdm启动的效果。完成了/etc/init/目录下的启动动作，init 进程会继续执行/etc/rc2.d目录下的脚本。
