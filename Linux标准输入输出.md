## 标准输入输出

linux启动后，会默认打开3个文件描述符

-   标准输入：0 standard input stdin
-   正确输出：1 standard output stdout  默认是屏幕
-   错误输出：2 error output stderr 默认是屏幕



新打开文件，文件绑定描述符可以依次增加

一条shell命令执行，都会继承父进程的文件描述符。因此，所有运行的shell命令，都会有默认3个文件描述符。



输出重定向的语法为

```sh
$ command > file
```

输入重定向

```sh
$ command < file
```

如果希望 stderr 重定向到 file，可以这样写：

```sh
$command 2 > file
## 追加写入 >>
```



## 全部可用的重定向命令列表



|       命令        |              说明               |
| :-------------: | :---------------------------: |
| command > file  |         将输出重定向到 file          |
| command < file  |         将输入重定向到 file          |
| command >> file |      将输出以追加的方式重定向到 file       |
|    n > file     |    将文件描述符为 n 的文件重定向到 file     |
|    n >> file    | 将文件描述符为 n 的文件以追加的方式重定向到 file  |
|     n >& m      |        将输出文件 m 和 n 合并         |
|     n <& m      |        将输入文件 m 和 n 合并         |
|     << tag      | 将开始标记 tag 和结束标记 tag 之间的内容作为输入 |



## /dev/null 文件

/dev/null 是一个特殊的文件, 写入到它的内容都会被丢弃；

如果尝试从该文件读取内容，那么什么也读不到; 可以方便的清空其他文件 `cat /dev/null > file`

但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到”禁止输出“的效果



