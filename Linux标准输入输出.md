## 标准输入输出

#### 默认的3个文件描述符

linux启动后，会默认打开3个文件描述符

-   标准输入 standard input  ：0  stdin
-   正确输出 standard output：1 stdout  默认是屏幕
-   错误输出 error output       ：2 stderr 默认是屏幕




新打开文件，文件绑定描述符依次增加

一条shell命令执行，都会继承父进程的文件描述符。因此，所有运行的shell命令，都会有默认3个文件描述符



#### /dev/null 文件

-   `/dev/null` 是一个特殊的文件, 写入到它的内容都会被丢弃, 将命令的输出重定向到它, 会起到”禁止输出“的效果
-   如果尝试从该文件读取内容，那么什么也读不到; 可以方便的清空其他文件 `cat /dev/null > file`




### 重定向

`FD <` 输入重定向到`FD`，`0`是预设值, `0<`与`<`一样

```sh
16:52:13 › echo "test, test" > test.txt

16:52:25 › cat < test.txt
test, test

16:52:29 › cat 0< test.txt
test, test
```



`FD>`输出重定向, `1>`与`>`相同, 都是改变stdout

文件 m 和 n 合并 `>&`和`<&`

```sh
16:58:02 › ls test.txt no.test.txt
ls: no.test.txt: No such file or directory
test.txt

16:58:16 › ls test.txt no.test.txt 1>out
ls: no.test.txt: No such file or directory

16:58:28 › cat out
test.txt

16:58:32 › ls test.txt no.test.txt 2>out
test.txt

16:58:40 › cat out
ls: no.test.txt: No such file or directory

16:58:41 › ls test.txt no.test.txt 1>out 2>out.err

16:59:43 › cat out out.err
test.txt
ls: no.test.txt: No such file or directory

16:59:46 › cat out.err out
ls: no.test.txt: No such file or directory
test.txt

17:00:44 › ls test.txt no.test.txt 1>out 2>out

# 输出有被覆盖一部分
17:00:46 › cat out
test.txt
st.txt: No such file or directory

# stdout & stderr 输出到同一位置
# 將 stderr 導進 stdout 或將 stdout 導進 sterr
2>&1 就是將 stderr 併進 stdout 作輸出
1>&2 或 >&2 就是將 stdout 併進 stderr 作輸出 
17:08:43 › ls test.txt no.test.txt 1>out 2>&1
```



设置是否允许重定向到非空文件

```sh
$ set -o noclobber 
$ echo "4" > file.out 
-bash: file: cannot overwrite existing file

$ set +o noclobber 
$ echo "5" > file.out 
$ cat file.out 
5

17:16:30 › cat < out
aa

17:16:39 › cat < out > out

# 内容变空了
# 原因: 在 IO Redirection 中，stdout 與 stderr 的管道會先準備好，才會從 stdin 讀進資料
# 下例中，> file 會先將 file 清空，然後才讀進 < file 
17:16:41 › cat out
```





### 全部可用的重定向命令列表



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


