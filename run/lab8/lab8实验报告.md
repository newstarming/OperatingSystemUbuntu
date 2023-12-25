# Lab 8文件系统

文件最早来自于计算机用户需要把数据持久保存在 持久存储设备 上的需求。由于放在内存中的数据在计算机关机或掉电后就会消失，所以应用程序要把内存中需要保存的数据放到 持久存储设备的数据块（比如磁盘的扇区等）中存起来。随着操作系统功能的增强，在操作系统的管理下，应用程序不用理解持久存储设备的硬件细节，而只需对 文件 这种持久存储数据的抽象进行读写就可以了，由操作系统中的文件系统和存储设备驱动程序一起来完成繁琐的持久存储设备的管理与读写。

## 实验目的

通过完成本次实验，希望能够达到以下目标

* 了解文件系统抽象层-VFS的设计与实现
* 了解基于索引节点组织方式的Simple FS文件系统与操作的设计与实现
* 了解“一切皆为文件”思想的设备文件设计
* 了解简单系统终端的实现

## 实验内容

实验七完成了在内核中的同步互斥实验。本次实验涉及的是文件系统，通过分析了解ucore文件系统的总体架构设计，完善读写文件操作(即实现sfs_io_nolock()函数)，从新实现基于文件系统的执行程序机制（即实现load_icode()函数），从而可以完成执行存储在磁盘上的文件和实现文件读写等功能。

与实验七相比，实验八增加了文件系统，并因此实现了通过文件系统来加载可执行文件到内存中运行的功能，导致对进程管理相关的实现比较大的调整。


### 练习

对实验报告的要求：

* 基于markdown格式来完成，以文本方式为主
* 填写各个基本练习中要求完成的报告内容
* 列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点，并简要说明你对二者的含义，关系，差异等方面的理解（也可能出现实验中的知识点没有对应的原理知识点）
* 列出你认为OS原理中很重要，但在实验中没有对应上的知识点

#### 练习0：填写已有实验

本实验依赖实验2/3/4/5/6/7。请把你做的实验2/3/4/5/6/7的代码填入本实验中代码中有“LAB2”/“LAB3”/“LAB4”/“LAB5”/“LAB6” /“LAB7”的注释相应部分。并确保编译通过。注意：为了能够正确执行lab8的测试应用程序，可能需对已完成的实验2/3/4/5/6/7的代码进行进一步改进。

#### 练习1: 完成读文件操作的实现（需要编码）

首先了解打开文件的处理流程，然后参考本实验后续的文件读写操作的过程分析，填写在 kern/fs/sfs/sfs_inode.c中 的sfs_io_nolock()函数，实现读文件中数据的代码。

#### 练习2: 完成基于文件系统的执行程序机制的实现（需要编码）

改写proc.c中的load_icode函数和其他相关函数，实现基于文件系统的执行程序机制。执行：make qemu。如果能看看到sh用户程序的执行界面，则基本成功了。如果在sh用户界面上可以执行”ls”,”hello”等其他放置在sfs文件系统中的其他执行程序，则可以认为本实验基本成功。

#### 扩展练习 Challenge1：完成基于“UNIX的PIPE机制”的设计方案

