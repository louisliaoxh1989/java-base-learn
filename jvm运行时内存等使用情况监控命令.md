
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

