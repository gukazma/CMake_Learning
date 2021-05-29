# 介绍

CMake 允许使用别名链接第三方库。

本教材的文件结构如下:

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 包含所要运行的CMake指令
  * main.cpp - 源文件

# 需求

本案例需求如下：

  * CMake v3.5+
  * 安装在系统默认位置的Boost库

# 概述

## 输入目标

输入目标是一个由FindXXX模块输出的只读目标。

你可以使用以下方式包含boost的filesystem模块

```cmake
target_link_libraries( imported_targets
      PRIVATE
          Boost::filesystem
)  
```

这会自动链接Boost::filesystem和Boost::system库，同时也会包含Boost的include文件。

# 构建案例

```cmake
$ mkdir build

$ cd build/

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
-- Boost version: 1.58.0
-- Found the following Boost libraries:
--   filesystem
--   system
boost found
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/K-imported-targets/build

$ make
Scanning dependencies of target imported_targets
[ 50%] Building CXX object CMakeFiles/imported_targets.dir/main.cpp.o
[100%] Linking CXX executable imported_targets
[100%] Built target imported_targets

$ ./imported_targets
Hello Third Party Include!
Path is not relative
```

