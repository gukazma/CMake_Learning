# 使用变量设置c++标准

# 介绍

以下方法是使用设置CMAKE_CXX_STANDARD变量的方法来设置C++标准。适用于CMake v3.1以上的方法。

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

使用`CMAKE_CXX_STANDARD`会让`CXX_STANDARD`属性作用在所有目标上，引起CMake在编译时，使用最适合的标志。

**NOTE:** `CMAKE_CXX_STANDARD`会将标准设置为最接近的版本，例如当设置`-std=gun11`编译器不支持时，可能会设置成`-std=gun0x。`

这个方法一般不会报错。

# 构建输出

以下是构建时的输出：

```bash
$ mkdir build
$ cd build


$ cmake ..
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build

$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/ii-cxx-standard -B/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
cd /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/ii-cxx-standard /data/code/01-basic/L-cpp-standard/ii-cxx-standard /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build /data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
[ 50%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++     -std=gnu++11 -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/ii-cxx-standard/main.cpp
[100%] Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++      CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
[100%] Built target hello_cpp11
make[1]: Leaving directory '/data/code/01-basic/L-cpp-standard/ii-cxx-standard/build'
```



