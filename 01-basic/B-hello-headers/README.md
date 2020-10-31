# Hello Headers

# 介绍

展示了一个使用源码文件和头文件在不同目录的案例。

这个教程文件包括：

```bash
B-hello-headers$ tree
.
├── CMakeLists.txt
├── include
│   └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp

```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/B-hello-headers/CMakeLists.txt) - 包含了你希望运行的CMake命令。
- [include/Hello.h](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/B-hello-headers/include/Hello.h) - 头文件。
- [src/Hello.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/B-hello-headers/src/Hello.cpp) - 需要编译的源文件。
- [src/main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/B-hello-headers/src/main.cpp) - main函数源文件。

# 概念

## 目录路径

CMake语法指定了许多变量，这些变量可以用来帮助在项目或源树中找到有用的目录。其中包括：

| 变量                     | 含义                                                         |
| ------------------------ | ------------------------------------------------------------ |
| CMAKE_SOURCE_DIR         | 源码根目录（CMakeList.txt所在目录）                          |
| CMAKE_CURRENT_SOURCE_DIR | 存在多个文件目录或者子项目时的当前源码根目录（最顶级的根目录） |
| PROJECT_SOURCE_DIR       | 当前项目的根目录（应该是子项目的根目录）                     |
| CMAKE_BINARY_DIR         | 你运行cmake文件的二进制临时文件所在目录                      |
| CMAKE_CURRENT_BINARY_DIR | 当前所在的二进制目录                                         |
| PROJECT_BINARY_DIR       | 当前项目的二进制目录                                         |

## 源码文件变量

创建一个包含多个源码文件的变量，方便再其他命令中传参。

```cmake
# Create a sources variable with a link to all cpp files to compile
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})
```

> 注意：使用通配符模式匹配的方式来查找并赋值变量(GLOB命令)。
>
> ```cmake
> file(GLOB SOURCES "src/*.cpp")
> ```
>
> 提示：再现代CMake中，不推荐使用变量来表示源码。而是直接再add_xxx 函数总直接申明。如果你再项目中添加了一个新文件，那么glob命令可能不能显示正确的结果。

## Including目录（头文件目录）

当你右多个不同的头文件文件夹，你可以使用target_include_directories()函数来使得编译器能够识别到这些头文件。当编译这些目标时，会使用  -I  标识符 将这些头文件目录添加到编译器，例如 -I/dircetory/path

```cmake
target_include_directories(target
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)
```

PRIVATE标识符指定include的范围。着对于库很重要，会再下一个案例中进行解释。更多细节点击[这里](https://cmake.org/cmake/help/v3.0/command/target_include_directories.html)

# 构建案例

## 标准输出

构建案例的标准输出如下：

```bash
$ mkdir build

$ cd build

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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build

$ make
Scanning dependencies of target hello_headers
[ 50%] Building CXX object CMakeFiles/hello_headers.dir/src/Hello.cpp.o
[100%] Building CXX object CMakeFiles/hello_headers.dir/src/main.cpp.o
Linking CXX executable hello_headers
[100%] Built target hello_headers

$ ./hello_headers
Hello Headers!
```

## 详细输出

在前面的案例中，运行make命令知识展示构建的状态。为了debug可以添加VERBOSE=1 标识符来查看完整的output。

纤细的输出如下，输出的检查展示了include目录确实是被添加到了c++编译器命令中：

```bash
$ make clean

$ make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/hello_headers -B/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
cd /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/hello_headers /home/matrim/workspace/cmake-examples/01-basic/hello_headers /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles/hello_headers.dir/DependInfo.cmake --color=
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
make -f CMakeFiles/hello_headers.dir/build.make CMakeFiles/hello_headers.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 1
[ 50%] Building CXX object CMakeFiles/hello_headers.dir/src/Hello.cpp.o
/usr/bin/c++    -I/home/matrim/workspace/cmake-examples/01-basic/hello_headers/include    -o CMakeFiles/hello_headers.dir/src/Hello.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/hello_headers/src/Hello.cpp
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 2
[100%] Building CXX object CMakeFiles/hello_headers.dir/src/main.cpp.o
/usr/bin/c++    -I/home/matrim/workspace/cmake-examples/01-basic/hello_headers/include    -o CMakeFiles/hello_headers.dir/src/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/hello_headers/src/main.cpp
Linking CXX executable hello_headers
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_headers.dir/link.txt --verbose=1
/usr/bin/c++       CMakeFiles/hello_headers.dir/src/Hello.cpp.o CMakeFiles/hello_headers.dir/src/main.cpp.o  -o hello_headers -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles  1 2
[100%] Built target hello_headers
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/hello_headers/build'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/hello_headers/build/CMakeFiles 0
```



