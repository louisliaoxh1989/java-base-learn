
查看系统里java进程信息
------
```shell
#查看当前机器上所有运行的java进程名称与pid(进程编号)
jps -l 
#显示指定的jvm进程所有的属性设置和配置参数
jinfo pid
```

内存占用情况查询jmap
------

```shell
#查询某个pid进程对应的应用程序内存占用情况
jmap -heap pid
```
### 参数
```

　　-heap ：打印jvm heap的情况

　　-histo： 打印jvm heap的直方图。其输出信息包括类名，对象数量，对象占用大小。

　　-histo：live ： 同上，但是只答应存活对象的情况

　　-permstat： 打印permanent generation heap情况
```

示例

```shell
#可以观察到New Generation(Eden Space，From Space，To Space),tenured generation,Perm Generation的内存使用情况
jmap -heap 5940
#可以观察heap中所有对象的情况(heap中所有生存的对象的情况)。包括对象数量和所占空间大小。
jmap -histo 3409 | jmap -histo:live 3409

```
实时监测系统资源占用与jvm运行情况_jstat
------
```shell
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
```

### 参数

```shell
Options — 选项，我们一般使用 -gcutil 查看gc情况
vmid    — VM的进程号，即当前运行的java进程号
interval– 间隔时间，单位为秒或者毫秒
count   — 打印次数，如果缺省则打印无数次

S0  — Heap上的 Survivor space 0 区已使用空间的百分比
S1  — Heap上的 Survivor space 1 区已使用空间的百分比
E   — Heap上的 Eden space 区已使用空间的百分比
O   — Heap上的 Old space 区已使用空间的百分比
P   — Perm space 区已使用空间的百分比
YGC — 从应用程序启动到采样时发生 Young GC 的次数
YGCT– 从应用程序启动到采样时 Young GC 所用的时间(单位秒)
FGC — 从应用程序启动到采样时发生 Full GC 的次数
FGCT– 从应用程序启动到采样时 Full GC 所用的时间(单位秒)
GCT — 从应用程序启动到采样时用于垃圾回收的总时间(单位秒)
```
示例

```shell
jstat -class -t 5940
Timestamp Loaded  Bytes  Unloaded  Bytes     Time
6188.4   3898  7178.4       40    58.3       1.78

jstat -gcutil 5940 1000 5
S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT
0.00  25.00  98.55  15.37  96.94  94.88     21    0.069     7    0.237    0.306
0.00  25.00  99.59  15.37  96.94  94.88     21    0.069     7    0.237    0.306
0.00  25.00  99.59  15.37  96.94  94.88     21    0.069     7    0.237    0.306
0.00  25.00 100.00  15.37  96.94  94.88     21    0.069     7    0.237    0.306
0.00  25.00 100.00  15.37  96.94  94.88     21    0.069     7    0.237    0.306
```

