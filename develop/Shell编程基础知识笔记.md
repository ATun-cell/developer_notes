## 什么是Shell
Shell编程就是对一堆Linux命令的逻辑化处理

### [Shell 编程的 Hello World](#shell-编程的-hello-world)

学习任何一门编程语言第一件事就是输出 HelloWorld 了！下面我会从新建文件到 shell 代码编写来说下 Shell 编程如何输出 Hello World。

(1)新建一个文件 [helloworld.sh](http://helloworld.sh) :`touch helloworld.sh`，扩展名为 sh（sh 代表 Shell）（扩展名并不影响脚本执行，见名知意就好，如果你用 php 写 shell 脚本，扩展名就用 php 好了）

(2) 使脚本具有执行权限：`chmod +x helloworld.sh`

(3) 使用 vim 命令修改 [helloworld.sh](http://helloworld.sh) 文件：`vim helloworld.sh`(vim 文件------>进入文件----->命令模式------>按 i 进入编辑模式----->编辑文件 ------->按 Esc 进入底行模式----->输入:wq/q! （输入 wq 代表写入内容并退出，即保存；输入 q!代表强制退出不保存。）)

[helloworld.sh](http://helloworld.sh) 内容如下：

```
#!/bin/bash
#第一个shell小程序,echo 是linux中的输出命令。
echo  "helloworld!"
```
shell 中 # 符号表示注释。**shell 的第一行比较特殊，一般都会以#!开始来指定使用的 shell 类型。在 linux 中，除了 bash shell 以外，还有很多版本的 shell， 例如 zsh、dash 等等...不过 bash shell 还是我们使用最多的。**

(4) 运行脚本:`./helloworld.sh` 。（注意，一定要写成 `./helloworld.sh` ，而不是 `helloworld.sh` ，运行其它二进制的程序也一样，直接写 `helloworld.sh` ，linux 系统会去 PATH 里寻找有没有叫 [helloworld.sh](http://helloworld.sh) 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 `helloworld.sh` 是会找不到命令的，要用`./helloworld.sh` 告诉系统说，就在当前目录找。）
## [Shell 变量](#shell-变量)

### [Shell 编程中的变量介绍](#shell-编程中的变量介绍)

**Shell 编程中一般分为三种变量：**

1. **我们自己定义的变量（自定义变量）:** 仅在当前 Shell 实例中有效，其他 Shell 启动的程序不能访问局部变量。
2. **Linux 已定义的环境变量**（环境变量， 例如：`PATH`, ​`HOME` 等..., 这类变量我们可以直接使用），使用 `env` 命令可以查看所有的环境变量，而 set 命令既可以查看环境变量也可以查看自定义变量。
3. **Shell 变量**：Shell 变量是由 Shell 程序设置的特殊变量。Shell 变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了 Shell 的正常运行

**常用的环境变量:**

> PATH 决定了 shell 将到哪些目录中寻找命令或程序  
> HOME 当前用户主目录  
> HISTSIZE 　历史记录数  
> LOGNAME 当前用户的登录名  
> HOSTNAME 　指主机的名称  
> SHELL 当前用户 Shell 类型  
> LANGUAGE 　语言相关的环境变量，多语言可以修改此环境变量  
> MAIL 　当前用户的邮件存放目录  
> PS1 　基本提示符，对于 root 用户是#，对于普通用户是$


**使用 Linux 已定义的环境变量：**

比如我们要看当前用户目录可以使用：`echo $HOME`命令；如果我们要看当前用户 Shell 类型 可以使用`echo $SHELL`命令。可以看出，使用方法非常简单。

**Shell 编程中的变量名的命名的注意事项：**

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头，但是可以使用下划线（_）开头。
- 中间不能有空格，可以使用下划线（_）。
- 不能使用标点符号。
- 不能使用 bash 里的关键字（可用 help 命令查看保留关键字）。

### [Shell 字符串入门](#shell-字符串入门)

字符串可以用单引号，也可以用双引号。这点和 Java 中有所不同。

在单引号中所有的特殊符号，如和反引号都没有特殊含义。在双引号中，除了"$"、"\"、反引号和感叹号（需开启 `history expansion`），其他的字符没有特殊含义

