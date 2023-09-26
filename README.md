# 4190.307 Operating Systems (Fall 2023)
# Project #2: System Calls
### Due: 11:59 PM, October 8 (Sunday)

## Introduction

System calls are the interfaces that allow user applications to request various services from the operating system kernel. This project aims to understand how those system calls are implemented in `xv6`.

## Problem specification

Your task is to implement a new system call named `ntraps()` that returns the total number of system calls or interrupts received by the `xv6` kernel. Detailed project requirements are described below.

**SYNOPSIS**
```
#define N_SYSCALL     0
#define N_INTERRUPT   1
#define N_TIMER       2

int ntraps(int type);
```

**DESCRIPTION**

In `xv6`, the term _trap_ is used as a generic term encompassing both _exceptions_ and _interrupts_ (see Chap. 4 of the [xv6 book](http://csl.snu.ac.kr/courses/4190.307/2023-2/book-riscv-rev3.pdf)). The system call `ntraps()` is used to get the total number of system calls, interrupts, or timer interrupts received by the `xv6` kernel since the system is booted. The actual return value depends on the `type` argument, as shown below.

**RETURN VALUE**

`ntraps()` returns the following value. 

* `ntraps(N_SYSCALL)` or `ntraps(0)` returns the total number of system calls (including the current `ntraps()` system call) made for all cores since the system is booted.
* `ntraps(N_INTERRUPT)` or `ntraps(1)` returns the total number of device interrupts (including timer interrupts) received by all cores since the system is booted. Software interrupts issued by the RISC-V hart should not be counted.
* `ntraps(N_TIMER)` or `ntraps(2)` returns the total number of timer interrupts received by all cores since the system is booted.
* Otherwise, -1 is returned.


## Restrictions

* Each count should be initialized to 0 on boot.
* The system call number for `ntraps()` is already assigned to 22 in the `./kernel/syscall.h` file. Please do not change this number.
* You only need to change the files in the `./kernel` directory. Any other changes will be ignored during grading.
* Do not change the `./kernel/start.c` file in the `./kernel` directory. 
* You must ensure the kernel tracks the number of system calls, interrupts, or timer interrupts correctly, even on multi-core systems. (Hint: See Chap. 6.1 of the [xv6 book](http://csl.snu.ac.kr/courses/4190.307/2023-2/book-riscv-rev3.pdf).)

## Tips

* Read Chap. 4.1 of the [xv6 book](http://csl.snu.ac.kr/courses/4190.307/2023-2/book-riscv-rev3.pdf) to understand RISC-V's privileged modes (supervisor mode and machine mode) and trap handling mechanism. More detailed information can be found in the [RISC-V Privileged Architecture manual](http://csl.snu.ac.kr/courses/4190.307/2023-2/riscv-privileged-20211203.pdf).
* Read Chap. 4.2 ~ 4.5 of the [xv6 book](http://csl.snu.ac.kr/courses/4190.307/2023-2/book-riscv-rev3.pdf) to see how traps (system calls and interrupts) are handled in xv6.
* Read Chap. 5.1 ~ 5.4 of the [xv6 book](http://csl.snu.ac.kr/courses/4190.307/2023-2/book-riscv-rev3.pdf) to learn about hardware interrupts.
  
## Skeleton code

The skeleton code for this project assignment (PA2) is available as a branch named `pa2`. Therefore, you should work on the `pa2` branch as follows:

```
$ git clone https://github.com/snu-csl/xv6-riscv-snu
$ git checkout pa2
```

After downloading, the first thing you have to do is to set your `STUDENTID` in the `Makefile` again.

The `pa2` branch has a user-level utility program called `ntraps`, which simply calls the `ntraps()` system call with the given argument. The source code of the `ntraps` utility is available in the `./user/ntraps.c` file. Using this program, you can get the value returned by the `ntraps()` system call.

Currently, any kernel-level implementation for supporting the `ntraps()` system call is missing. So, if you run the `ntraps` utility in the shell, you will get an error as shown below.

```
qemu-system-riscv64 -machine virt -bios none -kernel kernel/kernel -m 128M -smp 3 -nographic -global virtio-mmio.force-legacy=false -drive file=fs.img,if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0

xv6 kernel is booting

hart 2 starting
hart 1 starting
init: starting sh
$ ntraps
3 ntraps: unknown sys call 22
ntraps: kernel returns an error -1
$
```

## Hand in instructions

* First, make sure you are on the `pa2` branch in your `xv6-riscv-snu` directory. And then perform the `make submit` command to generate a compressed tar file named `xv6-{PANUM}-{STUDENTID}.tar.gz` in the `../xv6-riscv-snu` directory. Upload this file to the submission server.

* The total number of submissions for this project assignment will be limited to 30. Only the version marked `FINAL` will be considered for the project score. Please remember to designate the version you wish to submit using the `FINAL` button. 
  
* Note that the submission server is only accessible inside the SNU campus network. If you want off-campus access (from home, cafe, etc.), you can add your IP address by submitting a Google Form whose URL is available in the eTL. Note that adding your new IP address is a ___manual___ process, so please do not expect it to be completed immediately.

## Logistics

* You will work on this project alone.
* Only the upload submitted before the deadline will receive the full credit. 25% of the credit will be deducted for every single day delay.
* __You can use up to _3 slip days_ during this semester__. If your submission is delayed by one day and you decide to use one slip day, there will be no penalty. In this case, you should explicitly declare the number of slip days you want to use in the QnA board of the submission server right after each submission. Once slip days have been used, they cannot be canceled later, so saving them for later projects is highly recommended!
* Any attempt to copy others' work will result in a heavy penalty (for both the copier and the originator). Don't take a risk.

Have fun!

[Jin-Soo Kim](mailto:jinsoo.kim_AT_snu.ac.kr)  
[Systems Software and Architecture Laboratory](http://csl.snu.ac.kr)  
[Dept. of Computer Science and Engineering](http://cse.snu.ac.kr)  
[Seoul National University](http://www.snu.ac.kr)
