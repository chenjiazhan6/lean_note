# Shell

## 1. 入门程序

   - #!/bin/bash 开头,告知系统使用哪一个解释器来进行解析

   - 给程序加上可执行权限  chmod 755 文件名

   - 第一个函数echo:输出字符串

   - 程序示例

     ```shell
      #!/bin/bash                                                                              echo "hello world"
     ```

## 2. Shell变量

### 1. Shell基本变量

#### 1. 定义变量

（1）定义变量: 变量名=值，**注意中间不能有空格**

（2）撤销变量:  unset 变量名

（3）声明静态变量:  readonly 变量=值，不能使用unset来撤销该变量,并且这个变量是只读变量,是不能够写的

#### 2.变量定义规则

（1）变量名与值之间不能有空格

（2）一般变量名为大写

#### 3.命令返回值赋给命令

（1）A=`命令`：此时命令的返回值就赋给了A

（2）A=$(命令)

（3）A=$(其他变量)：将其他变量的值赋给另一个变量

#### 4.示例代码

Shell取值:$变量名,调用echo函数将值取出

```shell
 1 #!/bin/bash                                                                                                                                                
  2 A=100    #定义变量A
  3 echo $A   #取出变量A的值
  4 unset A   #撤销变量
  5 echo $A
  6 readonly A=99   #定义静态变量
  7 echo $A
  8 #unset A
  9 C=`ls -l`  //执行命令,将命令的返回值赋给变量C
 10 B=$(date)    
 11 D=$C      #将其他变量的值赋给另一个变量
 12 echo $D
 13 echo $B

```



### 2. 定义环境变量

#### 1.基本语法

- 环境变量需要在/etc/profile文件中设置

 - 语法: export 变量名=变量值
 - 刷新存储环境变量的文件:source /etc/profile

#### 2.代码示例

```shell
在/etc/prifile文件中编写该变量
export myenviroment=aaa 
```

在命令行执行: source /etc/profile,来刷新该文件

命令行输出环境变量的值

```shell
root@ubuntu:~/shell# echo $myenviroment                                                   aaa   
```

也可以在文件中直接使用该环境变量,与使用自身定义的变量相同



### 3. 位置参数变量

**可以在程序中使用命令行的参数信息**

#### 1.基本语法

（1) 索引为0-9的参数,可以直接 **$n** 取出对应的参数,10号以上的参数可以使用${n}取出值

（2）$*:将所有参数作为一个整体取出

（3）$@:将所有参数取出,但并非是作为一个整体,而是会被区分开来的

（4）$#:可以获取参数的个数

#### 2.示例程序

```shell
  1 #!/bin/bash
  2 echo $0 $1 $2  #$0:表示命令行的第一个参数,这里是./param.sh
  3 echo $*                                                                                                                                                                     
  4 echo $@
  5 echo "paramnumber=$#"

```

运行结果:

![](C:\Users\24960\Desktop\learn_note\shell\param.PNG)

### 4. 预定义变量

#### 1. 基本语法

1. $$:当前进程进程号

2. $!:最后一个进程运行的进程号
3. $?:最后一次执行命令是否成功,成功的话为0,不成功不为0

#### 2. 示例代码

```shell
  1. #!/bin/bash
  2 echo "current_pid=$$"
  3 ./param.sh 200 100  &                                                                                                                                                       
  4 echo "last_pid=$!"
  5 echo "issucceed=$?"

```

运行结果:

![](C:\Users\24960\Desktop\learn_note\shell\preparam.PNG)

#### 3. 后台运行程序

​	./param.sh &:在运行程序后方加上&,便是在后台运行程序



## 3. 算术运算

### 1. 基本语法

