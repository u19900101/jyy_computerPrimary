# PA0

## 1.Makefile

### 1.准备3个文件

hellomake.c	

```bash
#include <hellomake.h>

int main() {
  // call a function in another file
  myPrintHelloMake();

  return(0);
}
```

hellofunc.c	

```bash
#include <stdio.h>
#include <hellomake.h>

void myPrintHelloMake(void) {
  printf("Hello makefiles!\n");
  return;
}
```

hellomake.h

```bash
/*
example include file
*/
void myPrintHelloMake(void);
```

Normally, you would compile this collection of code by executing the following command:

```
gcc -o hellomake hellomake.c hellofunc.c -I.
```

新建 makefile 文件，执行 `make`命令

```bash
# 1.Makefile 1
hellomake: hellomake.c hellofunc.c
     gcc -o hellomake hellomake.c hellofunc.c -I.
     
# 2.Makefile 2
CC=gcc
CFLAGS=-I.
hellomake: hellomake.o hellofunc.o
     $(CC) -o hellomake hellomake.o hellofunc.o

# 3.Makefile 3
CC=gcc
CFLAGS=-I.
DEPS = hellomake.h

%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

hellomake: hellomake.o hellofunc.o 
	$(CC) -o hellomake hellomake.o hellofunc.o 
	
# 4.Makefile 4
CC=gcc
CFLAGS=-I.
DEPS = hellomake.h
OBJ = hellomake.o hellofunc.o 

%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

hellomake: $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS)
	
# 5.Makefile 5  暂时没看明白
IDIR =../include
CC=gcc
CFLAGS=-I$(IDIR)

ODIR=obj
LDIR =../lib

LIBS=-lm

_DEPS = hellomake.h
DEPS = $(patsubst %,$(IDIR)/%,$(_DEPS))

_OBJ = hellomake.o hellofunc.o 
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))


$(ODIR)/%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

hellomake: $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS) $(LIBS)

.PHONY: clean

clean:
	rm -f $(ODIR)/*.o *~ core $(INCDIR)/*~ 
```

### 2.执行结果

```bash
[root@localhost pa0]# vim makefile
[root@localhost pa0]# make
gcc -o hellomake hellomake.o hellofunc.o -I.
[root@localhost pa0]# ./hellomake 
Hello makefiles!
```

## 2.GDB

https://www.cprogramming.com/gdb.html

## 3. Installing tmux

