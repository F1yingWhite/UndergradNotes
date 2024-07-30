## 课程
查看已经登陆的用户数据![[Pasted image 20240529200803.png]]
管道重定向exec：![[Pasted image 20240529201706.png]]
重定向和subshell![[Pasted image 20240529202329.png]]
使用sed grep等指令![[Pasted image 20240529203106.png]]
## 作业
1. 重定向的顺序实际上也是会影响其结果的，请实践，采用如下不同顺序的重定向的结果，并分析其不同，及造成这种情况的原因（或者说产生如此结果的过程）。
```shell
[root@iZuf6j1e17uv1bufw4vjh5Z ~]# ls -yz >> cmd.log 2>&1
[root@iZuf6j1e17uv1bufw4vjh5Z ~]# cat cmd.log 
ls: invalid option -- 'y'
Try 'ls --help' for more information.
[root@iZuf6j1e17uv1bufw4vjh5Z ~]# ls -yz 2>&1 >> cmd.log
ls: invalid option -- 'y'
Try 'ls --help' for more information.
[root@iZuf6j1e17uv1bufw4vjh5Z ~]# cat cmd.log 
ls: invalid option -- 'y'
Try 'ls --help' for more information.
```
第一个命令将标准输出重定向到 `cmd.log` 文件，然后将标准错误重定向到标准输出。因此，标准输出和标准错误都会被重定向到 `cmd.log` 文件中。
第二个命令首先将标准错误重定向到标准输出，但这时标准输出还没有被重定向，所以标准错误输出仍然会显示在终端。接下来，将标准输出重定向到 `cmd.log` 文件，但标准错误输出不会被写入 `cmd.log`，因为在重定向时，标准错误已经被重定向到原始的标准输出流。
1. 请尝试执行下例，并分析说明，该例子能体现出何种符号的subshell属性。
`echo "0 $BASHPID" && echo "1 $BASHPID" | { cat && echo "2 $BASHPID";}`
输出如下：
0 1607
1 13502
2 13503
分析：每次使用&&和|都会生成子shell
1. HEREDOC还可以结合函数使用，具体请尝试如下例：
```shell
TotalFinalGrades () {  
    read name  
    read math  
    read literature  
    read physics  
    read english  
​  
    echo "\"$name\" has Total grade at: $(( $math + $literature + $physics +$english ))"  
}  
​  
TotalFinalGrades <<STUDENT001RECORD  
Michael  
18  
15  
18  
14  
STUDENT001RECORD
```
结果："Michael" has Total grade at: 65。这里使用HEREDOC传递多行参数
1. 请分析下列命令的功能是什么？并分析其区别和联系。
ls -l|cat
cat <(ls -l)
打印ls- l的输出，区别是第一个是使用管道 `|` ls -l输出直接作为另一个命令的输入
第二个是使用进程替换 (`<()`) 创建一个临时文件描述符，该描述符指向 `ls -l` 的输出，`cat` 读取这个文件描述符。
