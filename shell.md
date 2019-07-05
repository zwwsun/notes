# shell

- 一般情况下, 不区分 Bourne Shell 和 Bourne Again Shell, #!/bin/sh 同样也可以改为 #!/bin/bash。

## 变量

- 变量命名格式类似js变量
- your_name="runoob"，变量名和等号之间不能有空格
- 定义变量时，变量名不加美元符号（$，PHP语言中变量需要）
- 只有使用变量时需要加$,  $name或者${name}。  第二次赋值的时候也不能写$your_name="alibaba"，只有使用时才能写。
- readonly myUrl，readonly将变量myUrl变为只读,值不能改变，不能删除
- unset variable_name，删除变量，但不能删除只读变量，变量名不加$

```shell
    for file in `ls /etc`
    或
    for file in $(ls /etc)
    # 用语句给变量赋值，此处将/etc下目录的文件名循环出来
```

### 变量类型

- 局部变量：局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- 环境变量：所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- shell变量：shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## 字符串

- 字符串可以用单引号，也可以用双引号，也可以不用引号

### 单引号

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单引号（对单引号使用转义符后也不行）。

### 双引号

- 双引号里可以有变量
- 双引号里可以出现转义字符

```shell
    your_name="qinjx"
    greeting="hello, "$your_name" !"
    greeting_1="hello, ${your_name} !"
    # 都行
    # 反引号里不能用${}取值写法，只能用 `  "$变量名"  `
```

### 获取字符串长度

```shell
    string="abcd"
    echo ${#string}
    # 4
```

### 提取子字符串

- 序号从0开始

```shell
    string="runoob is a great site"
    echo ${string:1:4}
    # unoo
```

### 查找字符串

- 序号从1开始

```shell
    string="runoob is a great company"
    echo `expr index "$string" run`
    # 输出1
    # 查找字符 "run" 的位置
```

## shell数组

- 只支持一维数组，不限定大小
- 用括号来表示数组，数组元素用"空格"符号分割开：数组名=(值1 值2 ... 值n)

```shell
    array_name[0]=value0
    # 定义分量
```

### 读取数组 ${数组名[下标]}

- valuen=${array_name[n]}
- echo ${array_name} 默认输出第一个元素
- @符号可以获取数组中所有的元素 ${array_name[@]}  或者  ${array_name[*]}
- echo ${!array_name[@]} 输出数组所有元素

### 获取数组的长度

