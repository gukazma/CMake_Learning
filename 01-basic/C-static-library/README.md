# 静态库
# 介绍
这个案例将首次创建和链接静态库。这个简单的案例展示的是库和二进制文件在同一个目录下的情况，但是更一般的情况是在[02-sub-projects](https://github.com/xhguleixin123/cmake-examples/blob/master/02-sub-projects)
教程文件目录如下：

```bash
$ tree
.
├── CMakeLists.txt
├── include
│   └── static
│       └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/C-static-library/CMakeLists.txt) - 包含了所有需要执行的CMake指令
- [include/static/Hello.h](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/C-static-library/include/static/Hello.h) -头文件目录
- [src/Hello.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/C-static-library/src/Hello.cpp) - 需要编译的源码
- [src/main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/C-static-library/src/main.cpp) - main函数源码

# 概念

## 添加静态库

add_library()函数被用于从源码文件中创建l一个库：

```cmake
add_library(hello_library STATIC
    src/Hello.cpp
)
```

这条指令将会从源码中创建一个名为libhello_library.a的静态库

> 注解：正如前面的教程所提到的，现代CMake推荐直接将源码文件传送到add_xx函数中

## 填充头文件目录

本案例中，我们将使用target_include_directories()函数为静态库添目标添加头文件目录，并且使用PUBLIC表明范围。

```cmake
target_include_directories(hello_library
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)
```

这将使得头文件目录被用于以下场景：

* 编译库文件时
* 编译其他链接这个库的目标源时

范围的含义：

* PRIVATE - 只是将目录添加到目标的include目录下
* INTERFACE - 只是将目录添加到链接这个库的目标的include目录下
* PUBLIC - 不仅仅会将目录添加到链接这个库的目标的include目录下，而且会将目录添加到编译这个库时需要的include目录中。

> 注解：对于PBULIC的头文件目录，最好在头文件引用时添加次级目的名。
>
> target_include_directories 会将include目录树的根目录传递个target，所以你的C++文件源码的头文件所引应该包含头文件路径
>
> 例如：
>
> ```cpp
> #include "static/Hello.h"
> ```
>
> 使用这种方法可以很好的避免头文件文件名冲突



## 链接一个库

当创建可执行文件时会使用到库文件，所以你必须把库高速给编译器。使用target_link_libraries()函数

```cmake
add_executable(hello_binary
    src/main.cpp
)

target_link_libraries( hello_binary
    PRIVATE
        hello_library
)
```

这回告诉CMake在编译时，将hello_library链接到hollo_binary上。同时也会传递库目标的PUBLIC和INTERFACE作用域的头文件目录。

这个案例会使用如下的编译器参数：

```bash
/usr/bin/c++ CMakeFiles/hello_binary.dir/src/main.cpp.o -o hello_binary -rdynamic libhello_library.a
```

# 构建案例

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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/C-static-library/build

$ make
Scanning dependencies of target hello_library
[ 50%] Building CXX object CMakeFiles/hello_library.dir/src/Hello.cpp.o
Linking CXX static library libhello_library.a
[ 50%] Built target hello_library
Scanning dependencies of target hello_binary
[100%] Building CXX object CMakeFiles/hello_binary.dir/src/main.cpp.o
Linking CXX executable hello_binary
[100%] Built target hello_binary

$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_binary  libhello_library.a  Makefile

$ ./hello_binary
Hello Static Library!
```

# 错误

```cmake
target_include_directories(hello_library
    PUBLIC 
        $(PROJECT_SOURCE_DIR)/include  # 这里应该是大括号不是圆括号
)

cmake_minimum_required(VERSION) # 忘记标版本号
```

