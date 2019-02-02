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

## 2.










*[参考链接](https://seisman.github.io/how-to-write-makefile/introduction.html#id1 "Makefile介绍")  
