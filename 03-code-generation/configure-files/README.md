# 配置文件生成

# 介绍

在cmake被调用时，可以使用任何来自CMakeLists.txt和cmake缓存的的变量。CMake生成的文件被复制到新位置并且任何变量能够被替换。

文件的结构如下：

```bash
$ tree
.
├── CMakeLists.txt
├── main.cpp
├── path.h.in
├── ver.h.in
```

 * CMakeLists.txt - 包含所有的CMake指令
  * main.cpp - main函数源文件
  * path.h.in - 包含path指定到build目录到变量的文件
  * ver.h.in - 包含项目版本的ver变量的文件

# 概述

## 配置文件

你可以使用`configure_file`函数进行一个文件的变量代换。这个函数的两个核心参数是，源文件和目标文件。

```cmake
configure_file(ver.h.in ${PROJECT_BINARY_DIR}/ver.h)

configure_file(path.h.in ${PROJECT_BINARY_DIR}/path.h @ONLY)
```

第一个调用的生成文件允许使用`${}`或者`@@`这两种方式，调用这个函数会在`PROJECT_BINARY_DIR`目录中生成一个新的ver.h文件。

```cmake
const char* ver = "${cf_example_VERSION}";
```



第二个调用只允许`@@`这种调用方式，并且会在`PROJECT_BINARY_DIR`目录中生成一个新的path.h文件。

```cmake
const char* path = "@CMAKE_SOURCE_DIR@";
```

# 构建案例

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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/03-code-generation/configure-files/build

$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  Makefile  path.h  ver.h

$ cat path.h
#ifndef __PATH_H__
#define __PATH_H__

// version variable that will be substituted by cmake
// This shows an example using the @ variable type
const char* path = "/home/matrim/workspace/cmake-examples/03-code-generation/configure-files";

#endif

$ cat ver.h
#ifndef __VER_H__
#define __VER_H__

// version variable that will be substituted by cmake
// This shows an example using the $ variable type
const char* ver = "0.2.1";

#endif

$ make
Scanning dependencies of target cf_example
[100%] Building CXX object CMakeFiles/cf_example.dir/main.cpp.o
Linking CXX executable cf_example
[100%] Built target cf_example

$ ./cf_example
Hello Version 0.2.1!
Path is /home/matrim/workspace/cmake-examples/03-code-generation/configure-files
```

