# 正点原子阿尔法Linux开发板学习笔记

## 说明

**本库仅供学习交流使用**

- **芯片型号**: I.MX6ULL
- **开发环境**: Ubantu16.04
- **编译器版本**: gcc V5.4.0
- **参考视频**: 【非常全的Linux嵌入式教学视频(正点原子阿尔法ARM开发板，IMX6ULL)共83讲】 https://www.bilibili.com/video/BV1DK411P775/?p=4&share_source=copy_web&vd_source=0db47c15b9f51dbaa4548ec2dc55dea4

## Linux C编程

### gcc 编译器

gcc text.c -o out 

- text.c: 源文件名
- out: 输出可执行文件名

### make和Makefile

**Makefile基础**

```tex
目标: 依赖文件集合...
    命令1
    命令2
```

**每条命令前要加TAB, 不能加空格**

举例: main需要main.c, input.c, calcu.c三个文件生成

```makefile
main: main.o input.o calcu.o
    gcc -o main main.o input.o calcu.o
main.o: main.c
    gcc -c main.c
input.o: input.c
    gcc -c input.c
calcu.o: calcu.c
    gcc -c calcu.c

clean:
    rm *.o
    rm main
```

使用make工具, make命令可通过Makefile的规则进行编译, make clean命令可执行Makefile中clean部分的命令

```shell
make
make clean
```

## Makefile 语法

### Makefile 规则格式

```tex
目标...: 依赖文件集合...
    命令1
    命令2
    ...
```

### Makefile 变量

将main.o input.o calcu.o设置为变量objects, 并引用变量objects

- 设置变量: 使用=进行变量的定义
- 引用变量: 使用$(变量名)进行引用

```makefile
# Makefile 的变量使用
objects = main.o input.o calcu.o
main: $(objects)
    gcc -o main $(objects)
```
#### 赋值符=的使用

*在使用=给变量赋值时,变量的值为最后一次赋值的值*

```makefile
name = a
curname = $(name)
name = b

print:
    @echo curname: $(curname)
```

输入命令

```shell
make print
```

最后的输出结果为: curname: b

- @echo: 不显示命令本身, 只显示命令的结果
- echo: 显示命令本身和命令的结果

#### 赋值符:=的使用

*在使用:=给变量赋值时,变量的值为第一次赋值的值*

```makefile
name = a
curname := $(name)
name = b

print:
    @echo curname: $(curname)
```

输入命令

```shell
make print
```

最后的输出结果为: curname: a

#### 赋值符?=的使用

*在使用?=给变量赋值时, 如果变量前面没有赋值, 则变量的值为?=后面的值, 如果变量前面有赋值, 则变量的值不变*

```makefile
name = a
curname ?= $(name)
name = b

print:
    @echo curname: $(curname)
```

输入命令

```bash
make print
```

最后的输出结果为: curname: a

#### 变量追加+=

*给前面定义好的变量追加字符串*

```makefile
objects = main.o input.o
objects += calcu.o
```

objects为main.o input.o calcu.o

### Makefile 模式规则

使用符号%表示模式, **%表示任意长度的字符串**

*%.c是指所有的.c文件*

```makefile
objects = main.o input.o calcu.o
main: $(objects)
    gcc -o main $(objects)

%.o: %.c
    #命令
```

### Makefile 自动化变量

| 自动化变量 | 描述 |
| --- | --- |
| $@ | 表示规则中的目标文件集合 |
