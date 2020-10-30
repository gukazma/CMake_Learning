# 介绍

```bash
A-hello-cmake$ tree
.
├── CMakeLists.txt
├── main.cpp
```

* CMakeList.txt - 包含了你希望执行的CMake命令
* main.cpp - 一个简单的cpp文件

# 概念

## 最低CMake版本

在创建一个使用CMake的工程时，你可以指定CMake支持的最低版本

```cmake
cmake_minimum_required(VERSION 3.5)
```

## 多项目

CMake构建可以设置一个项目名称，以便在当使用多个项目时，应用某些特定的变量。

```cmake
project (hello_cmake)
```

## 创建一个可执行文件

add_executable()命令可以指定从指定源码中生成的可执行文件，第一个参数时可执行文件的名称，第二个参数时可执行文件需要编译的源码

```cmake
add_executable(hello_cmake main.cpp)
```

> 注意：使用${PROJECT_NAME}变量可以构建与项目名相同的可执行文件。
>
> ```cmake
> cmake_minimum_required(VERSION 2.6)
> project (hello_cmake)
> add_executable(${PROJECT_NAME} main.cpp)
> ```

## 二进制目录

你执行cmake命令的目录就是被称为CMAKE_BINARY_DIR的目录，这个目录保存所有的二进制文件。CMake支持i-place和out-of-source的两种构建二进制文件的方式。

### In-Place 生成（同源码目录生成）

In-Place生成会在源码目录下生成所有的临时文件。这就意味着所有的Makefiles文件和object文件都会和你的源码参杂在一起。在你的根目录（CMakeList.txt目录）下运行cmake命令，即可进行In-place生成。例如：

```bash
A-hello-cmake$ cmake .
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake

A-hello-cmake$ tree
.
├── CMakeCache.txt
├── CMakeFiles
│   ├── 2.8.12.2
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── a.out
│   │   │   └── CMakeCCompilerId.c
│   │   └── CompilerIdCXX
│   │       ├── a.out
│   │       └── CMakeCXXCompilerId.cpp
│   ├── cmake.check_cache
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── hello_cmake.dir
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── DependInfo.cmake
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   └── progress.make
│   ├── Makefile2
│   ├── Makefile.cmake
│   ├── progress.marks
│   └── TargetDirectories.txt
├── cmake_install.cmake
├── CMakeLists.txt
├── main.cpp
├── Makefile
```

### Out-of-Source 生成（异源码目录生成）

Out-of-Source生成允许你在你的文件系统的任意位置创建生成文件夹。所有的临时文件和object文件都会在这个文件夹内，保持你的源码文件加清洁。在你的生成文件夹内执行cmake命令，并指定CMakeList.txt文件所在的目录就可以进行out-of-source生成。你想要重写创建CMake生成文件，只需要重写允许cmake命令即可。

例如：

```bash
A-hello-cmake$ mkdir build

A-hello-cmake$ cd build/

A-hello-cmake/build$ make ..
make: Nothing to be done for `..'.
matrim@freyr:~/workspace/cmake-examples/01-basic/A-hello-cmake/build$ cmake ..
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake/build

A-hello-cmake/build$ cd ..

A-hello-cmake$ tree
.
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 2.8.12.2
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── a.out
│   │   │   │   └── CMakeCCompilerId.c
│   │   │   └── CompilerIdCXX
│   │   │       ├── a.out
│   │   │       └── CMakeCXXCompilerId.cpp
│   │   ├── cmake.check_cache
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── hello_cmake.dir
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   └── progress.make
│   │   ├── Makefile2
│   │   ├── Makefile.cmake
│   │   ├── progress.marks
│   │   └── TargetDirectories.txt
│   ├── cmake_install.cmake
│   └── Makefile
├── CMakeLists.txt
├── main.cpp
```

> 本教程将全程使用out-of-source生成

# 生成例子

以下是简单的生成例子

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
-- Build files have been written to: /workspace/cmake-examples/01-basic/hello_cmake/build

$ make
Scanning dependencies of target hello_cmake
[100%] Building CXX object CMakeFiles/hello_cmake.dir/hello_cmake.cpp.o
Linking CXX executable hello_cmake
[100%] Built target hello_cmake

$ ./hello_cmake
Hello CMake!
```