如果要在ucore里加入UNIX的管道（Pipe)机制，至少需要定义哪些数据结构和接口？（接口给出语义即可，不必具体实现。数据结构的设计应当给出一个(或多个）具体的C语言struct定义。在网络上查找相关的Linux资料和实现，请在实验报告中给出设计实现”UNIX的PIPE机制“的概要设方案，你的设计应当体现出对可能出现的同步互斥问题的处理。）

#### 扩展练习 Challenge2：完成基于“UNIX的软连接和硬连接机制”的设计方案

如果要在ucore里加入UNIX的软连接和硬连接机制，至少需要定义哪些数据结构和接口？（接口给出语义即可，不必具体实现。数据结构的设计应当给出一个(或多个）具体的C语言struct定义。在网络上查找相关的Linux资料和实现，请在实验报告中给出设计实现”UNIX的软连接和硬连接机制“的概要设方案，你的设计应当体现出对可能出现的同步互斥问题的处理。）


### 项目组成

#### Lab8 项目组成

```
lab8
├── Makefile
├── disk0
│   ├── badarg
│   ├── badsegment
│   ├── divzero
│   ├── exit
│   ├── faultread
│   ├── faultreadkernel
│   ├── forktest
│   ├── forktree
│   ├── hello
│   ├── matrix
│   ├── pgdir
│   ├── priority
│   ├── sh
│   ├── sleep
│   ├── sleepkill
│   ├── softint
│   ├── spin
│   ├── testbss
│   ├── waitkill
│   └── yield
├── giveitatry.pyq
├── kern
│   ├── debug
│   │   ├── assert.h
│   │   ├── kdebug.c
│   │   ├── kdebug.h
│   │   ├── kmonitor.c
│   │   ├── kmonitor.h
│   │   ├── panic.c
│   │   └── stab.h
│   ├── driver
│   │   ├── clock.c
│   │   ├── clock.h
│   │   ├── console.c
│   │   ├── console.h
│   │   ├── ide.c
│   │   ├── ide.h
│   │   ├── intr.c
│   │   ├── intr.h
│   │   ├── kbdreg.h
│   │   ├── picirq.c
│   │   ├── picirq.h
│   │   ├── ramdisk.c
│   │   └── ramdisk.h
│   ├── fs
│   │   ├── devs
│   │   │   ├── dev.c
│   │   │   ├── dev.h
│   │   │   ├── dev_disk0.c
│   │   │   ├── dev_stdin.c
│   │   │   └── dev_stdout.c
│   │   ├── file.c
│   │   ├── file.h
│   │   ├── fs.c
│   │   ├── fs.h
│   │   ├── iobuf.c
│   │   ├── iobuf.h
│   │   ├── sfs
│   │   │   ├── bitmap.c
│   │   │   ├── bitmap.h
│   │   │   ├── sfs.c
│   │   │   ├── sfs.h
│   │   │   ├── sfs_fs.c
│   │   │   ├── sfs_inode.c
│   │   │   ├── sfs_io.c
│   │   │   └── sfs_lock.c
│   │   ├── swap
│   │   │   ├── swapfs.c
│   │   │   └── swapfs.h
│   │   ├── sysfile.c
│   │   ├── sysfile.h
│   │   └── vfs
│   │       ├── inode.c
│   │       ├── inode.h
│   │       ├── vfs.c
│   │       ├── vfs.h
│   │       ├── vfsdev.c
│   │       ├── vfsfile.c
│   │       ├── vfslookup.c
│   │       └── vfspath.c
│   ├── init
│   │   ├── entry.S
│   │   └── init.c
│   ├── libs
│   │   ├── readline.c
│   │   ├── stdio.c
│   │   └── string.c
│   ├── mm
│   │   ├── default_pmm.c
│   │   ├── default_pmm.h
│   │   ├── kmalloc.c
│   │   ├── kmalloc.h
│   │   ├── memlayout.h
│   │   ├── mmu.h
│   │   ├── pmm.c
│   │   ├── pmm.h
│   │   ├── swap.c
│   │   ├── swap.h
│   │   ├── swap_fifo.c
│   │   ├── swap_fifo.h
│   │   ├── vmm.c
│   │   └── vmm.h
│   ├── process
│   │   ├── entry.S
│   │   ├── proc.c
│   │   ├── proc.h
│   │   └── switch.S
│   ├── schedule
│   │   ├── default_sched.h
│   │   ├── default_sched_c
│   │   ├── default_sched_stride.c
│   │   ├── sched.c
│   │   └── sched.h
│   ├── sync
│   │   ├── check_sync.c
│   │   ├── monitor.c
│   │   ├── monitor.h
│   │   ├── sem.c
│   │   ├── sem.h
│   │   ├── sync.h
│   │   ├── wait.c
│   │   └── wait.h
│   ├── syscall
│   │   ├── syscall.c
│   │   └── syscall.h
│   └── trap
│       ├── trap.c
│       ├── trap.h
│       └── trapentry.S
├── lab5.md
├── libs
│   ├── atomic.h
│   ├── defs.h
│   ├── dirent.h
│   ├── elf.h
│   ├── error.h
│   ├── hash.c
│   ├── list.h
│   ├── printfmt.c
│   ├── rand.c
│   ├── riscv.h
│   ├── sbi.h
│   ├── skew_heap.h
│   ├── stat.h
│   ├── stdarg.h
│   ├── stdio.h
│   ├── stdlib.h
│   ├── string.c
│   ├── string.h
│   └── unistd.h
├── tools
│   ├── boot.ld
│   ├── function.mk
│   ├── gdbinit
│   ├── grade.sh
│   ├── kernel.ld
│   ├── mksfs.c
│   ├── sign.c
│   ├── user.ld
│   └── vector.c
└── user
    ├── badarg.c
    ├── badsegment.c
    ├── divzero.c
    ├── exit.c
    ├── faultread.c
    ├── faultreadkernel.c
    ├── forktest.c
    ├── forktree.c
    ├── hello.c
    ├── libs
    │   ├── dir.c
    │   ├── dir.h
    │   ├── file.c
    │   ├── file.h
    │   ├── initcode.S
    │   ├── lock.h
    │   ├── panic.c
    │   ├── stdio.c
    │   ├── syscall.c
    │   ├── syscall.h
    │   ├── ulib.c
    │   ├── ulib.h
    │   └── umain.c
    ├── matrix.c
    ├── pgdir.c
    ├── priority.c
    ├── sh.c
    ├── sleep.c
    ├── sleepkill.c
    ├── softint.c
    ├── spin.c
    ├── testbss.c
    ├── waitkill.c
    └── yield.c

21 directories, 176 files
```

本次实验主要是理解kern/fs目录中的部分文件，并可用user/*.c测试所实现的Simple FS文件系统是否能够正常工作。本次实验涉及到的代码包括：

* 文件系统测试用例： user/*.c：对文件系统的实现进行测试的测试用例；
* 通用文件系统接口
  n user/libs/file.[ch]|dir.[ch]|syscall.c：与文件系统操作相关的用户库实行；
  n kern/syscall.[ch]：文件中包含文件系统相关的内核态系统调用接口
  n kern/fs/sysfile.[ch]|file.[ch]：通用文件系统接口和实行
* 文件系统抽象层-VFS
  n kern/fs/vfs/*.[ch]：虚拟文件系统接口与实现
* Simple FS文件系统
  n kern/fs/sfs/*.[ch]：SimpleFS文件系统实现
* 文件系统的硬盘IO接口
  n kern/fs/devs/dev.[ch]|dev_disk0.c：disk0硬盘设备提供给文件系统的I/O访问接口和实现
* 辅助工具
  n tools/mksfs.c：创建一个Simple FS文件系统格式的硬盘镜像。（理解此文件的实现细节对理解SFS文件系统很有帮助）
* 对内核其它模块的扩充
  n kern/process/proc.[ch]：增加成员变量 struct fs_struct *fs_struct，用于支持进程对文件的访问；重写了do_execve load_icode等函数以支持执行文件系统中的文件。
  n kern/init/init.c：增加调用初始化文件系统的函数fs_init。

#### Lab8文件系统初始化过程

与实验七相比，实验八增加了文件系统，并因此实现了通过文件系统来加载可执行文件到内存中运行的功能，导致对进程管理相关的实现比较大的调整。我们来简单看看文件系统是如何初始化并能在ucore的管理下正常工作的。

首先看看kern_init函数，可以发现与lab7相比增加了对fs_init函数的调用。fs_init函数就是文件系统初始化的总控函数，它进一步调用了虚拟文件系统初始化函数vfs_init，与文件相关的设备初始化函数dev_init和Simple FS文件系统的初始化函数sfs_init。这三个初始化函数联合在一起，协同完成了整个虚拟文件系统、SFS文件系统和文件系统对应的设备（键盘、串口、磁盘）的初始化工作。其函数调用关系图如下所示：

![Alt text](image/lab8%E5%AE%9E%E9%AA%8C%E6%8A%A5%E5%91%8A/image004.png)