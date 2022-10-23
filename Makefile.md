# 基本规则

* makefile是一个文件，其中记载了目标可执行文件的生成方式。**为什么需要makefile呢？**这是因为对于大型代码工程文件，会由很多模块的源码组成，而各模块的代码间存在依赖关系，通过makefile可以指定各文件间**（包括.c .o .exe文件）**的依赖关系**（编译的先后顺序）**，并最终生成可执行目标文件。
* makefile具有强大功能的原因是，其中可以执行shell命令，这样便可以执行操作系统命令。
* makefile需要通过make命令来执行，**make是一个命令行工具，用以解析makefile中所规定的编译规则并执行编译相关命令。**make工具的具体实现是与操作系统或者IDE相关的（Delphi的make，Visual C++的nmake，Linux下GNU的make）。
* 程序编译链接相关知识
  * 源代码文件（.c）->目标文件（UNIX下是.o；windows下是.obj）->可执行文件（windows下是.exe）。**通常，一个源文件会对应一个同名的目标文件**
  * 这里第一步叫**“编译compile”**，第二步叫**“链接link”**。
  * 对于大量的中间代码文件，我们通常会打个包，生成所谓库文件（在Windows下叫“库文件”（Library File），也就是 `.lib` 文件，在UNIX下，是Archive File，也就是 `.a` 文件）。

**makefile规则**

```
target ... : prerequisites ...
    command
    ...
    ...
```
* target
  1. 可以是一个object file（即中间代码文件.obj）
  2. 也可以是一个执行文件（至少需要有一个写在文件首行的可执行文件）
  3. 还可以是一个标签（clean就是标签）

* prerequisites

  生成该target所依赖的文件和/或target

* command

  该target要执行的命令（任意的shell命令）



下面结合规则看两个简单的makefile文件

* 生成main.exe（依赖两个源文件）

```makefile
main:main.o my.o              
	gcc main.o my.o -o main
main.o:main.c
	gcc -c main.c
my.o:my.c
	gcc -c my.c 
.PHONY:clean

#linux 下用 rm -rf *.o main
clean:
	@echo "=======clean project========="
	del  *.o 
	@echo "=======clean completed========="
```
* 生成edit.exe文件（依赖三个头文件和八个源文件）
```makefile
edit : main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
    cc -o edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o

main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
```

* 从上得出的基础规则
  * 在定义好依赖关系后，后续的那一行定义了如何生成目标文件的操作系统命令，行首一定要以一个 `Tab` 键作为缩进。
  *  `clean` 作为target，但并不是一个文件，而是像label一样，其冒号后什么也没有，那么，make就不会自动去找它的依赖性，也就不会自动执行其后所定义的命令。要执行其后的命令，就要在make命令后明显得指出这个label的名字（例如在命令行中执行make clean）。这样就可以在makefile中定义不用的编译或是和编译无关的shell命令，比如程序的打包，程序的备份、这里的清除clean，等等。
  * `\`是换行连接符；`#`是注释
  * 

**变量**

变量的定义和使用

* `objects = main.o kbd.o command.o display.o  insert.o search.o files.o utils.o`  定义变量，简单的赋值语法

* `$(objects)` 使用变量，$()