- 取得整个数组的长度 length=${#array_name[@]} 或者 length=${#array_name[*]}
- 取得数组单个元素的长度 lengthn=${#array_name[n]}

## shell注释

- #（一个或多个）开头 + 空格 + 注释     shell只有单行注释，每行加#
- 多行注释可以用花括号把要注释的代码括起来，定义成函数，只要没有地方调用，函数就不会执行，相当于注释

```shell
    # 多行注释的另外格式
    :<<EOF
    注释内容...
    注释内容...
    注释内容...
    EOF

    # EOF可以使用其他符号 会出警告
    :<<'
    注释内容...
    注释内容...
    注释内容...
    '

    :<<!
    注释内容...
    注释内容...
    注释内容...
    !
```

## shell 传递参数

- 执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n， n 代表一个第几个参数。 $0为执行的文件名
  - $#  传递到脚本的参数个数
  - `$*`以一个单字符串显示所有向脚本传递的参数。 如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
  - `$@` 与`$*`相同，但是使用时加引号，并在引号中返回每个参数。如`"$@"`用「"」括起来的情况、以`"$1" "$2" … "$n"`的形式输出所有参数。假设在脚本运行时写了三个参数 1、2、3, 则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）
  - `$$`  脚本运行的当前进程ID号
  - `$!`  后台运行的最后一个进程的ID号
  - `$-`  显示Shell使用的当前选项，与set命令功能相同。
  - `$?`  显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

## shell 基本运算符

### 算术运算符

- 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用
- val=`expr 2 + 2`  得使用反引号，且数值与运算符之间需要有空格： 2 + 2 要有空格，不能2+2
- 条件表达式要放在方括号之间，并且要有空格 if [ $a == $b ]
- 运算符 `*` 乘号前一定要加斜杠转义 `\*`

```shell
    # 条件语句
    if [ $a != $b ]
    then
        echo "a 不等于 b"
    fi
```

- 在MAC中shell 的 expr 语法是：$((表达式))，此处表达式中的 "*" 不需要转义符号 "\" 。
- 算术运算符： + - * / % = == !=  （后三位为 赋值 相等 不等）

### 关系运算符

- 关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
- -eq  检测两个数是否相等，相等返回 true。 [ $a -eq $b ] 返回 false。
- -ne  检测两个数是否不相等，不相等返回 true。
- -gt  检测左边的数是否大于右边的，如果是，则返回 true。
- -lt  检测左边的数是否小于右边的，如果是，则返回 true。
- -ge  检测左边的数是否大于等于右边的，如果是，则返回 true。
- -le  检测左边的数是否小于等于右边的，如果是，则返回 true。

### 布尔运算符

- ！  非运算，表达式为 true 则返回 false，否则返回 true。 [ ! false ] 返回 true。
- -o  或运算，有一个表达式为 true 则返回 true。 [ $a -lt 20 -o $b -gt 100 ] 返回 true。
- -a  与运算，两个表达式都为 true 才返回 true。 [ $a -lt 20 -a $b -gt 100 ] 返回 false。

### 逻辑运算符

- &&  逻辑的 AND
- ||  逻辑的 OR

### 字符串运算符

- =   检测两个字符串是否相等，相等返回 true。
- !=  检测两个字符串是否相等，不相等返回 true。
- -z  检测字符串长度是否为0，为0返回 true。    [ -z $a ] 返回 false
- -n  检测字符串长度是否为0，不为0返回 true。  [ -n "$a" ] 返回 true。
- str 检测字符串是否为空，不为空返回 true。    [ $a ] 返回 true。

### 文件测试运算符

- -b file  检测文件是否是块设备文件，如果是，则返回 true。   [ -b $file ] 返回 false。
- -c file  检测文件是否是字符设备文件，如果是，则返回 true。
- -d file  检测文件是否是目录，如果是，则返回 true。
- -f file  检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。  
- -g file  检测文件是否设置了 SGID 位，如果是，则返回 true。  
- -u file  检测文件是否设置了 SUID 位，如果是，则返回 true。  
- -k file  检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。
- -p file  检测文件是否是有名管道，如果是，则返回 true。  
- -r file  检测文件是否可读，如果是，则返回 true。
- -w file  检测文件是否可写，如果是，则返回 true。
- -x file  检测文件是否可执行，如果是，则返回 true。
- -s file  检测文件是否为空（文件大小是否大于0），不为空返回 true。
- -e file  检测文件（包括目录）是否存在，如果是，则返回 true。

## echo 命令

- echo "It is a test" 与  echo It is a test 相同，可省略双引号
- echo "\"It is a test\"" 显示转义字符，结果是 "It is a test"
- 显示换行

```shell
    # -e 开启转义  \n换行符
    echo -e "OK! \n"
    echo "It it a test"
    # OK!
    # It it a test

    # -e 开启转义 \c 不换行
    echo -e "OK! \c"
    echo "It is a test"
    # OK! It is a test
```

- 显示结果定向至文件  echo "It is a test" > myfile
- 原样输出字符串，不进行转义或取变量(用单引号)

```shell
    echo '$name\"'
    $name\"
```

- 显示命令执行结果

```shell
    echo `date`
    # Thu Jul 24 10:08:46 CST 2014
```

## 输出字符串总结

```shell
            能否引用变量  能否引用转义符  能否引用文本格式（换行符、制表符等等）
    单引号      -              -                    -
    双引号      能             能                   能
    无引号      能             能                   -
```

## read命令

- 一个一个词组地接收输入的参数，每个词组需要使用空格进行分隔；如果输入的词组个数大于需要的参数个数，则多出的词组将被作为整体为最后一个参数接收
- -p 输入提示文字   -n 输入字符长度限制(达到最大，自动结束)  -t 输入限时  -s 隐藏输入内容

```shell
    read -p "请输入一段文字:" -n 6 -t 5 -s password
    echo -e "\npassword is $password"
    # $ sh test.sh
    #  请输入一段文字:
    #  password is asdfgh
```

## printf 命令

- printf  format-string  [arguments...]   format-string: 为格式控制字符串，arguments: 为参数列表。

```shell
    printf "Hello, Shell\n"
    printf "Hello, Shell2\n"
    # 两个printf不会自动换行，得需要加\n

    # format-string为双引号或者单引号，效果一样
    printf "%d %s\n" 1 "abc"   #  1 abc

    # format-string为无引号，效果一样，但不能引用文本格式，会当成转义符
    printf %s\n abcdef #  nabcdef

    # 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
    printf %s abc def  # abcdef
    printf "%s\n" abc def
    #  abc
    #  def
    printf "%s %s\n" a b c d
    #  ab
    #  cd
    # 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
    printf "%s and %d \n"  #     and 0

    printf "a string, no processing:<%s>\n" "A\nB"
    #  a string, no processing:<A\nB>

    printf "a string, no processing:<%b>\n" "A\nB"
    # a string, no processing:<A
    # B>

    $ printf "www.runoob.com \a"
    # www.runoob.com $  
```

- %d %s %c %f 格式替代符
  - d: Decimal 十进制整数 -- 对应位置参数必须是十进制整数，否则报错！
  - s: String 字符串 -- 对应位置参数必须是字符串或者字符型，否则报错！
  - c: Char 字符 -- 对应位置参数必须是字符串或者字符型，否则报错！
  - f: Float 浮点 -- 对应位置参数必须是数字型，否则报错！

```shell
    printf "%d %s %c\n" 1 "abc" "def"
    # 1 abc d
    # 其中最后一个参数是 "def"，%c 自动截取字符串的第一个字符作为结果输出。
```

## test命令

- test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

### 数值测试

- -eq 等于则为真  num1=100  num2=100    if test $[num1] -eq $[num2]
- -ne 不等于则为真
- -gt 大于则为真
- -ge 大于等于则为真
- -lt 小于则为真
- -le 小于等于则为真

```shell
    #  代码中的 [] 执行基本的算数运算
    a=5
    b=6
    result=$[a+b]
```

### 字符串测试

- = 等于则为真
- != 不相等则为真
- -z 字符串的长度为零则为真
- -n 字符串的长度不为零则为真

### 文件测试

- -e 文件名    文件存在则为真  if test -e ./bash
- -r 文件名    文件存在且可读则为真
- -w 文件名    文件存在且可写则为真
- -x 文件名    文件存在且可执行则为真
- -s 文件名    文件存在且至少有一个字符则为真
- -d 文件名    文件存在且为目录则为真
- -f 文件名    文件存在且为普通文件则为真
- -c 文件名    文件存在且为字符型特殊文件则为真

```shell
    #  逻辑操作符 非( ! )、并( -a )、或( -o )
    if test -e ./notFile -o -e ./bash
```

## 流程控制

- sh的流程控制不可为空，比如if判断语句里，如果else分支没有语句执行，就不要写这个else。

```shell
    if condition
    then
        command1
        command2
        ...
        commandN
    fi

    #  终端写成一行
    if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

- if else-if else

```shell
    if condition1
    then
        command1
    elif condition2
    then
        command2
    else
        commandN
    fi
```

- for循环

```shell
    for name in item1 item2 ... itemN
    do
        command1
        command2
        ...
        commandN
    done

    #  for name in item1 item2 ... itemN; do command1; command2…; done;
    #  in列表可以包含替换、字符串和文件名。in列表是可选的，如果不用它，for循环使用命令行的位置参数。

    for loop in 1 2; do echo "The value is: $loop"; done;
    #  The value is: 1
    #  The value is: 2

    for str in 'This is a string'; do echo $str; done;
    #  This is a string
```

- while循环

```shell
    while condition
    do
        command
    done


    int=1
    while [ $int<=3 ]
    do
        echo $int
        let "int++"
    done
    #  1
    #  2
    #  3
    #  let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量
```

- until 循环

```shell
    #  执行一系列命令直至条件为 true 时停止
    until condition
    do
        command
    done
```