+++

description = "在 Linux 系统中 GLIBC 不会立刻回收 free 的内存，这里有一些内部机制，通过 Google 最终在 gnu 官网和 man 手册网站找到了相关说明"
Tags = ["C", "CPP", "Linux", "GLIBC",  "free", "memory"]
date = "2016-02-23T11:25:30+08:00"
title = "Linux 下 GLIBC 内存回收的疑问"

+++

记得好早之前刚刚接触 Linux 下 C/C++ 开发的时候就有个疑问：在程序内 free 掉的空间没有立刻交换给操作系统，程序进程的内存没有减少。后来一度在开发各种长连接程序时 GOOGLE 各种资料，最近又偶尔遇到，其中两个重点内容如下：

1. [Freeing Memory Allocated with malloc](http://www.gnu.org/software/libc/manual/html_node/Freeing-after-Malloc.html)：

	![glibc freeing after malloc](/note/glibc-memory-freeing/freeing-with-malloc.gif)
	
	注意 “**Occasionally**” 用词；

2. [Linux Programmer's Manual - mallopt](http://man7.org/linux/man-pages/man3/mallopt.3.html)：

	![glibc freeing after malloc](/note/glibc-memory-freeing/mallopt.gif)
	
	描述了上述 “**Occasionally**” 回收的情况；

基本可以描述为，**内存会在 `free` 后被后续的 `malloc` 复用，当 `free` 的栈顶连续内存达到 `M_TRIM_THRESHOLD` 描述的值（默认 128kB）后回归系统**