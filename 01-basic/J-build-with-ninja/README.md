# 介绍

CMake是一个元构建系统，其可以被用来创建各种其他构建工具的构建文件。这个案例将会展示如何使CMake来使用ninja构建工具。

教程的文件构成如下：

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 包含你所有想运行的CMake指令。
  * main.cpp - 一个简单的Helloworldcpp文件

# 概念

### 生成器

CMake 生成器负责将输入文件交给构建系统。执行`cmake --help`将会显示可用的生成器。对于cmake v2.8.12.2支持的生成器如下：

```bash
Generators

The following generators are available on this platform:
  Unix Makefiles              = Generates standard UNIX makefiles.
  Ninja                       = Generates build.ninja files (experimental).
  CodeBlocks - Ninja          = Generates CodeBlocks project files.
  CodeBlocks - Unix Makefiles = Generates CodeBlocks project files.
  Eclipse CDT4 - Ninja        = Generates Eclipse CDT 4.0 project files.
  Eclipse CDT4 - Unix Makefiles
                              = Generates Eclipse CDT 4.0 project files.
  KDevelop3                   = Generates KDevelop 3 project files.
  KDevelop3 - Unix Makefiles  = Generates KDevelop 3 project files.
  Sublime Text 2 - Ninja      = Generates Sublime Text 2 project files.
  Sublime Text 2 - Unix Makefiles = Generates Sublime Text 2 project files.Generators
```

CMake包含不同生成器，包括命令行、IDE和额外的生成器。

#### 命令行生成器

这些命令行生成器类似于Make和Ninja，必须在CMake生成构建系统前制定生成器工具链。

支持的生成器包括：

  * Borland Makefiles
  * MSYS Makefiles
  * MinGW Makefiles
  * NMake Makefiles
  * NMake Makefiles JOM
  * Ninja
  * Unix Makefiles
  * Watcom WMake

#### IDE构建生成器

集成开发环境生成器包含他们自己的编译器。例如Visual Studio和Xcode都有自己原生的编译器。

支持的生成器如下：

  * Visual Studio 6
  * Visual Studio 7
  * Visual Studio 7 .NET 2003
  * Visual Studio 8 2005
  * Visual Studio 9 2008
  * Visual Studio 10 2010
  * Visual Studio 11 2012
  * Visual Studio 12 2013
  * Xcode

#### 额外的生成器

这些生成器可以指定是命令行还是集成开发环境。

支持的生成器包括：

 * CodeBlocks
 * CodeLite
 * Eclipse CDT4
 * KDevelop3
 * Kate
 * Sublime Text 2

Note：对于linux系统使用`sudo apt-get install ninja-build`命令来安装ninja

Note：对于MaxOS系统使用`brew intall Ninja`来安装ninja

### 分配生成器

使用-G指令来分配生成器，例如：

```bash
cmake .. -G Ninja
```

当执行完以上CMake后会生成必要的Ninja构建文件，其会在运行ninja指令后被运行。

```bash
$ cmake .. -G Ninja

$ ls

build.ninja  CMakeCache.txt  CMakeFiles  cmake_install.cmake  rules.ninja
```



# 构建案例

以下是构建案例的输出：

```bash
$ mkdir build.ninja

$ cd build.ninja/

$ cmake .. -G Ninja
-- The C compiler identification is GNU 4.8.4
-- The CXX compiler identification is GNU 4.8.4
-- Check for working C compiler using: Ninja
-- Check for working C compiler using: Ninja -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler using: Ninja
-- Check for working CXX compiler using: Ninja -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/J-building-with-ninja/build.ninja

$ ninja -v
[1/2] /usr/bin/c++     -MMD -MT CMakeFiles/hello_cmake.dir/main.cpp.o -MF "CMakeFiles/hello_cmake.dir/main.cpp.o.d" -o CMakeFiles/hello_cmake.dir/main.cpp.o -c ../main.cpp
[2/2] : && /usr/bin/c++      CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake  -rdynamic && :

$ ls
build.ninja  CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_cmake  rules.ninja

$ ./hello_cmake
Hello CMake!
```

