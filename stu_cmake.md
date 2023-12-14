# cmake快速入门
## 1. cmake是什么？
cmake是一种跨平台的编译工具。项目通常包括源代码和构建系统，构建系统决定如何构建源代码。构建系统通常用于生成可执行文件，库，文档，以及安装包。  

源文件要生成可执行文件，需要调用系统中的toolchain，进行如下流程：**预处理**、**编译**（转汇编语言）、**汇编**（转机器语言）、**链接**（将多个目标文件合并成一个可执行文件）。  

当项目中的文件较少时，我们可以通过输入命令的方式执行上述过程，但是，当项目文件较多时，我们不可能手动输入命令，而是需要编写一个脚本文件（makefile），让脚本文件自动执行上述过程。  

makefile的编写并不总是那么轻松，因此需要更加上层的构建系统cmake，**通过编写cmake的脚本文件cmakeLists.txt，cmake会自动生成makefile，然后通过make命令执行上述过程**。


## 2. 如何编写简单的cmakeLists.txt？
* 指定cmake最低版本
```
cmake_minimum_required(VERSION 3.10)
```
* 指定工程名称
```
project(test)
```
* 指定源文件
```
add_executable(test main.cpp)
```
* 指定头文件
```
include_directories([头文件所在的目录])
```

包括了以上四个部分，就已经可以用户通过cmakeLists.txt生成makefile了。


## 2.set的使用
* set(变量名 值)  
  set可以将一些值绑定成一个自定义变量，如果需要同时编译多个源文件，正常情况下需要在add_executable()中指定多个源文件，但是，如果使用set，就可以将多个源文件绑定成一个自定义变量，然后通过add_executable()指定这个自定义变量。   
  eg: 
  ```cmake
  set(SRC_LIST main.cpp test.cpp) #这条语句要写在前面  

  add_executable(test ${SRC_LIST})
  ```

* set指定c++版本   
  例如auto是在c++11中出现的，如果需要使用auto，需要指定c++版本为c++11，可以通过set指定c++版本
  ```cmake
  set(CMAKE_CXX_STANDARD 11)
  ```

## 3.自动查找多个源文件
* aux_source_directory() 
  ~~~cmake
  aux_source_directory(${PROJECT_SOURCE_DIR}//src src_list) 
  ~~~
  PROJECT_SOURCE_DIR表示cmakelists.txt所在的目录，aux_source_directory()会自动查找${PROJECT_SOURCE_DIR}/src目录下的所有源文件，并将源文件地址存入src_list中

* file()
  ~~~cmake
  file( GLOB src_list ${CMAKE_CURRENT_SOURCE_DIR}//src//*.cpp) 
  ~~~
  file()会自动查找${CMAKE_CURRENT_SOURCE_DIR}/src目录下的所有源文件，并将源 文件地址存入src_list中,区别在于这种方式需要指定查找文件的后缀。