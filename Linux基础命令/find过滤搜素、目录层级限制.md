参考博客：

[大佬的详细博客](https://www.cnblogs.com/kevingrace/p/11907695.html)

#### find过滤目录

忽略某些目录，可以使用`-path 过滤的目录路径 -prune -o`参数来进行过滤

```
比如查找/data/web/ssy/online路径下的的目录，并统计目录大小，以G位单位进行排序（默认为降序），并统计前10个大小的目录。命令如下：

# find /data/web/ssy/online/* -maxdepth 0 -type d -exec /usr/bin/du -sh {} \;|grep '[0-9]G'|sort -rh|head -10
 
查找/data/web/ssy/online路径下除tmp目录之外的目录，并统计目录大小，以G位单位进行排序（默认为降序），并统计前10个大小的目录。命令如下

# find /data/web/ssy/online/* -path /data/web/ssy/online/tmp -prune -o -maxdepth 0 -type d -exec /usr/bin/du -sh {} \;|grep '[0-9]G'|sort -rh|head -10
```

<font color='red'>注意：</font>

1. "-maxdepth 0"  表示只查找到/data/web/ssy/online下的目录。如果是"-maxdepth 1"则表示查找到/data/web/ssy/online/xxx下的目录
2. find命令中的过滤、忽略、排除使用`-path 过滤的文件或目录-prune -o `，其中-prune类似于`if`判断，如果-prune之前的语句为真，比如找到了前面-path指定的/data/web/ssy/online/tmp目录，就不再执行后面-o跟的语句了，如果没有找到则执行后面的语句。这样就做到了排除效果！
   其中的"-o" 是 "-or" 的意思！
3. `-path要过滤掉的文件或目录路径参数一定要紧跟在要搜索的路径之后`，否则过滤效果就不会实现！！也就是说上面的"-path /data/web/ssy/online/tmp"必须紧跟着放在"/data/web/ssy/online/*"后面，否则查找时就不会过滤掉/data/web/ssy/online/tmp这个目录
4. 当要搜索的目录不是全路径时，要过滤掉的目录必须是"./test2"才能实现过滤效果。如果是"test2"或者"./test2/"都不能实现过滤效果
5. 当要搜索的目录是全路径时，要过滤掉的目录也必须是全路径才能实现过滤效果！要过滤掉的目录后面不能加"/"，否则也不能实现过滤效果

#### find过滤文件

先查看对应文件，然后使用"grep -v"进行过滤，集合-maxdepth控制目录搜索层级

```
查找目录下的文件，并过滤某个文件

find . -maxdepth 1 -type f |grep -v "haha2"|grep -v haha3|grep -v haha4
```

#### 控制搜索深度的选项

-maxdepth ：指定遍历搜索的最大深度。最大目录层级

-mindepth： 指定开始遍历搜索的最小深度。最小目录层级

* -maxdepth 0：最大目录层级为0，表示只针对当前目录本身(比如/opt/kevin)进行搜索操作或du -sh 统计操作。
* -maxdepth 1：最大目录层级为1，表示针对/opt/kevin/ 路径进行搜索操作或du -sh 统计操作。
* -maxdepth 2：最大目录层级为2，表示针对/opt/kevin/xxx/ 路径进行搜索操作或du -sh 统计操作。

`但是如果当前路径加入"*"使用"-maxdepth 0" 效果和 当前路径不加"*"使用"-maxdepth 1" 是一样的！`

```
find ./* -maxdepth 0 -type f

等价于

find . -maxdepth 1 -type f
```

