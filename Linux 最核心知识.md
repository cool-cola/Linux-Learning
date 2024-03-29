### 1. Linux 入门
1. '#'管理员输入提示符，'$'其他用户输入提示符
2. 管理员的目录为/root，普通用户的目录为/home/username
3. Linux 下有GNOME 和 KDE 两种图形操作接口，centOS 默认使用 GNOME
4. man page 文件放入 /usr/share/man， info page文件放入/usr/share/info，其他的文档在/usr/share/doc中
5. 开机进入图形界面还是字符界面配置在/etc/inittab文件
6. Linux 下安装软件可以使用 yum 开源软件
7. 启动输入法按下 ctrl + space 按键
8. Linux 下是大小写敏感的
9. 系统关键的目录为/etc、/bin、/sbin、/dev、/lib 不能与 / 根目录挂载在不同的分区上！
10. 在一些Linux 系统中，umask 的设定保存在 /etc/profile中（对所以用户有效），如果只需要当某个用户生效，那么保存在~/.bash_profile(普通用户)，
/root/.profile(root用户)中，在centOS中，umask的设定在/etc/bashrc文件中
11. 跟用户权限有关的文件有/etc/passwd、/etc/shadow、/etc/group、/etc/gshadow四个文件
    * /etc/passwd 记录用户的UID，用户名，初始组名，家目录，shell名称等
    * /etc/shadow 记录了用户的密码信息
    * /etc/group 记录了用户的所有群众的信息（gid 和组名的对应关系）
    * /etc/gshadow 记录了群组的密码
12. 开机挂载配置信息在/etc/fstab文件中，实际的文件系统挂载信息是写入到/etc/mtab 和 /proc/mounts两个文件中
13. 默认的Linux 系统中，关于tty 的数量配置在 /etc/inittab 中，在centOS6.0 以后的版本中，关于tty 的配置转移到了
/etc/init/start-ttys.conf 文件和 /etc/sysconfig/init 中
14. 输出系统的所有语系使用locale -a 命令，输出iconv 命令支持的所有语系转换使用 iconv --list命令，查看当前语系使用echo $LANG命令，
常用的中文语系有：
    * zh_CN
    * zh_CN.gb2312
    * zh_CN.gbg18030
    * zh_CN.gbk
    * zh_CN.utf8
15. 与指令相关的非常重要的目录：
    * /bin 放置开机时候，进入单人维护模式后还能够被使用的指令
    * /usr/bin 放置大部分用户指令
    * /sbin 放置开机时候，进入单人维护模式后还能够被使用的指令，不过只能被系统管理员使用
    * /usr/sbin 放置大部分的系统管理员指令
    * ~/bin 则是放置具体某一个用户的可执行命令文件的地方
    * /usr/local/bin 放置普通用户自定义安装软件的目录
    * /usr/local/sbin 放置root用户自定义安装软件的目录
    * 因此一个软件安装的时候有两种方式选择：
        * 将可执行文件安装到上述任何一个目录中，一般是/usr/bin 或者/usr/local/bin 目录下
        * 建立一个快捷方式（符号链接/软连接）文件，链接到真正的软件安装目录的可执行文件
        * 如果上述都没有执行，那么在命令行输入该软件的命令将显示无此命令
16. 在Linux 中，系统提供的函数lib 库和头文件都在/lib、/usr/lib，/lib/modules 和 /usr/include 目录中，
其中/lib/modules主要放置Linux 核心的动态库
17. 用户定义的库 和 头文件主要放在 /usr/local/lib 和 /usr/local/include 目录下
18. 理论上磁盘分区的时候，要**挂载磁盘的目录一定要是空目录，否则会造成写入覆盖**。比如/usr 挂载到sda1，而
下面有目录/usr/tst，且有数据，并且在运行的过程中运行命令将/usr/tst 被挂载到其他磁盘分区，那么原先/usr/tst的数据将
会不见，卸载后有可见了
19. 开机过程中仅有根目录会被挂载，其他分区则是在开机完后才会持续的进行挂载，不可与根目录分开的有：
    * /etc：配置文件
    * /bin：重要执行文件
    * /dev：所需要的装置文件
    * /lib：执行程序所需的函式库
    * /sbin：重要的系统执行文件
