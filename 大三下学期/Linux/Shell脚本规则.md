\\用来链接两行
command1 && command2条件执行,只有command1退出状态为1,command2才会执行
|| 前面成功后面就不执行
```shell
if command1
then
	command2
	..
elif
	...
fi
```
这里让命令的退出状态当做执行
方括号命令用于条件判断。方括号内可以使用各种条件表达式来判断条件是否成立。方括号命令的语法如下：
[ condition ] ,用法和test差不多
例子：
$ [ -f file.txt ] && echo “file.txt exists”
file.txt exists
这个例子判断当前目录下是否存在file.txt文件，如果存在则输出”file.txt exists”。
-eq 表示判断
默认值设置 ${var1:="some default value"}
${1}表示传入参数的第一个值
```shell
set -- ${ls}   #把ls的结果置为位置参数,可以用位置参数来获得
echo ${1}
```

```shell
read -p "请输入变量" x #用于读入输入.-p是提示
case $x in
	var1|var3|var4)
		...
		;;
	var2)
		...
		;;
esac

while command
do
	...
done
```

```shell
变量说明:   
$$   
Shell本身的PID（ProcessID）   
$!   
Shell最后运行的后台Process的PID   
$?   
最后运行的命令的结束代码（返回值）   
$-   
使用Set命令设定的Flag一览   
$*   
所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。   
$@   
所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。   
$#   
添加到Shell的参数个数   
$0   
Shell本身的文件名   
$1～$n   
添加到Shell的各参数值。$1是第1参数、$2是第2参数…。
```

