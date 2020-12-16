



[TOC]

### sed简介

流编辑器，用来过滤和转换文件中的内容的

sed一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕；接着处理下一行，这样不断重复，直到文件末尾

### 格式

sed [options] 'command' filename(s)

sed的替换命令格式有三种：
1) s/A/B/g
2) s#A#B#g
3) s_A_B_g

#### 参数

* -n ：使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到终端上。但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行(或者动作)才会被列出来。

* -e ：直接在命令列模式上进行 sed 的动作编辑；（加多个-e，表示可以多点编辑）

* -f ：直接将 sed 的动作写在一个文件内， -f filename 则可以运行 filename 内的 sed 动作；

* -r ：sed 的动作支持的是延伸型正规表示法的语法。(默认是基础正规表示法语法)

* -i [SUFFIX]：`直接修改读取的文件内容`，而不是输出到终端。（这个很关键！）`如果指定一个后缀，则会自动创建备份`

* --follow-symlinks：

  ​		如果是软连接，则会修改连接所指向的原始文件

  ​		如果是硬连接，则会打破硬链接之间的关联关系

#### 动作

* a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行);
* c ：取代， c 的后面可以接字串，这些字串可以取代 多行 之间的内容;
* d ：删除，因为是删除啊，所以 d 后面通常不接任何内容；
* i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
* p ：列印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行;
* s ：取代，可以直接进行取代的工作。通常这个 s 的动作可以搭配正规表示法;例如 sed -i '1,20 s/old/new/g' filename 将第1行到20行的old替换为new

#### 注意

sed 后面接的动作，最好以''单引号括住。当然用""双引号也可以。
如果使用单引号，那么就没办法通过\’这样来转义，就有双引号就可以了，在双引号内可以用\”来转义。

### sed的简单格式举例

#### 过滤行包含指定字符串

```
sed -n '/string/p' fileName
```

#### 删除包含指定字符串的行

```
sed -n '/string/d' fileName
```

#### p打印内容

```
例子：显示文件test中的第二行的内容
# sed -n '2p' test

例子：显示文件test中的第二~四行的内容
# sed -n '2,4p' test

例子：显示文件test中的第二~末行的内容
# sed -n '2,$p' test
```

#### d删除内容

```
例子：显示文件test中的末行的内容
# sed -n '$p' test

例子：删除第一行的内容
# sed '1d' test

例子：删除文件中的全部的内容
# sed '1,$d' test

例子：删除前4行的内容
# sed '1,4d' test  //这里仅仅删除的是模式空间的内容，对原始文件不会修改
```

#### c替换内容

```
例子：将第2行，替换成i am zxhk
# sed '2c i am zxhk' test

例子：将第2~4之间的内容替换为 i am zxhk
# sed '2,4c i am zxhk' test
```

#### 显示包含指定字符串的行

`匹配行号 'NUMAction'`

`匹配字符 '/string/Action'`

```
显示包含字符 oo 的行
# sed -n '/oo/p' test

例子：显示包含root字符串的行
# sed -n '/root/p' test

例子：显示包含root单词的行
# sed -n '/\<root\>/p' test

例子：显示以root字符串为开头的行
# sed -n '/^root/p' test

例子：显示以root单词为开头的行
# sed -n '/^root\>/p' test
```

#### 查找替换

`s/要匹配的内容/要替换成的内容/[g]`

​	g：全局替换。默认只会替换每行匹配到的第一个对象

​	i：忽略大小写

```
将root单词替换成zxhk
# sed -i 's/\<root\>/zxhk/g' test //替换文件
# sed -n 's/\<root\>/zxhk/p' test //打印
```

#### 查找字符串后执行命令

`{ }：在这里面写要执行的命令,多个命令之间用分号分隔`

```
例子：将shell类型为bash的行中的root替换为zxhk
# sed -n '/bash/{s/\<root\>/zxhk/g;p;q}' test

例子：将shell类型为bash的行中的root改为zxhk，0改为root
# sed -n '/bash/{s/root/zxhk/g;s/0/100/g;p;q}'
```

#### 多点编辑

同时执行多个命令（操作）`-e`

```
例子：将文件中的空白行和#开头的行都删除
# sed -e '/^#/d' -e '/^$/d' test

例子：将3到末行的内容全部删除，而且将root替换为zxhk，将0替换为1
# sed -e '3,$d' -e 's/root/zxhk/g' -e 's/0/1/g' test
```

#### 原地修改

`-i选项`

```
例子：将文件中的空白行和#开头的行都删除
# sed -i -e '/^#/d' -e '/^$/d' test
```

#### 取反操作

`! 原基础的不操作`

```
例子：显示以非 # 开头的行
# sed -n '/^#/!p' test

例子：显示非1-3行之间的内容
# sed -n '4,$p' test
or
# sed -n '1,3!p' test

例子：在包含字符串root的行下添加一行 i zm zxhk
# sed '/root/a i am zxhk' test

例子：在不包含字符串root的行下各添加一行 i zm zxhk
# sed '/root/!a i am zxhk' test
```

#### 从一个文件中读取内容添加到另一个文件的指定位置

`r`

```
例子：从文件info 中读取内容写入到test的第二行的后面
# sed '2r info' test

例子：从文件info 中读取内容写入到test的第二~四行的后面
# sed '2,4r info' test

例子：从文件info 中读取内容写入到test中包含字符root的行的后面
# sed '/root/r inof' test

例子：从文件info 中读取内容写入到test中以#开头的行的后面
# sed '/^#/r info' test
```

#### 将匹配到的行写入到一个文件中

`w`

```
例子：将包含字符root的行写入到文件a.txt
# sed '/root/w a.txt' test
```

#### 引用前面搜索匹配的结果

```
例子：将r和t之间有两字符的单词的后面添加上er
# sed 's/\<r..t\>/&er/g' test

or

# sed -r 's/(\<r..t\>)/\1er/g' test
```

#### 大神博客链接

[sed操作详解](https://www.cnblogs.com/kevingrace/p/5949208.html)

[sed&awk奇偶行方法](https://www.cnblogs.com/kevingrace/p/10540374.html)