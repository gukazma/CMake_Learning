# C++标准的一般方法

# 介绍

以下是设置C++标准方法中，适用于大多数CMake版本的方法。

本教材的文件结构如下:

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 包含所要运行的CMake指令
  * main.cpp - 源文件

# 概述

## Checking compile flag(检查标志)

`CHECK_CXX_COMPILER_FLAG`函数会朝编译程序中传递函数的第一个参数，然后将结果保存在第二个参数重。

例如：

```cmake
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
```

`CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)`这条指令是使用`-std=c++11`这个标志来编译程序，然后将结果保存在`COMPILER_SUPPORTS_CXX11`变量中。

`include(CheckCXXCompilerFlag)`函数是告诉CMake将`CHECK_CXX_COMPILER_FLAG`这个函数包含，使其可用。

## 添加标志

一旦你确定了这个标志可用（上面的方法），就可以用标准的方法来朝目标指定标志。再本案例中，使用变量`CMAkE_CXX_FLAGS`来朝所有目标传递标志（这个变量会使得所有的编译后面都添加这个标志）。

```cmake
if(COMPILER_SUPPORTS_CXX11)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()
```

以上方法是检查从c++11到非标准c++0x的编译器标志，在实际使用中，你可能会检查c++14，或设置不同的编译器，如`-std=gun11`。

# 编译案例

以下是编译案例的输出

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
-- Performing Test COMPILER_SUPPORTS_CXX11
-- Performing Test COMPILER_SUPPORTS_CXX11 - Success
-- Performing Test COMPILER_SUPPORTS_CXX0X
-- Performing Test COMPILER_SUPPORTS_CXX0X - Success
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/i-common-method/build

$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/i-common-method -B/data/code/01-basic/L-cpp-standard/i-common-method/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
cd /data/code/01-basic/L-cpp-standard/i-common-method/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 1
[100%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++    -std=c++11   -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/i-common-method/main.cpp
Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++    -std=c++11    CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles  1
[100%] Built target hello_cpp11
make[1]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 0
```

