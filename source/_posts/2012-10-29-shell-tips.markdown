---
layout: post
title: "shell tips"
date: 2012-10-29 14:57
comments: true
categories: [tip]
---

# shell(bash)

* 脚本所在的路径

        SCRIPTDIR="$( cd -P "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

* 生成随机密码

        cat /dev/urandom|tr -dc "a-zA-Z0-9-_."|fold -w 12 | head -n 1

* 取一个 ( (变量名是变量) 的变量) 的值，嗯有点绕

        varname='abc'
        # 相当于 value=$abc
        value=${!varname}

* 取字符串长度

        length_of_str_var=${#str_var}

* substring

        b=${a:12:5}

* 取后缀名

        ~% FILE="example.tar.gz"
        ~% echo "${FILE%%.*}"
        example
        ~% echo "${FILE%.*}"
        example.tar
        ~% echo "${FILE#*.}"
        tar.gz
        ~% echo "${FILE##*.}"
        gz

* 輸出一個多行的字符串，將多行的字符串賦值給變量

        cat <<EOF
        mkdir -p $dest && unzip -q -o -d $dest \$1
        echo "OK"
        EOF

        vvv=$(cat <<EOF
        mkdir -p $dest && unzip -q -o -d $dest \$1
        echo "OK"
        EOF)

* 使用find命令时自动忽略指定的目录（[参考](http://stackoverflow.com/a/4210072)）

        # 查找当前目录下所有的lua文件，并且忽略 znum, tests, .git 目录
        find .  -type d \( -name znum -o -name tests -o -name .git \) -prune  -o -iname '*.lua' -print

* 将一个命令的输出重定向到`while read line`循环

    有时候需要逐行处理一个命令的输出，因为每行输出里面有空格，所以不能用下面的for循环

        for i in $( grep 'require' a.lua )
        do
            ...
        done

    有一种方式是
        
        some command > a.tmp
        while read line 
        do
            ...
        done < a.tmp
        rm a.tmp

    这样会产生一个临时文件，总归不太爽，直接使用重定向的写法如下（[参考](http://fvue.nl/wiki/Bash:_Piped_%60while-read'_loop_starts_subshell)），需要将命令用括号包起来构成一个sub shell：

        while read line
        do
            ...
        done < <( grep 'require' a.lua ) # 注意两个箭头之间有空格

