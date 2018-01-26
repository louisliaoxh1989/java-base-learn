
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
其他详细说明
```
jstat -class pid:显示加载class的数量，及所占空间等信息。

　　jstat -compiler pid:显示VM实时编译的数量等信息。

　　jstat -gc pid:可以显示gc的信息，查看gc的次数，及时间。其中最后五项，分别是young gc的次数，young gc的时间，full gc的次数，full gc的时间，gc的总时间。

　　jstat -gccapacity:可以显示，VM内存中三代(young,old,perm)对象的使用和占用大小，如：PGCMN显示的是最小perm的内存使用量，PGCMX显示的是perm的内存最大使用量，PGC是当前新生成的perm内存占用量，PC是但前perm内存占用量。其他的可以根据这个类推， OC是old内纯的占用量。

　　jstat -gcnew pid:new对象的信息。

　　jstat -gcnewcapacity pid:new对象的信息及其占用量。

　　jstat -gcold pid:old对象的信息。

　　jstat -gcoldcapacity pid:old对象的信息及其占用量。

　　jstat -gcpermcapacity pid: perm对象的信息及其占用量。

　　jstat -util pid:统计gc信息统计。

　　jstat -printcompilation pid:当前VM执行的信息。
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
进程所包含线程情况查询_jstack
------
```
jstack pid
```
示例
```shell
jstach 5940
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.92-b14 mixed mode):

"RMI TCP Connection(10)-10.2.13.162" #32 daemon prio=5 os_prio=0 tid=0x00000000179dc000 nid=0x1f60 in Object.wait() [0x000000001d7dd000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
        at java.lang.Object.wait(Native Method)
        at com.sun.jmx.remote.internal.ArrayNotificationBuffer.fetchNotifications(ArrayNotificationBuffer.java:449)
        - locked <0x00000000d462ec18> (a com.sun.jmx.remote.internal.ArrayNotificationBuffer)
        at com.sun.jmx.remote.internal.ArrayNotificationBuffer$ShareBuffer.fetchNotifications(ArrayNotificationBuffer.java:227)
        at com.sun.jmx.remote.internal.ServerNotifForwarder.fetchNotifs(ServerNotifForwarder.java:274)
        at javax.management.remote.rmi.RMIConnectionImpl$4.run(RMIConnectionImpl.java:1270)
        at javax.management.remote.rmi.RMIConnectionImpl$4.run(RMIConnectionImpl.java:1268)
        at javax.management.remote.rmi.RMIConnectionImpl.fetchNotifications(RMIConnectionImpl.java:1274)
        at sun.reflect.GeneratedMethodAccessor59.invoke(Unknown Source)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:324)

..............

"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x0000000002d10000 nid=0x27dc runnable

"GC task thread#2 (ParallelGC)" os_prio=0 tid=0x0000000002d11800 nid=0x2d84 runnable

"GC task thread#3 (ParallelGC)" os_prio=0 tid=0x0000000002d13800 nid=0x118 runnable

"VM Periodic Task Thread" os_prio=2 tid=0x0000000015ccb000 nid=0x2fd4 waiting on condition

JNI global references: 239
```

