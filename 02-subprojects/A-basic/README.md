# 介绍

这个案例将会告诉你如何设置一个带有子项目的CMake项目，顶级的CMakeLists.txt会调用创建在以下目录下的CMakeLists.txt文件：

* sublibrary1 - 一个静态库
* sublibrary2 - 一个头文件库
* subbinary - 可执行文件

文件结构如下：

```bash
$ tree
.
├── CMakeLists.txt
├── subbinary
│   ├── CMakeLists.txt
│   └── main.cpp
├── sublibrary1
│   ├── CMakeLists.txt
│   ├── include
│   │   └── sublib1
│   │       └── sublib1.h
│   └── src
│       └── sublib1.cpp
└── sublibrary2
    ├── CMakeLists.txt
    └── include
        └── sublib2
            └── sublib2.h
```

  * CMakeLists.txt - 顶级CMakeLists.txt
  * subbinary/CMakeLists.txt - 用来生成可执行文件
  * subbinary/main.cpp - 可执行文件源码
  * sublibrary1/CMakeLists.txt - 用来生成静态库
  * sublibrary1/include/sublib1/sublib1.h
  * sublibrary1/src/sublib1.cpp
  * link:sublibrary2/CMakeLists.txt - 用力设置头文件库
  * link:sublibrary2/include/sublib2/sublib2

> Tip : 这个案例将每个子项目都include目录放在各自都目录下，而不是放在总到根目录下。这可以很好的防止名字冲突，
>
> 你将不得不在文件中包含头如下：
>
> ```c++
> #include "sublib1/sublib1.h"
> ```
>
> 意味着，如果你install你的项目，那么include的目录会自动install在usr/local/include/sublib1下。

# 概述

## 添加一个子目录

一个CMakeLists.txt会包含（包含这个子目录的代码）和调用这些子目录的CMakeLists.txt文件命令。

```cmake
add_subdirectory(sublibrary1)
add_subdirectory(sublibrary2)
add_subdirectory(subbinary)
```

## 引用子项目目录

当一个项目使用project()函数创建时，CMake会自动创建一系列变量，以引用项目的详细相关信息。这些信息可以被子项目或者主项目使用。例如你可以利用如下方式，引用不同项目的源码目录：

```cmake
${sublibrary1_SOURCE_DIR}
${sublibrary2_SOURCE_DIR}
```

CMake创建的的变量包括：

| 变量               | 介绍                                                         |
| ------------------ | ------------------------------------------------------------ |
| PROJECT_NAME       | 当前项目的名字                                               |
| CMAKE_PROJECT_NAME | 第一个调用project()函数的项目的名字，例如顶级的项目名。      |
| PROJECT_SOURCE_DIR | 当前项目CMakeLists.txt文件所在目录                           |
| PROJECT_BINARY_DIR | 当前项目源码所在目录                                         |
| name_SOURCE_DIR    | "name"这个项目的CMakeLists.txt文件所在的目录，例如sublibrary1_SOURCE_DIR就是sublibrary1这个项目CMakeLists.txt文件所在的目录 |
| name_BINARY_DIR    | "name"这个项目的二进制文件所在的目录，例如sublibrary1_BINARY_DIR就是sublibrary1这个项目二进制文件所在的目录 |

## 单头文件库

如果你创建的是单头文件库，cmake支持INTERFACE目标，以允许一个目标没有任何构建输出。

```cmake
add_library(${PROJECT_NAME} INTERFACE)
```

当你创建你的库时，可以指定这个库目标的include目录的域为`INTERFACE`，当目标的域被设置成`INTERFACE`后，所有链接这个库的目标都会包含这个目录，但你创建的库在编译时不会编译这个include库。

```cmake
target_include_directories(${PROJECT_NAME}
    INTERFACE
        ${PROJECT_SOURCE_DIR}/include
)
```

## 从子项目引用库

如果一个子项目创建了一个库时，其他工程可以通过`target_link_libraries()`函数来引用这个库。这意味着你不需要再重新创建一个新库目标，而是以依赖的方式引用这个库。

```cmake
target_link_libraries(subbinary
    PUBLIC
        sublibrary1
)
```

你可以使用如下的只读格式的别名引用库：

创建库别名：

```cmake
add_library(sublibrary2)
add_library(sub::lib2 ALIAS sublibrary2)
```

以如下方式，通过别名引用库：

```cmake
target_link_libraries(subbinary
    sub::lib2
)
```

## 从子项目的include目录

从子项目中添加库时，不需要额外添加子项目库的include目录。

通过`target_include_directories()`命令指定库的include目录的域。例如，案例中，子二进制可执行文件会链接sublibrary1和sublibrary2两个库，同时也会包含两个库的include目录，因为两个库的域是`PUBLIC`和`INTERFACE`。

# 构建案例

构建的输出：

```bash
$ mkdir build

$ cd build/

$ cmake ..
-- The C compiler identification is GNU 4.8.4
-- The CXX compiler identification is GNU 4.8.4
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/02-sub-projects/A-basic/build

$ make
Scanning dependencies of target sublibrary1
[ 50%] Building CXX object sublibrary1/CMakeFiles/sublibrary1.dir/src/sublib1.cpp.o
Linking CXX static library libsublibrary1.a
[ 50%] Built target sublibrary1
Scanning dependencies of target subbinary
[100%] Building CXX object subbinary/CMakeFiles/subbinary.dir/main.cpp.o
Linking CXX executable subbinary
[100%] Built target subbinary
```

