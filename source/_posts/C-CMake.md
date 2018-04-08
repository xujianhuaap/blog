---
title: C_CMake
date: 2018-03-12 20:37:06
tags: c_cmake
---
1. CMake 
```
CMake 创建一个本地构建环境,这个环境可以编译源码，生成libary,并且以组合的方式生成可执行文件
CMake 被设计成可以构建目录复杂的工程.
```
2. CMakeList.txt
```
CMakeList.txt 是配置文件
例子如下
a> cmakeList.txt
cmake_minimum_required(VERSION 2.6)
project (Tutorial)#工程名
set (Tutorial_VERSION_MAJOR 1)# 设置工程主版本号
set (Turorial_VERSION_MINOR 0)# 设置工程次版本号

# 配置文件
configure_file( "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"
                "${PROJECT_BINARY_DIR}/TutorialConfig.h")
# 设置.h文件所在的根目录
include_directories("${PROJECT_BINARY_DIR})
#添加新的依赖库
include_directories("${PROJECT_SOUCE_DIR}/MathFuctions")
# 调用MathFuctions目录下的CMakeLists.txt
add_subdirectory(MathFuctions)
# 生成一个Tutorial的可执行文件
add_executable( Tutorial tutorial.cpp)
# 动态链接 MathFuctions
target_link_libraries(Tutorial MathFuctions)
# 安装与测试
b> TutorialConfig.h.in
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
c> /MathFutions/CMakeLists.txt
# 生成一个静态库   
add_library(MathFuction mysqrt.cpp)
```