- $((运算式))
- $[运算式]
- expr 运算式的内容：运算符中间要有空格,并且是特殊字符,需要对其进行转译/*,运算式需要用反引号括起来

### 2. 示例代码

```shell
1 #!/bin/bash                                                                              2 result=$(( (2 + 3) * 4 ))                                                           
3 echo "result=$result"    # 第一种方式

5 second=$[(2 + 3) * 4]
6 echo "second=$second"  # 第二种方式
```

运行结果:

![](expr.png)





## 4.流程控制

### 1. 条件判断

条件语句两边需要空格

#### 1、if的基本语法:

```text
if [ command ];then
   符合该条件执行的语句
elif [ command ];then
   符合该条件执行的语句
else
   符合该条件执行的语句
fi
```



#### 2、文件/文件夹(目录)判断

```text
[ -b FILE ] 如果 FILE 存在且是一个块特殊文件则为真。
[ -c FILE ] 如果 FILE 存在且是一个字特殊文件则为真。
[ -d DIR ] 如果 FILE 存在且是一个目录则为真。
[ -e FILE ] 如果 FILE 存在则为真。
[ -f FILE ] 如果 FILE 存在且是一个普通文件则为真。
[ -g FILE ] 如果 FILE 存在且已经设置了SGID则为真。
[ -k FILE ] 如果 FILE 存在且已经设置了粘制位则为真。
[ -p FILE ] 如果 FILE 存在且是一个名字管道(F如果O)则为真。
[ -r FILE ] 如果 FILE 存在且是可读的则为真。
[ -s FILE ] 如果 FILE 存在且大小不为0则为真。
[ -t FD ] 如果文件描述符 FD 打开且指向一个终端则为真。
[ -u FILE ] 如果 FILE 存在且设置了SUID (set user ID)则为真。
[ -w FILE ] 如果 FILE存在且是可写的则为真。
[ -x FILE ] 如果 FILE 存在且是可执行的则为真。
[ -O FILE ] 如果 FILE 存在且属有效用户ID则为真。
[ -G FILE ] 如果 FILE 存在且属有效用户组则为真。
[ -L FILE ] 如果 FILE 存在且是一个符号连接则为真。
[ -N FILE ] 如果 FILE 存在 and has been mod如果ied since it was last read则为真。
[ -S FILE ] 如果 FILE 存在且是一个套接字则为真。
[ FILE1 -nt FILE2 ] 如果 FILE1 has been changed more recently than FILE2, or 如果 FILE1 exists and FILE2 does not则为真。
[ FILE1 -ot FILE2 ] 如果 FILE1 比 FILE2 要老, 或者 FILE2 存在且 FILE1 不存在则为真。
[ FILE1 -ef FILE2 ] 如果 FILE1 和 FILE2 指向相同的设备和节点号则为真。
```



#### 3、字符串判断

```text
[ -z STRING ] 如果STRING的长度为零则为真 ，即判断是否为空，空即是真；
[ -n STRING ] 如果STRING的长度非零则为真 ，即判断是否为非空，非空即是真；
[ STRING1 = STRING2 ] 如果两个字符串相同则为真 ；
[ STRING1 != STRING2 ] 如果字符串不相同则为真 ；
[ STRING1 ]　 如果字符串不为空则为真,与-n类似
```



#### 4、数值判断

```text
INT1 -eq INT2           INT1和INT2两数相等为真 ,=
INT1 -ne INT2           INT1和INT2两数不等为真 ,<>
INT1 -gt INT2            INT1大于INT1为真 ,>
INT1 -ge INT2           INT1大于等于INT2为真,>=
INT1 -lt INT2             INT1小于INT2为真 ,<</div>
INT1 -le INT2             INT1小于等于INT2为真,<=
```



#### 5、复杂逻辑判断

```text
-a 与
-o 或
! 非
```

#### 6. 代码示例

```shell
  1 #!/bin/bash
  2 if [ "aaa" = "aaa" ]
  3 then
  4     echo "equal"
  5 else
  6     echo "no equal"
  7 fi
  8 
  9 if [ -e /root/shell/expr.sh ]
 10 then echo "exitst"
 11 else
 12     echo "no exist"
 13 fi
 14 
 15 if [ 44 -gt 55 ]                                                                                                                                                            
 16 then
 17     echo "greater"
 18 else
 19     echo "less"
 20 fi

```

运行结果:

![](condition.png)



#### 7.说明

```text
[]和test

两者是一样的，在命令行里test expr和[ expr ]的效果相同。

test的三个基本作用是判断文件、判断字符串、判断整数。支持使用 ”与或非“ 将表达式连接起来。

test中可用的比较运算符只有==和!=，两者都是用于字符串比较的，不可用于整数比较，整数比较只能使用-eq, -gt这种形式。
无论是字符串比较还是整数比较都千万不要使用大于号小于号。当然，如果你实在想用也是可以的，对于字符串比较可以使用尖括号的转义形式， 如果比较"ab"和"bc"：[ ab \< bc ]，结果为真，也就是返回状态为0.

[[ ]]
这是内置在shell中的一个命令，它就比刚才说的test强大的多了。支持字符串的模式匹配（使用=~操作符时甚至支持shell的正则表达 式）。逻辑组合可以不使用test的-a,-o而使用&& ||。
字符串比较时可以把右边的作为一个模式（这是右边的字符串不加双引号的情况下。如果右边的字符串加了双引号，则认为是一个文本字符串。），而不仅仅是一个字符串，比如[[ hello == hell? ]]，结果为真。

注意：使用[]和[[]]的时候不要吝啬空格，每一项两边都要有空格，[[ 1 == 2 ]]的结果为“假”，但[[ 1==2 ]]的结果为“真”！

let和(())

两者也是一样的(或者说基本上是一样的，双括号比let稍弱一些)。主要进行算术运算(上面的两个都不行)，也比较适合进 行整数比较，可以直接使用熟悉的<,>等比较运算符。可以直接使用变量名如var而不需要$var这样的形式。支持分号隔开的多个表达式
```

### 2. case

#### 1. 基本语法

```text
case expr in                         #expr为表达式
parrern1)                             #若expr与pattern1匹配，注意括号
     commands1                   #执行语句块commands1
     ;;                                    #跳出case结构
parrern2)                             #若expr与pattern2匹配，注意括号
     commands2                   #执行语句块commands2
     ;;                                    #跳出case结构
    ''''                                    #可以有任意多个模式匹配块
*)                                        #若expr与上面的模式都不匹配
    commands                      #执行语句块commands
    ;;
esac                                    #case语句必须以esac终止
```



#### 2. 说明

1、表达式expr按照顺序匹配每个模式，一旦有一个模式匹配成功，则执行该模式后面所有命令，然后退出case.

2、如果expr没有找到匹配模式，则执行默认值“*）”后面的命令块（类似于if语句中的else块）；“*）”块可以不出现。

3、所给的匹配模式“pattrer?"中可以含有通配符和逻辑或“|”，

4、除非特殊需要，否则每个命令块的最后必须有一个双分号";;"，可以独占一行，或者放在语句块最后一个命令的后面。



#### 3. 代码示例

```shell
 1 #!/bin/bash
  2 case $1 in
  3 "aaa")
  4     echo "params is aaa"  # 普通模式的匹配
  5     ;;
  6 "bbb")
  7     echo "param is bbb"
  8     ;;
  9 "ccc" | "ddd")   #含有 | 逻辑运算符的匹配
 10     echo "param is ccc | ddd"
 11     ;; 
 12 "abc"*)     # 匹配符的匹配
 13     echo "param is begin with abc"
 14     ;;
 15 *)
 16     echo "no pattern"
 17     ;;
 18 esac                                        
```

运行结果:

![](case.png)

### 3. for

#### 1. 基本语法

**in后面的参数可以是程序运行的一组数据或者是声明的一组数据**

```text
for i in 的各种用法 ：

for i in “file1” “file2” “file3”  #将值依次赋给变量i
for i in /boot/*            #将该目录下的目录或者文件名打印出来
for i in /etc/*.conf       # 将该目录下的以conf结尾的文件名打印出来
for i in $(seq -w 10)    # seq命令生成一串连续的数字
for i in {1..10}    
for i in $( ls )    # 从ls运行的结果赋值到变量中
for I in $(< file)
for i in “$@”  #从命令行中取出参数

for i in 数据来源
do
	程序代码
done
```

#### 2. seq命令的用法

```text
  seq [选项]... 尾数
　或：seq [选项]... 首数 尾数
　或：seq [选项]... 首数 增量 尾数
以指定增量从首数开始打印数字到尾数。
  -f, --format=格式 使用printf 样式的浮点格式
  -s, --separator=字符串使用指定字符串分隔数字(默认使用：\n)
  -w, --equal-width 在列前添加0 使得宽度相同
      --help 显示此帮助信息并退出
      --version 显示版本信息并退出
如果省略了首数或者增量，则默认其值为1，即使这样尾数仍小于首数。
首数、增量和尾数均以浮点数形式解释。当首数小于尾数时增量一般为正值，
相反在首数大于尾数时增量一般为负数。
指定的格式必须适用于显示"double"类型的参数；当首数、增量和尾数均为指定
精确度的定点十进制数时默认为"%.精确度f"，否则默认为"%g"。
```

3. 代码示例

   ```shell
     1 #!/bin/bash
     2 for i in $(seq 0 10)
     3 do
     4     echo $i
     5 done     
   ```

   运行结果

   ![](fortest.png)



### 5. while

#### 1. 基本语法

```text
while [ condition ]
do 
	程序代码
done
```



#### 2. 代码示例

```shell
  1 #!/bin/bash                                                                                                                                                                 
  2 a=0
  3 sum=0
  4 while [ $a -le 100 ]
  5 do
  6     let sum=sum+$a
  7     let a++
  8 done
  9 echo $sum
 10 echo $a

```

运行结果:

![](whiletest.png)

**let可以直接进行算术运算,而无需使用其他的语法,这个let语法比较方便**

## 5. read:读取控制台输入

### 1. read命令的参数

| -p        | 指定要显示的提示                                       |
| --------- | ------------------------------------------------------ |
| -s        | 静默输入，一般用于密码                                 |
| -n #      | 指定输入的字符长度最大值#                              |
| -d ‘字符’ | 输入结束符，当你输入的内容出现这个字符时，立即结束输入 |
| -t N      | 超出N秒没有进行输入，则自动退出。                      |

### 2. 代码参考

##### (1)基本输入

```shell
  1 #!/bin/bash
  2 echo "please enter your name:"
  3 read name
  4 echo "hello $name"       
```

运行结果:

![](read_simple.png)

##### (2)使用上面所说参数

```shell
 1 #!/bin/bash
 
 # 不使用参数
  2 echo "please enter your name:"
  3 read name
  4 echo "hello $name"
  5 
  6 echo "=============="
  # 使用参数 -p:显示提示信息
  8 echo "please enter your name:"
  9 read -p "name:"  name
 10 echo "hello $name"
 11 
 12 echo "==============="
 #使用参数-s:不显示内容,一般用于密码
 14 echo "please enter your password"
 15 read -s -p "password" password
 16 echo "your password is : $password"
 #使用参数-d,遇到某个字符马上结束输入
 18 echo "=============="
 19 echo "please enter your name:"
 20 read -d 'h' -p "char is h" name
 21 echo "enter name is $name"
 22 
 23 echo "==============="
 #在规定时间内没有输入自动结束
 25 echo "please enter your name"
 26 read -p "name:" -t 20  name
 27 echo $name                        
```



运行结果:



![使用参数后产生的结果](read_full.png)

## 6. 函数

### 1.系统函数

#### (1)基本语法

- baseName:提取出完整路径中的文件名

  ```shell
  basename [pathname][suffix]:加上后缀的话会将文件的后缀名去掉
  ```

  

- dirname:提取出完整路径中的目录名

  ```shell
  basename [pathname]
  ```

  

#### (2)代码示例

```shell
  1 #!/bin/bash
  2 filename=$(basename /root/shell/bbb.txt  .pdf)
  3 echo $filename
  4 dirname=$(dirname /root/shell/bbb.txt)
  5 echo $dirname                   
```

运行结果:

![](sys_function.png)

### 2. 自定义函数

#### 	(1)基本语法

```text
[function] funname(){
	
	action;
	[return int;]
}
定义函数是无需写入形参,可以在函数中直接取出
当函数有返回值,可以直接使用$?将值取出
```

#### （2)代码示例

```shell
1 #!/bin/bash
  2 function getsum(){
  3 
  4 sum=0
  5 i=$1
  6 while [ $i -le $2 ]                                                                                                                                                         
  7 do
  8     let sum=sum+$i
  9     let i++
 10 done
 11 
 12 echo $sum
 13 return 0
 14 
 15 }
 16 getsum 0 100

```

运行结果:



![](myfunction.png)





```shell
获取参数返回值的示例；
funWithReturn(){
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $?"
```



## 7.语法注意事项

- 定义变量时中间不能有空格
- $((运算式)),$[运算式]这两种方式计算运算式时,运算符之间需要有空格
- let表达式来计算是,运算式中间不能有空格,必须是一个整体,可以使用“”括起来,也可以不括
- 条件语句都是使用[]括起,并且[]与condition直接需要有空格隔开
- 当要使用值时,必须使用$符取出参数的值