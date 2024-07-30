## 作业
其中`$@`表示单个输入,以空格分隔,`$*`表示所有的
![[Pasted image 20240529172709.png]]
使用管道运算符，管道运算符能够把前面的输出和后面的输入连接起来
![[Pasted image 20240529174620.png]]
使用ssh登录![[Pasted image 20240529192206.png]]
- 编写一个shell脚本，直到输入“quit”或学号时会输出提示信息并退出，否则将持续要求用户输入。
```shell
#!/bin/bash

while true; do
    read -p "请输入内容" input
	echo "$input"
    
    # 检查输入是否为 quit 或学号
    if [[ "$input" == "quit" || "$input" =~ ^[0-9]{8}$ ]]; then
        break
    fi
done

```
- 试用脚本实现如下功能：（1）后台计数功能（1-1000计数）；（2）通过输入参数可以控制将计数的信息显示或不显示；（3）可以查询当前的计数状况。
```shell
#!/bin/bash
show_count_info=true
count=0
fifo_file="count_fifo"
if [[ ! -p $fifo_file ]]; 
	then mkfifo "$fifo_file" 
fi

while [[ $# -gt 0 ]]; do
    case "$1" in
        --hide-info)
            show_count_info=false
            shift
            ;;
        --show-info)
            show_count_info=true
            shift
            ;;
        *)
            echo "未知的选项：$1"
            exit 1
            ;;
    esac
done

while true; do
    ((count++))
    echo "$count" > "$fifo_file"
    if [[ $show_count_info == true ]]; then
        echo "当前计数：$count"
    fi
    sleep 1
done
```

```shell
#!/bin/bash

fifo_file="count_fifo"

if [[ ! -p $fifo_file ]]; then
    echo "管道文件 $fifo_file 不存在"
    exit 1
fi

# 循环读取管道中的内容
while true; do
    if read -r count < "$fifo_file"; then
        echo "接收到的计数：$count"
    else
        echo "无法从管道中读取计数"
        exit 1
    fi
done

```

- 对于给定的如下脚本，请尝试使用不同的重定向命令，会有什么结果。
```shell
#!/bin/bash  
# filename: redirect.sh  
# redirection examples  
cat << self-defined-eof  
=======================  
some info  
blabla  
=======================  
self-defined-eof  
cd redirect.sh
```
![[Pasted image 20240529194748.png]]
- 请将当前的实验环境中使用的远程服务器，其连接方式修改为密钥登录，并将其端口修改为其他非占用端口（如1205），将其超时验证修改为合理的数值，以规避过于频繁的断连现象。
修改`/etc/ssh/sshd_config`中的port为1205，并且修改
```shell
PasswordAuthentication no 
PubkeyAuthentication yes 
ClientAliveInterval 60
ClientAliveCountMax 3 
```
