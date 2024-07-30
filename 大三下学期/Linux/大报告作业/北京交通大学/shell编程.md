## 课程
- 使用ssh链接![[Pasted image 20240529151421.png]]
- 打印helloworld![[Pasted image 20240529151446.png]]
- 查看用户名![[Pasted image 20240529151528.png]]
- 使用man命令![[Pasted image 20240529151618.png]]
- 查看文件![[Pasted image 20240529151804.png]]
- 查看详细文件![[Pasted image 20240529151838.png]]
- 创建目录:![[Pasted image 20240529151935.png]]
## 练习
- less是一个类似于more的程序，但它允许在文件中向后移动以及向前移动。此外，less不需要在启动前读取整个输入文件，因此对于大输入文件，它比vim这样的文本编辑器启动得更快。
- 我的目录结构为
```shell
年->月->日->光照_酸碱_菌落情况->图片
						|->csv
						|->说明
```
- 在已有上述文件目录结构的情况下，如何快速的生成本年所需的记录结果的文件夹？如果假如从3月20日~4月15日的数据丢失了，如何模拟出这种效果？
可以写一个shell脚本自动创建
```shell
#!/bin/bash

current_year=2024

days_in_month=(31 28 31 30 31 30 31 31 30 31 30 31)

is_leap_year() {
  year=$1
  if (( (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 )); then
    return 0
  else
    return 1
  fi
}

if is_leap_year $current_year; then
  days_in_month[1]=29
fi

for month in $(seq -w 1 12); do
  days=${days_in_month[$((10#$month - 1))]} # 使用10#来避免前导零问题
  for day in $(seq -w 1 $days); do
    base_path="${current_year}/${month}/${day}/光照_酸碱_菌落情况"
    mkdir -p "${base_path}/图片"
    mkdir -p "${base_path}/csv"
    mkdir -p "${base_path}/说明"
  done
done
```
如果3.20~4.15的缺失了，我么可以添加条件，让目录跳过这段日期
```shell
#!/bin/bash

current_year=2024

days_in_month=(31 28 31 30 31 30 31 31 30 31 30 31)

is_leap_year() {
  year=$1
  if (( (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 )); then
    return 0
  else
    return 1
  fi
}

if is_leap_year $current_year; then
  days_in_month[1]=29
fi

for month in $(seq -w 1 12); do
  days=${days_in_month[$((10#$month - 1))]}
  for day in $(seq -w 1 $days); do
    # 跳过3月20日到4月15日
    if [[ "$month" == "03" && "$day" -ge 20 ]] || [[ "$month" == "04" && "$day" -le 15 ]]; then
      continue
    fi
    base_path="${current_year}/${month}/${day}/光照_酸碱_菌落情况"
    mkdir -p "${base_path}/图片"
    mkdir -p "${base_path}/csv"
    mkdir -p "${base_path}/说明"
  done
done
```