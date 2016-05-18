---
layout: post
title:  "Shell语法"
date:   2016-05-18
categories: base
author: "Chyler"
---

>为了给我的LNMP写启动脚本，现在来跟<a href="http://vbird.dic.ksu.edu.tw/linux_basic/0320bash.php">鸟哥</a>学习shell script

**script执行方式**

1. 直接执行（使用路径直接执行或使用bash/sh执行），会使用新的bash环境来执行脚本内的命令，其实实在子进程的bash内执行的，当子进程完成后，子进程的各项变量或操作将会结束不会传回到父进程中。

2. source执行，会在父进程中执行。

**声明**

```
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
```
声明“\#!/bin/bash”，当执行该程序时，能够加载bash的相关环境配置文件，并执行hash来是下面的命令能够执行。

**用户输入名字 输出**

```
read -p "first name:" firstname #等待输入
read -p "last name:" lastname
echo -e "full name: $firstname $lastname" #输出
```

**按日期创建文件**

```
read -p "input your filename:" fileuser
#检测是否输入文件名
filename=${fileuser:-"filename"} #fileuser为空则为“filename”

#利用date命令获取需要的文件名
#注意=$空格不能随便加
date1=$(date --date='2 days ago' +%Y%m%d)
date2=$(date --date='1 days ago' +%Y%m%d)
date3=$(date +%Y%m%d)
file1=${filename}${date1}
file2=${filename}${date2}
file3=${filename}${date3}

touch "$file1"
touch "$file2"
touch "$file3"
```
**数值加减乘除**

```
#echo $((运算内容)) 
```
**test命令的测试功能**

```
#test -z 判断字符串是否为空
test -z $filename && echo "you must input a filename" && exit 0

#test -e 判断文件是否存在，存在返回true ！返回反向状态
test ! -e $filename && echo "the filename '$filename' do not exist" && exit 0

#test -f 判断文件是否存在且为文件
test -f $filename && filetype="regular file"

#test -d 判断文件是否存在且为目录
test -d $filename && filetype="directory"

echo $filetype
```

**判断符号[]**
```
read -p "please input (Y/N): " yn
[ "$yn" == "Y" -o "$yn" == "y" ] && echo "OK" && exit 0
[ "$yn" == "N" -o "$yn" == "n" ] && echo "NO" && exit 0
echo "neither y/n"
```

**默认变量**

```
#sh test.sh a b c
echo "$0" #脚本名 test.sh
echo "$#" #参数个数 3
[ "$#" -lt 2 ] && echo "parameter is less than 2" && exit 0
echo "$@" #参数的全部内容 'a b c'
echo "$1" #第一个参数 a
echo "$2" #第二个参数 b
```

**变量shift**
```
echo "$#" #7
echo "$@" #a b c d e f g
shift #一个变量的shift
echo "$#" #6 
echo "$@" #b c d e f g 
shift 3 #3个变量的shift
echo "$#" #3
echo "$@" #e f g
```

**if...**

```
read -p "please input (Y/N): " yn
if [ "$yn" == "Y" ] || [ "$yn" == "y" ]; then
	echo "OK"
	exit 0
fi
if [ "$yn" == "N" ] || [ "$yn" == "n" ]; then
	echo "NO"
	exit 0
fi
echo "neither y/n"

```

**if...elif...else**

```
read -p "please input (Y/N): " yn
if [ "$yn" == "Y" ] || [ "$yn" == "y" ]; then
	echo "OK"
	exit 0
elif [ "$yn" == "N" ] || [ "$yn" == "n" ]; then
	echo "NO"
	exit 0
else
	echo "neither y/n"
fi
```
**检测端口**

```
testing = $ (netstat -tuln | grep ":80")
if [ "$testing" != ""]; then
	echo "WWW is running in your system."
fi
```

**case**

```
case $1 in
 "hello")
	 echo "Hello"
	 ;;
 "")
	 echo "empty"
	 ;;
 *)
	 echo "Usage $0 {hello}"
	 ;;
esac
```
**case choice**

```
#./sh07.sh one
case $1 in
 "one" )
	echo "your choice is ONE"
	;;
 "two" )
	echo "your choice is TWO"
	;;
 "three" )
	echo "your choice is THREE"
	;;
 * )
	echo "Usage $0 {one|two|three}"
	;;
esac

```
**function**

```
function printit () {
	echo -n "Your choice is "  #-n不换行显示
}
case $1 in
 "one")
	printit; echo $1 | tr 'a-z' 'A-Z' #小写 转 大写
	;;
 "two" )
	printit; echo $1 | tr 'a-z' 'A-Z'
	;;
 "three" )
	printit; echo $1 | tr 'a-z' 'A-Z'
	;;
 * )
	echo "Usage $0 {one|two|three}"
	;;
esac

```
**while do done**

```
while [ "$yn" != "yes" -a "$yn" != "YES" ]
do
	read -p "please input yes/YES to stop this program" yn
done
echo "ok"
```
**for**

```
users=$(cut -d ':' -f1 /etc/passwd)
for username in $users
do
	id $username
done

```
**网路检测**

```
network="192.168.1"
for sitenu in $(seq 1 100)
do
	ping -c 1 -w ${network}.${sitenu} &> /dev/null && result=0 || result=1
	if [ "$result" == 0 ]; then
		echo "Server ${network}.${sitenu} is UP."
	else
		echo "Server ${network}.${sitenu} is DOWN."
	fi
done
```


