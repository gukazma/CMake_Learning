# 构建类型

# 介绍

在编译你的项目时，CMake有很多的构建配置。这会指定优化等级和是否将debug信息包含在二进制文件中。

优化等级包括：

* Release - 添加-03 -DNDEBUG标签到编译器
* Debug - 添加 -g 标签
* MinSizeRel - 添加-0s -DNDEBUG
* RelWithDebInfo - 添加 -02 -g -DNDEBUG 标签

以下时文件结构

```bash
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

- [CMakeLists.txt](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/F-build-type/CMakeLists.txt) - 包含了需要运行的CMake指令
- [main.cpp](https://github.com/xhguleixin123/cmake-examples/blob/master/01-basic/F-build-type/main.cpp) - main函数源文件

# 概念

## 设置构建类型

可以使用以下方法设置构建类型

* 使用cmake-gui

![cmake_gui](cmake-gui-build-type.png)

* 使用cmake指令

```bash
cmake .. -DCMAKE_BUILD_TYPE=Release
```

## 设置默认的构建类型

CMake提供的默认的构建种类时没有优化标记的。对于某些项目可以设置默认的构建种类，以免你不需要在去设置它。

添加以下命令到CMakeLists.txt的顶部

```cmake
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message("Setting build type to 'RelWithDebInfo' as none was specified.")
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Choose the type of build." FORCE)
  # 设置CMake-gui可以设置的构建种类值
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
    "MinSizeRel" "RelWithDebInfo")
endif()
```

# 构建案例

```bash
$ mkdir build

$ cd build/

$ cmake ..
Setting build type to 'RelWithDebInfo' as none was specified.
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build

$ make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/F-build-type -B/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
make -f CMakeFiles/cmake_examples_build_type.dir/build.make CMakeFiles/cmake_examples_build_type.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
cd /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/F-build-type /home/matrim/workspace/cmake-examples/01-basic/F-build-type /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/DependInfo.cmake --color=
Dependee "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/DependInfo.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/depend.internal".
Dependee "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/depend.internal".
Scanning dependencies of target cmake_examples_build_type
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
make -f CMakeFiles/cmake_examples_build_type.dir/build.make CMakeFiles/cmake_examples_build_type.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles 1
[100%] Building CXX object CMakeFiles/cmake_examples_build_type.dir/main.cpp.o
/usr/bin/c++    -O2 -g -DNDEBUG   -o CMakeFiles/cmake_examples_build_type.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/F-build-type/main.cpp
Linking CXX executable cmake_examples_build_type
/usr/bin/cmake -E cmake_link_script CMakeFiles/cmake_examples_build_type.dir/link.txt --verbose=1
/usr/bin/c++   -O2 -g -DNDEBUG    CMakeFiles/cmake_examples_build_type.dir/main.cpp.o  -o cmake_examples_build_type -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles  1
[100%] Built target cmake_examples_build_type
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles 0$ mkdir build
$ cd build/
/build$ cmake ..
Setting build type to 'RelWithDebInfo' as none was specified.
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
-- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build
/build$ make VERBOSE=1
/usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/F-build-type -B/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
make -f CMakeFiles/cmake_examples_build_type.dir/build.make CMakeFiles/cmake_examples_build_type.dir/depend
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
cd /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/F-build-type /home/matrim/workspace/cmake-examples/01-basic/F-build-type /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/DependInfo.cmake --color=
Dependee "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/DependInfo.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/depend.internal".
Dependee "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles/cmake_examples_build_type.dir/depend.internal".
Scanning dependencies of target cmake_examples_build_type
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
make -f CMakeFiles/cmake_examples_build_type.dir/build.make CMakeFiles/cmake_examples_build_type.dir/build
make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles 1
[100%] Building CXX object CMakeFiles/cmake_examples_build_type.dir/main.cpp.o
/usr/bin/c++    -O2 -g -DNDEBUG   -o CMakeFiles/cmake_examples_build_type.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/F-build-type/main.cpp
Linking CXX executable cmake_examples_build_type
/usr/bin/cmake -E cmake_link_script CMakeFiles/cmake_examples_build_type.dir/link.txt --verbose=1
/usr/bin/c++   -O2 -g -DNDEBUG    CMakeFiles/cmake_examples_build_type.dir/main.cpp.o  -o cmake_examples_build_type -rdynamic
make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles  1
[100%] Built target cmake_examples_build_type
make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/F-build-type/build'
/usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/F-build-type/build/CMakeFiles 0
```

