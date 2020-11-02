# 编译标记

# 介绍

CMake支持多种设置编译标记的方法

* 使用traget_compile_definitions()函数
* 使用 CMAKE_C_FLAGS 和 CMAKE_CXX_FLAGS 变量

案例文件目录：

```bash
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/G-compile-flags/CMakeLists.txt) - 包含了所有需要运行的CMake指令
- [main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/G-compile-flags/main.cpp) - main函数源码

# 概念

## 设置Per-Target（每个target）C++ 标记

推荐使用target_compile_definitions() 函数以下方式设置C++标记。这会为库填充[INTERFACE_COMPILE_DEFINITIONS](https://cmake.org/cmake/help/v3.0/prop_tgt/INTERFACE_COMPILE_DEFINITIONS.html#prop_tgt:INTERFACE_COMPILE_DEFINITIONS)，然后将定义推送到基于范围的链接目标

```cmake
target_compile_definitions(cmake_examples_compile_flags
    PRIVATE EX3
)
```

当目标是library时，且范围是PUBLIC or INTERFACE时，所选定义会被推送到所有链接这个库的可执行文件上。

对于target_compile_options()函数的详细参数[参考](https://cmake.org/cmake/help/v3.0/command/target_compile_options.html)

## 设置默认C++标记

默认的CMAKE_CXX_FLAGS 标签可能时空值或者包含构建类型的适当值

为了增加额外的默认值可以在CMakeLists.txt中添加以下命令：

```cmake
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DEX2" CACHE STRING "Set C++ Compiler Flags" FORCE)
```

类似的选项还包括：

- 设置C编译器使用 CMAKE_C_FLAGS
- 设置链接标记使用 CMAKE_LINKER_FLAGS.

> 注意：以上命令中 值 CACHE STRING "Set C++ Compiler Flags" FORCE时用来将变量设置到CMakeLists.txt文件中
>
> 更多详细内容 参考[这里](https://cmake.org/cmake/help/v3.0/command/set.html)

一旦设置了 CMAKE_C_FLAGS 和 CMAKE_CXX_FLAGS ，那么会在设置当前目录或者任何包含的字母的所有目标的编译器标签和定义。这个办法不推荐使用，更加推荐使用 target_compile_definitions函数

## 设置CMake 标记

类似于构建类型，C++宏定义可以使用以下方法设置

* 使用CMake-gui

