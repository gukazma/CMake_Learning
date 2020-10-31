# 动态库

# 介绍

创建和链接共享库

创建别名目标

文件结构

```bash
$ tree
.
├── CMakeLists.txt
├── include
│   └── shared
│       └── Hello.h
└── src
    ├── Hello.cpp
    └── main.cpp
```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/D-shared-library/CMakeLists.txt) - 所有需要运行的CMake指令
- [include/shared/Hello.h](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/D-shared-library/include/shared/Hello.h) - 头文件
- [src/Hello.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/D-shared-library/src/Hello.cpp) - 需要编译的源码
- [src/main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/D-shared-library/src/main.cpp) - 主函数源码

# 概念

## 添加动态库

和静态库案例一样，add_library()函数也会从源码中创建一个动态库。

```cmake
add_library(hello_library SHARED
    src/Hello.cpp
)
```

这回产生一个名字为libhello_library.so的动态库文件

## 别名目标（Alias Target）

顾名思义，Alias Target就是为目标提供一个额外的名字，可以以只读的形式代替真实的目标（别名无法修改）

```cmake
add_library(hello::library ALIAS hello_library)
```

这运行你可以使用别名将库链接到目标上

## 链接动态库

链接一个动态库和链接静态库相同

```cmake
add_executable(hello_binary
    src/main.cpp
)

target_link_libraries(hello_binary
    PRIVATE
        hello::library
)
```

这个命令使用库别名会将hello_library链接到hello_binary的可执行文件上

这个例子会呼叫这个链接

```bash
/usr/bin/c++ CMakeFiles/hello_binary.dir/src/main.cpp.o -o hello_binary -rdynamic libhello_library.so -Wl,-rpath,/home/matrim/workspace/cmake-examples/01-basic/D-shared-library/build
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/D-shared-library/build

$ make
Scanning dependencies of target hello_library
[ 50%] Building CXX object CMakeFiles/hello_library.dir/src/Hello.cpp.o
Linking CXX shared library libhello_library.so
[ 50%] Built target hello_library
Scanning dependencies of target hello_binary
[100%] Building CXX object CMakeFiles/hello_binary.dir/src/main.cpp.o
Linking CXX executable hello_binary
[100%] Built target hello_binary

$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_binary  libhello_library.so  Makefile

$ ./hello_binary
Hello Shared Library!
```

# 错误

```cmake
target_link_libraries(hello_library # 这里的目标写成了hello_library，应该是hello_binary
    PRIVATE
        hello::library
)
```

