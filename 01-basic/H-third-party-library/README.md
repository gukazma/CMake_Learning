# 介绍

对于较大型的项目，需要引入第三方库、头文件或程序。CMake已经支持使用`find_package()`函数来查找这些工具的路径。这会在`CMAKE_MODULE_PATH`指定的路径下，以"FindXXX.cmake"形式搜索CMake模块，  linux中默认的模块安装地址在 `/usr/share/cmake/Modules`. 对于我的系统，支持搜索147个常用第三方模块



本教程的文件结构如下:

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 你希望运行的CMake指令。
  * main.cpp - 主要源代码。

# 需要

这个案例需要你在系统本地安装Boost库。

# 概述

## 寻找包

如上所述使用`find_package()`函数来查找这些工具的路径。这会在`CMAKE_MODULE_PATH`指定的路径下，以"FindXXX.cmake"形式搜索CMake模块。`find_package`函数具体的参数依赖于你所需要搜索的包。  搜索形式被保存在 `FindXXX.cmake` 文件中。

一个基础案例如下:

```cmake
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)
```

参数:

  * Boost - 库名，用来搜索模块文件 FindBoost.cmake。
  * 1.46.1 - boost库的最低版本。
  * REQUIRED - 告诉模块库被需要，如果库没发现，就会返回失败。
  * COMPONENTS - 要寻找的库清单。

Boost可以使用更多参数，更复杂的包寻找会在后面进行介绍。


## 检查包是否被找到

`XXX_FOUND`变量被用来检查包是否被找到。

对于本案例来说，这个变量为Boost_FOUND`。

```cmake
if(Boost_FOUND)
    message ("boost found")
    include_directories(${Boost_INCLUDE_DIRS})
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()
```

## 输出变量

当包被发现后，会输出一些变量，来告诉用户库地址，头文件地址和可执行文件的地址。
类似于 `XXX_FOUND`变量。这些变量被指定在`FindXXX.cmake` 文件中。

本案例中的输出变量包括:

  * `Boost_INCLUDE_DIRS` - boost头文件目录。

## 化名/输入变量

大多数现代化的CMake库会输出一个化名到模型文件中，这个输入目标的好处是方便包含和连接这个库。

例如，对于v3.5+版本的CMake支持这个，类似于使用你自己库的化名目标，化名在模块中使得参考找到目标更加方便。

以Boost为例，所有的目标都会输出到`Boost::`这个命名空间中，然后加上子系统名。

  * `Boost::boost` 库的头文件。
  * `Boost::system` boost的system库。
  * `Boost::filesystem` filesystem库。

和你的目标一样，这些目标包含了他们的依赖，所以当连接`Boost::filesystem`会自动添加`Boost::boost`和`Boost::system`依赖。

使用以下命令以使用输入目标：  

## 非化名目标

即使大多数现代化的库使用输入目标，但并不是所有的模块被更新。当一个库没有被更新时，你将会发现以下变量可供使用：

  * xxx_INCLUDE_DIRS - 一个变量指向库的include目标。
  * xxx_LIBRARY - 一个变量指向库的目录。

这些变量可以被添加到target_include_directories和target_link_libraries函数中，如下:

```cmake
# Include the boost headers

target_include_directories( third_party_include
    PRIVATE ${Boost_INCLUDE_DIRS}
)

# link against the boost libraries

target_link_libraries( third_party_include
    PRIVATE
    ${Boost_SYSTEM_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}

)
```

# Building the Example

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
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/H-third-party-library/build

$ make
Scanning dependencies of target third_party_include
[100%] Building CXX object CMakeFiles/third_party_include.dir/main.cpp.o
Linking CXX executable third_party_include
[100%] Built target third_party_include
matrim@freyr:~/workspace/cmake-examples/01-basic/H-third-party-library/build$ ./
CMakeFiles/          third_party_include
matrim@freyr:~/workspace/cmake-examples/01-basic/H-third-party-library/build$ ./third_party_include
Hello Third Party Include!
Path is not relative
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
-- Boost version: 1.54.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/H-third-party-library/build

$ make
Scanning dependencies of target third_party_include
[100%] Building CXX object CMakeFiles/third_party_include.dir/main.cpp.o
Linking CXX executable third_party_include
[100%] Built target third_party_include

$ ./third_party_include
Hello Third Party Include!
Path is not relative
```