[使用教程](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

安装  apt-get install vim

```bash
# 解决 vim中无法粘贴
# .改vim配置文件
#root@kvm1:/etc/network# vim /usr/share/vim/vim81/defaults.vim
将 set mouse=a 改为：set mouse-=a
```

```bash
deb http://mirrors.tencentyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.tencentyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.tencentyun.com/ubuntu/ bionic-updates main restricted universe multiverse
#deb http://mirrors.tencentyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
#deb http://mirrors.tencentyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.tencentyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.tencentyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.tencentyun.com/ubuntu/ bionic-updates main restricted universe multiverse
#deb-src http://mirrors.tencentyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
#deb-src http://mirrors.tencentyun.com/ubuntu/ bionic-backports main restricted universe multiverse

# 阿里云源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse


# 163源
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse


# 清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse


# 中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```



```bash
apt-get install tmux
cd ~
vim .tmux.conf
bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -c "#{pane_current_path}"
```

安装git

修改apt-get源

```bash
# 1.安装 apt-get install gnupg  #为了配置公钥
# 2.备份配置源
sudo cp /etc/apt/sources.list /etc/apt/sources.list-bak
# 3.修改源  编辑配置文件/etc/apt/sources.list
# 中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

# 4.更新源 && 升级软件包
sudo apt-get update
报错 没有 3B4FE6ACC0B21F32 公钥


# 5.安装公钥   继续报错
gpg: keyserver receive failed: Server indicated a failure

#6.选80 端口  再次安装  终成
root@buster:/etc/apt# sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 3B4FE6ACC0B21F32
Executing: /tmp/apt-key-gpghome.XPYhTPFZls/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 --recv 3B4FE6ACC0B21F32
gpg: key 3B4FE6ACC0B21F32: public key "Ubuntu Archive Automatic Signing Key (2012) <ftpmaster@ubuntu.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1

# 7.更新源 && 升级软件包
sudo apt-get update
sudo apt-get dist-upgrade

# 8.再次安装git 直接飞起
```





无法安装g++

```bash
# aptitude可以比apt-get更加智能地解决依赖问题，先安装它：
sudo apt-get install aptitude
# 使用aptitude：
sudo aptitude install build-essential
sudo aptitude install g++
sudo aptitude install libreadline-dev 
# 1.选择no
# 2.强制安装  最后yes
# 3.然后就好了。。
```





最重要的一点是： 退出当前会话请使用

-  ctrl+ b  d    (先同时按 ctrl b  再按 d)
- 不要使用  ctrl+d  排除这个错误找了很久



>- `Ctrl+b %`：划分左右两个窗格。
>- `Ctrl+b "`：划分上下两个窗格。
>- `Ctrl+b `：光标切换到其他窗格。``是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键`↓`。
>
>- `Ctrl+b x`：关闭当前窗格。
>- `Ctrl+b !`：将当前窗格拆分为一个独立窗口。
>- `Ctrl+b z`：当前窗格全屏显示，再使用一次会变回原来大小。
>- `Ctrl+b Ctrl+`：按箭头方向调整窗格大小。

## 4.Getting Source Code for PAs

~~centOS下安装sdl 2.0~~

~~下载链接：http://www.libsdl.org/download-2.0.php~~

### 1.Getting Source Code

ow get the source code for PA by the following command:

```
git clone -b 2020 https://github.com/NJU-ProjectN/ics-pa.git ics2020
```

A directory called `ics2020` will be created. This is the project directory for PAs. Details will be explained in PA1.

Then issue the following commands to perform `git` configuration:

```bash
git config --global user.name "u19900101" # your student ID and name
git config --global user.email "815000342@qq.com"   # your email
git config --global core.editor vim                 # your favorite editor
git config --global color.ui true
```

You should configure `git` with your student ID, name, and email. Before continuing, please read [this](https://nju-projectn.github.io/ics-pa-gitbook/ics2020/git.html) `git` tutorial to learn some basics of `git`.

Enter the project directory `ics2020`, then run

```
git branch -m master
bash init.sh nemu
bash init.sh abstract-machine
```

to initialize some subprojects. The script will pull some subprojects from github. We will explain them later.

Besides, the script will also add some environment variables into the bash configuration file `~/.bashrc`. These variables are defined by absolute path to support the compilation of the subprojects. Therefore, DO NOT move your project to another directory once finishing the initialization, else these variables will become invalid. Particularly, if you use shell other than `bash`, please set these variables in the corresponding configuration file manually.

To let the environment variables take effect, run

```
source ~/.bashrc
```

Then try

```
echo $NEMU_HOME
echo $AM_HOME
cd $NEMU_HOME
cd $AM_HOME
```

###  2.Compiling and Running NEMU

Now enter `nemu/` directory, and compile the project by `make`:

```
make
```

If nothing goes wrong, NEMU will be compiled successfully.

```bash
# 各种报错没有相关的库  随性安装   apt-get install 无法进行下去
# 第一个选 n 后面的都选yes 来回折腾几次就好了。。。
sudo aptitude install libreadline-dev
sudo aptitude install libsdl2-dev  
```

大功告成

```bash
root@buster:~/ics2020/nemu# make
Building x86-nemu-interpreter
+ CC src/device/keyboard.c
+ CC src/device/vga.c
+ CC src/device/audio.c
+ CC src/memory/paddr.c
+ CC src/main.c
+ CC src/isa/x86/decode.c
+ CC src/isa/x86/difftest/dut.c
+ CC src/isa/x86/reg.c
+ CC src/isa/x86/intr.c
+ CC src/isa/x86/mmu.c
+ CC src/isa/x86/exec/special.c
+ CC src/isa/x86/exec/exec.c
+ CC src/isa/x86/logo.c
+ CC src/isa/x86/init.c
+ CC src/engine/interpreter/init.c
+ LD build/x86-nemu-interpreter
```



# PA1

## 1.框架代码初探

```bash
ics2020
├── abstract-machine   # 抽象计算机
├── fceux-am           # 红白机模拟器
├── init.sh            # 初始化脚本
├── Makefile           # 用于工程打包提交
├── nemu               # NEMU
└── README.md
```



### 准备第一个客户程序

第一项工作就是将一个内置的客户程序读入到内存中, 因此我们需要一种方式让客户计算机的CPU知道客户程序的位置. 我们采取一种最简单的方式: 约定. 具体地, 我们让monitor直接把客户程序读入到一个固定的内存位置`IMAGE_START`(也就是`0x100000`).



事实上, 在GNU/Linux中, 你可以很容易得知操作系统在背后做了些什么. 键入`sudo dmesg`, 就可以输出操作系统的启动日志, 操作系统的行为一览无余.

### 运行第一个客户程序

在nemu文件夹下运行

```bash
# Supported: mips32 riscv32 riscv64 x86.  Stop.
make ISA=mips32 run
# make ISA=x86 run
...
Welcome to mips32-NEMU!
For help, type "help"
(nemu) ^CMakefile:106: recipe for target 'run' failed
```

```bash
typedef struct {
           union {
	          union {
		           uint32_t _32;
		           uint16_t _16;
		           uint8_t _8[2];
	          } gpr[8];

	/* Do NOT change the order of the GPRs' definitions. */
    	      struct {
		           uint32_t eax, ecx, edx, ebx, esp, ebp, esi, edi;
		      };
		   };
		  swaddr_t eip;
}CPU_state;	
```



就是这么简单

事实上, TRM的实现已经都蕴含在上述的介绍中了.

- 存储器是个在`nemu/src/memory/paddr.c`中定义的大数组
- PC和通用寄存器都在`nemu/include/isa/$ISA.h`中的结构体中定义
- 加法器在... 嗯, 这部分框架代码有点复杂, 不过它并不影响我们对TRM的理解, 我们还是在PA2里面再介绍它吧
- TRM的工作方式通过`cpu_exec()`和`isa_exec_once()`体现

在NEMU中, 我们只需要一些很简单的C语言知识就可以理解最简单的计算机的工作方式, 真应该感谢先驱啊.

## pa1.1. 基础设施:

### 1.简易调试器

为了提高调试的效率, 同时也作为熟悉框架代码的练习, 我们需要在monitor中实现一个具有如下功能的简易调试器 (相关部分的代码在`nemu/src/monitor/debug/`目录下), 如果你不清楚命令的格式和功能, 请参考如下表格:

| 命令         | 格式          | 使用举例          | 说明                                                         |
| ------------ | ------------- | ----------------- | ------------------------------------------------------------ |
| 帮助(1)      | `help`        | `help`            | 打印命令的帮助信息                                           |
| 继续运行(1)  | `c`           | `c`               | 继续运行被暂停的程序                                         |
| 退出(1)      | `q`           | `q`               | 退出NEMU                                                     |
| 单步执行     | `si [N]`      | `si 10`           | 让程序单步执行`N`条指令后暂停执行, 当`N`没有给出时, 缺省为`1` |
| 打印程序状态 | `info SUBCMD` | `info r` `info w` | 打印寄存器状态 打印监视点信息                                |
| 扫描内存(2)  | `x N EXPR`    | `x 10 $esp`       | 求出表达式`EXPR`的值, 将结果作为起始内存 地址, 以十六进制形式输出连续的`N`个4字节 |
| 表达式求值   | `p EXPR`      | `p $eax + 1`      | 求出表达式`EXPR`的值, `EXPR`支持的 运算请见[调试中的表达式求值](https://nju-projectn.github.io/ics-pa-gitbook/ics2020/1.6.html)小节 |
| 设置监视点   | `w EXPR`      | `w *0x2000`       | 当表达式`EXPR`的值发生变化时, 暂停程序执行                   |
| 删除监视点   | `d N`         | `d 2`             | 删除序号为`N`的监视点                                        |

备注:

- (1) 命令**已实现**
- (2) 与GDB相比, 我们在这里做了简化, 更改了命令的格式

### 2.解析命令





## pa1.2. 表达式求值

## pa1.3.监视点

### 5.如何阅读手册