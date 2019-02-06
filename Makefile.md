# Makefile Skills

## 1.Makefile文件的规则：
target ... : prerequisites ... </br>
&emsp;&emsp;command </br>
&emsp;&emsp;... </br>
&emsp;&emsp;... </br>
`注意：command前的空白符必须是严格的制表符，这是语法所严格规定的` </br>
这里的target是一个目标文件，是这个步骤所需要生成的。（但也不全是如此）  
而prerequisites是一个依赖列表，生成target的过程需要问文件。  
command就是要执行的命令，可以是任意的shell命令。  
`当prerequisites中有一个或者多个文件比target文件要新的话，才会执行command的命令`  
</br>


## 2.C/C++程序的制定文件编译：
```makefile
main: main.o xxx.o xxx.o ......
  g++ -o main main.o xxx.o xxx.o ......
```
如上的部分可以通过链接已经编译好的各个*.o文件来链接生成可执行文件main。  
```makefile
xxx.o: xxx.h xxx.cpp
  g++ -c xxx.h xxx.cpp
```
如上的部分可以将对应需要的头文件和源文件合并编译，生成对应的*.o文件。  
`虽然可以列出.h文件，但是事实上，如果只给出.cpp，g++也会根据#include中的文件列表链接相应的.h文件`  
```makefile
clean:
  rm main main.o xxx.o xxx.o ......
```
如上的部分是对应指令`make clean`，通过shell指令删除编译生成的文件。  
`但是需要注意，gcc/g++编译器会对.h头文件进行预处理，生成同名的.h.gch文件，用于快速编译，最好在clean中也对这个文件进行删除`  
</br>


## 3.Makefile中使用变量：
如，可以用一个变量表示所有的*.o文件：  
```makefile
object = main.o xxx.o xxx.o \
  xxx.o ......
```
其中，符号`\`是一个延续换行符，即使视觉上换行，但是逻辑上仍然属于一行的内容。  
之后可以使用已经定义的变量进行Makefile相关的语句进行编写：  
```makefile
main: $(object)
  g++ -o main $(object)
  
clean: 
  rm main $(object)
```
如上通过`$(变量)`的方式来使用已经定义的变量。  
`另外，需要注意，在makefile中的变量其实类似于C/C++中的宏，所以不会展开，而是带入所有的使用变量的地方`  
</br>


## 4.对于清空文件的规则叙述：
如上所述，在使用变量的情况下可以使用如下的语句进行清空：
```makefile
clean:
  rm main $(objects)
```
但是更稳健的做法是：
```makefile
.PHONY : clean
clean :
  -rm main $(objects)
```
这里的`.PHONY`表示`clean`是一个“伪目标”。  
而在`rm`命令前面加一个减号的意思是，如果某些文件出现问题，也不用管，而继续执行。  
`clean命令不要放在makefile文件的开头，否则会成为默认的目标`  
一般大家都习惯将clean指令放到makefile文件的最后，是一个不成文的规矩。  
</br>


## 5.在Makefile中使用通配符（类似于正则表达）：
1. `~`在文件名中有特殊的用途，与`cd ~`的意义类似，是当前用户的`home`目录。  
2. `*`如同正则表达式中的作用，是一个通配符，如`*.cpp`代表所有的cpp文件。  
如最常见的：
```makefile
clean:
  rm -f *.o
```
但是需要注意如果进行如下的表示：
```makefile
objects = *.o
```
其中的`*.o`不会展开。  
如果你需要让通配符在变量中展开，也就是让objects的值是所有的.o文件的集合，那你需要：
```makefile
objects := $(wildcard *.o)
```
</br>

类似的方法，可以得到一个确定的文件夹中的所有.cpp文件如下：
```makefile
objects := $(wildcard *.c)
```
对于上述的所有.cpp文件，我们可以找出对应的.o文件，方法如下：
```makefile
$(patsubst %.cpp,%.o,$(wildcard *.cpp))
```
通过如上的铺垫，可以写出编译并且链接所有.cpp和.o文件的指令如下：  
```makefile
objects := $(patsubst %.cpp,%.o,$(wildcard *.cpp))
main : $(objects)
  g++ -o main $(objects)
```
对于上述的`wildcard`和`patsubst`两个属于Makefile的关键词，具体的部分叙述可以在如下链接中看到一部分：   
* [函数关键词的部分描述](https://seisman.github.io/how-to-write-makefile/functions.html "关键词描述")  
</br>





* [参考链接](https://seisman.github.io/how-to-write-makefile/introduction.html#id1 "Makefile介绍")  
