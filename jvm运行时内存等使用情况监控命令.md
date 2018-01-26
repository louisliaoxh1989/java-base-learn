
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

```shell
// 示例
jmap -heap 5940
Attaching to process ID 5940, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.92-b14

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 734003200 (700.0MB)
   NewSize                  = 44040192 (42.0MB)
   MaxNewSize               = 244318208 (233.0MB)
   OldSize                  = 88080384 (84.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 32505856 (31.0MB)
   used     = 13906760 (13.262519836425781MB)
   free     = 18599096 (17.73748016357422MB)
   42.782322052986395% used
From Space:
   capacity = 6291456 (6.0MB)
   used     = 294912 (0.28125MB)
   free     = 5996544 (5.71875MB)
   4.6875% used
To Space:
   capacity = 7340032 (7.0MB)
   used     = 0 (0.0MB)
   free     = 7340032 (7.0MB)
   0.0% used
PS Old Generation
   capacity = 41943040 (40.0MB)
   used     = 6127536 (5.8436737060546875MB)
   free     = 35815504 (34.15632629394531MB)
   14.609184265136719% used

8535 interned Strings occupying 710344 bytes.
```