20. /usr 目录的文件放置原则请见《Linux 鸟叔私房菜 第二部》 P193~P194

### 2. 编译运行
1. Linux 连续下达多个命令用分号分隔，撤销已经输入但还没有执行的命令可用 CTRL + u
2. 非正规目录（/bin 与 /sbin）下的文件执行必须通过 ./ 来强制指定执行文件的路径
3. Linux 下的文件有修改时间 mtime，状态改变时间 ctime 和访问时间 atime，没有文件创建时间的概念
4. 每块磁盘的第一个扇区最重要，包含64bytes 的磁盘分区表和446bytes 的MBR 主要启动记录区
5. 编译程序头文件搜索路径
    * -I 指定的路径
    * gcc 的环境变量 C_INCLUDE_PATH、CPULS_INCLUDE_PATH、OBJC_INCLUDE_PATH 指定的路径
    * 寻找内定目录
        * /usr/include
        * /usr/local/include
        * /usr/lib/gcc-lib/i386-linux/2.95.2/include
        * /usr/lib/gcc-lib/i386-linux/2.95.2/../../../../include/g++-3
        * /usr/lib/gcc-lib/i386-linux/2.95.2/../../../../i386-linux/include 编译的时候
    * 如果安装gcc 时候指定了prefix 的话，那么内定目录为：
        * /usr/include
        * prefix/include
        * prefix/xxx-xxx-xxx-gnulibc/include
        * prefix/lib/gcc-lib/xxx-xxx-xxx-gnulibc/2.8.1/c
 6. 编译时库文件搜索路径
    * -L 指定的路径
    * gcc 的环境变量 LIBRARY_PATH 指定的路径
    * 内定目录指定的路径
        * /lib
        * /usr/lib
        * /usr/local/lib
7. 运行时动态库搜索路径
    * 编译目标代码时指定的动态库搜索路径（gcc 命令 -wl，-rpath）
    * 环境变量 LD_LIBRARY_PATH 指定的动态库搜素路径
    * 配置文件 /etc/ld.so.conf 中指定的动态库搜素路径
    * 默认的动态库搜索路径 /lib
    * 默认的动态库搜索路径 /usr/lib
    **记住，没有/usr/local/lib，需要自己添加到ld.so.conf 中**
   
