# 介绍

当使用CMake时，可以设定C和C++编译器。这个案例将会介绍如何简单的设置c或c++编译器。

本教程的目录结构如下：

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 包含你想运行的CMake命令。
  * main.cpp - 一个简单cpp程序文件。

# 概述

### 编译器选择

CMake会暴露选项来控制用于编译或者链接你的代码的程序。这些程序包括。

  * CMAKE_C_COMPILER -用于编译C的程序。
  * CMAKE_CXX_COMPILER - 用于编译C++的程序。
  * CMAKE_LINKER - 用于链接你的二进制文件的程序。

Note：在本案例中clang-3.6通过`sudo apt-get install clang-3.6`安装。

Note：这是最简单的调用clang的方法。未来会展示更好的方法。

调用Clang的方法
====


### Setting Flags

As described in the link:../F-build-type[Build Type] example, you can set CMake options
using either a cmake gui or by passing from the command line.

Below is an example of passing the compiler via the command line.

```bash
cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
```

设置这些选项后，当你运行`make`时，会使用clang来编译你的二进制文件。以下的make输出将会看到。

```bash
/usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
Linking CXX executable hello_cmake
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1

/usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic
```



# 编译案例

以下是编译案例的输出

```bash
$ mkdir build.clang

$ cd build.clang/

$ cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
-- The C compiler identification is Clang 3.6.0
-- The CXX compiler identification is Clang 3.6.0
-- Check for working C compiler: /usr/bin/clang-3.6
-- Check for working C compiler: /usr/bin/clang-3.6 -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/clang++-3.6
-- Check for working CXX compiler: /usr/bin/clang++-3.6 -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang

$ make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang -B/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
cd /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake --color=
Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
Scanning dependencies of target hello_cmake
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 1
[100%] Building CXX object CMakeFiles/hello_cmake.dir/main.cpp.o
/usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
Linking CXX executable hello_cmake
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1
/usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles  1
[100%] Built target hello_cmake
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 0

$ ./hello_cmake

Hello CMake!
```

