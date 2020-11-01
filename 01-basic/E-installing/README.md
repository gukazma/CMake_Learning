# installing

make intall 命令会将二进制文件、头文件、库文件等安装到指定位置

# 介绍

本案例将会展示如何生成一个make install 目标来安装文件和二进制文件在你的系统上。（基于之前的动态库）

文件如下：

```bash
$ tree
.
├── cmake-examples.conf
├── CMakeLists.txt
├── include
│   └── installing
│       └── Hello.h
├── README.adoc
└── src
    ├── Hello.cpp
    └── main.cpp
```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/E-installing/CMakeLists.txt) - 包含了需要执行的CMake指令
- [cmake-examples.conf](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/E-installing/cmake-examples.conf) - 案例配置文件
- [include/installing/Hello.h](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/E-installing/include/installing/Hello.h) - 头文件
- [src/Hello.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/E-installing/src/Hello.cpp) - 需要编译的源文件
- [src/main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/E-installing/src/main.cpp) - main函数源文件

# 概念

## installing

CMake提供了make install 目标，这允许用户将二级制文件、库文件等其他文件安装到指定目录中。安装目录是由变量 CMAKE_INSTALL_PREFIX 控制的，可以通过cmake .. -DCMAKE_INSTALL_PREFIX=/install/location来设置

安装文件是通过install()函数控制的

```cmake
install (TARGETS cmake_examples_inst_bin
    DESTINATION bin)
```

以上命令是将从目标cmake_examples_inst_bin产生的二进制文件安装到目标${CMAKE_INSTALL_PREFIX}/bin中

```cmake
install (TARGETS cmake_examples_inst
    LIBRARY DESTINATION lib)
```

以上命令是将从目标cmake_examples_inst产生的动态库安装到${CMAKE_INSTALL_PREFIX}/lib中

> 注解：这可能在windows系统中不起作用。在某些具有DLL目标的平台中，你可能需要添加以下命令：
>
> ```cmake
> install (TARGETS cmake_examples_inst
>     LIBRARY DESTINATION lib
>     RUNTIME DESTINATION bin)
> ```

```cmake
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
    DESTINATION include)
```

以上命令是将开发 cmake_examples_ins库t所用的头文件安装到 ${CMAKE_INSTALL_PREFIX}/include 目录下

```cmake
install (FILES cmake-examples.conf
    DESTINATION etc)
```

以上命令会将配置文件安装到目标  ${CMAKE_INSTALL_PREFIX}/etc下

在运行make install 之后，CMake会产生一个intall_manifest.txt 文件包含了所有安装文件的目录地址

> 注解：如果你以root身份执行make install 命令，install_manifest.txt 文件只有root用户在拥有

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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/E-installing/build

$ make
Scanning dependencies of target cmake_examples_inst
[ 50%] Building CXX object CMakeFiles/cmake_examples_inst.dir/src/Hello.cpp.o
Linking CXX shared library libcmake_examples_inst.so
[ 50%] Built target cmake_examples_inst
Scanning dependencies of target cmake_examples_inst_bin
[100%] Building CXX object CMakeFiles/cmake_examples_inst_bin.dir/src/main.cpp.o
Linking CXX executable cmake_examples_inst_bin
[100%] Built target cmake_examples_inst_bin

$ sudo make install
[sudo] password for matrim:
[ 50%] Built target cmake_examples_inst
[100%] Built target cmake_examples_inst_bin
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/bin/cmake_examples_inst_bin
-- Removed runtime path from "/usr/local/bin/cmake_examples_inst_bin"
-- Installing: /usr/local/lib/libcmake_examples_inst.so
-- Installing: /usr/local/etc/cmake-examples.conf

$ cat install_manifest.txt
/usr/local/bin/cmake_examples_inst_bin
/usr/local/lib/libcmake_examples_inst.so
/usr/local/etc/cmake-examples.conf

$ ls /usr/local/bin/
cmake_examples_inst_bin

$ ls /usr/local/lib
libcmake_examples_inst.so

$ ls /usr/local/etc/
cmake-examples.conf

$ LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib cmake_examples_inst_bin
Hello Install!
```

> 注解：如果/usr/local/lib 不是你库文件路径，在执行二进制文件前你可能需要将库文件添加到该路径。
>
> 本人注解：windows系统会直接在bin目录下生成dll动态库文件。

# 额外注解

## 重写默认安装位置

 CMAKE_INSTALL_PERF 的默认值是 /usr/local/

如果你向对所有用户修改默认安装位置，你可以添加以下命令到你的顶级CMakeLists.txt中（在添加任何二进制或者库之前）。

> 本人注解： message函数的消息类型
>
> ```cmake
> # (无) = 重要消息；
> # STATUS = 非重要消息；
> # WARNING = CMake 警告, 会继续执行；
> # AUTHOR_WARNING = CMake 警告 (dev), 会继续执行；
> # SEND_ERROR = CMake 错误, 继续执行，但是会跳过生成的步骤；
> # FATAL_ERROR = CMake 错误, 终止所有处理过程；
> # set CACHE FORCE 是设置全局变量，并且子项目的这个全局变量都是这个值
> # STRING 是变量类型
> # "The path to use for make install" 是变量注释注释
> set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
> # 设置好之后会会保持到生成的构建文件的CMakeLists.txt中，并且带注释。
> ```

```cmake
if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
  message(STATUS "Setting default CMAKE_INSTALL_PREFIX path to ${CMAKE_BINARY_DIR}/install") # STATUS是指消息类型
  set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
endif()
```