### 3. shell 程序设计（极度重要）
1. 直接在shell 中定义变量的时候，= 两边都不能有空格
2. 在shell 脚本中定义变量的时候，= 连边可以有空格
3. shell 脚本的if 命令一定要有；号和then
4. shell 脚本的循环一定要有 do 和 done
5. python 脚本与 C++语法基本一致，区别是if、for、while 后面一定要有：符号
6. awk命令内部的 command 语法与shell 的语法完全不一样，这个很坑，不过幸运的是 awk 内部的语法很符合C++ 的语法，比shell 语法好用很多了
7. 在makefile 中使用shell命令，语法和 shell 脚本一样，不过makefile 中要使用 shell 中的变量，需要两个$$符号，而不是一个$符号，比如：
'''> #makefile
>
>1. 在sed 命令中的正则表达式中定义的变量都是shell 变量，如$符号
>2. 在awk 命令中预设的变量都是 shell 变量，比如$0, $1 等
>
> VERSION:=$(shell if [$$(git rev-parse --is-inside-work-tree)]);then git svn info;else svn info; fi | awk '{if(NR==5){print $$0}}'|awk 'print $$2'
>3. sed -i '$$a \\tgcc -o $(subst .d, .o, $@) -c $<' $@;
>4. #SUBDIRS 是makefile 变量
> SUBDIRS= RttCalculator RttClient RttStore RttSlideWindow RttCalculator
>5. #define 定义一个命令包，其中subdir 是在shell 命令中定义的，所以是 shell 变量，需要$$ 符号
 ```  
   define make_subdir
   @for subdir in $(SUBDIRS); do \
           (cd $$subdir && make $1) \
   done;
   endef;
```

8. 在awk 命令中打印用print，在shell 中用 echo
9. 在awk 命令中也可以使用shell 中定义得变量，但是默认是不能使用的，需要做特殊处理
   coreNumber='cat /proc/cpuinfo | grep processor | wc -l'
   cpulist='ps auxf --width=1000 | grep $1 | grep -v grep | awk -v cpuCore=$coreNumber '{print $3/cpuCores}'';
10. 直接在shell 窗口输入 shell 命令是可以换行的，在shell 脚本中输入shell 命令也可以换行
11. 在shell 脚本中使用awk 命令，在awk 中的''部分中的命令，也是可以换行的，不过在BEGIN 命令后都必须紧跟{符号，在END 前面必须紧挨着}符号
12. 在shell 脚本和awk 命令中，对字符串的拼接不能采用+、+=符号，而是直接将多个变量放在一起就可以拼接了，比如
   var='abc'
   var2='def'
   myvar=$var$var2
13. shell 中对数值的操作需要'a=$(($a+$b))',不能'a=$($a+$b)'，更不能'a=$a+$b'
14. 在awk 中使用变量就像c语言，完全不用加上$符号，在shell 中则必须加上$符号
15. 对于 shell 脚本中的 [] 和 [[]] 判断符号，在中括号内的每一个组件都要有空格键来分隔！
16. 在 awk 中要调用 shell 命令，需要用 system 函数，在python 中也是一样，需要使用system 函数来调用 shell命令
17. awk 命令中能够使用的函数，makefile中能够使用的函数，shell命令中能够使用的函数，均有所不同
18. shell 脚本中true 是 0， false 是 1
19. shell 可以使用（（））、[]、[[]]，test 作为判断符号，语法格式如下
```
if ((a > b)) || ((a < c))
if [[$a > $b]] || [[$a < $c]]
if [$a -gt $b -o $a -lt $c]
```
20. (())、[]、[[]] 都支持对数值使用 >, <, <=, >=, !=等操作符
21. (())不支持试用-eq，-lt、-gt、-ne、-le、-ge等选项，而[] 和[[]] 则支持这些选项
22. (()) 和[[]] 内部支持用||、&&、!符号表达逻辑关系
23. [] 内部不支持用||、&&、!符号表达逻辑关系
24. (()) 中引用变量不需要$ 符号，其他任何shell 地方都需要使用$ 符号，这是最关键的
25. python 用 or、and、not表达逻辑关系，不能用||、&&、！符号表达逻辑关系
26. shell 本身不支持浮点数运算，(()) 之类的数学运算符只能进行整数运算。要想在shell 中运算浮点数，只能借助于bc，awk或则perl命令
27. awk 的所有数字，都以double 的浮点值表示，所以算术运算都是浮点运算
28. 字符串转数字：`s='123', n=s+0` 那么此时n就是数字了，而且还是double型，如果需要为整型，可以用`n=int(s)`达到目的
------------------------------------
* shell 一定要写在target 里，否则你写了也白费劲，是会被make 忽略的
```
@echo "Building all..."
all:
    @echo "Begin"
# 只会打印Begin
```
* 每一行的 shell 都是在独立的进程中运行的，如果你在上一行为一个变量赋值，不要指望他在下一行有效，比如：
```
@CROSS_COMPILE=mipsel-linux-
@echo $(CROSS_COMPILE)
```
打印的结果不会是mipsel-linux-，如果你想得到所要的结果，就并为一条shell 吧：
```
@CROSS_COMPILE=mipsel-linux-；echo @echo $(CROSS_COMPILE)
或者
@CROSS_COMPILE=mipsel-linux- \
   echo @echo $(CROSS_COMPILE)
```




