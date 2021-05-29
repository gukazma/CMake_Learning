# 使用变量设置c++标准

# 介绍

使用`target_compile_features`函数来指定c++标准，支持自CMake v3.1。

本教材的文件结构如下:

```
$ tree
.
├── CMakeLists.txt
├── main.cpp
```

  * CMakeLists.txt - 包含所要运行的CMake指令
  * main.cpp - 源文件

# 概述

调用`target_compile_features`将会根据传递的参数，来设置对于目标最适合的编译标志。

```cmake
target_compile_features(hello_cpp11 PUBLIC cxx_auto_type)
```

使用`target_*`之类的函数，会指定你的目标的特征域，这会填充`INTERFACE_COMPILE_FEATURES`属性。

特征列表可以从`CMAKE_CXX_COMPILE_FEATURES`中找到，你可以通过以下命令获取：

```cmake	
message("List of compile features: ${CMAKE_CXX_COMPILE_FEATURES}")
```

# 构建案例

构建案例的输出如下：

```cmake
$ mkdir build
$ cd build

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
List of compile features: cxx_template_template_parameters;cxx_alias_templates;cxx_alignas;cxx_alignof;cxx_attributes;cxx_auto_type;cxx_constexpr;cxx_decltype;cxx_decltype_incomplete_return_types;cxx_default_function_template_args;cxx_defaulted_functions;cxx_defaulted_move_initializers;cxx_delegating_constructors;cxx_deleted_functions;cxx_enum_forward_declarations;cxx_explicit_conversions;cxx_extended_friend_declarations;cxx_extern_templates;cxx_final;cxx_func_identifier;cxx_generalized_initializers;cxx_inheriting_constructors;cxx_inline_namespaces;cxx_lambdas;cxx_local_type_template_args;cxx_long_long_type;cxx_noexcept;cxx_nonstatic_member_init;cxx_nullptr;cxx_override;cxx_range_for;cxx_raw_string_literals;cxx_reference_qualified_functions;cxx_right_angle_brackets;cxx_rvalue_references;cxx_sizeof_member;cxx_static_assert;cxx_strong_enums;cxx_thread_local;cxx_trailing_return_types;cxx_unicode_literals;cxx_uniform_initialization;cxx_unrestricted_unions;cxx_user_literals;cxx_variadic_macros;cxx_variadic_templates;cxx_aggregate_default_initializers;cxx_attribute_deprecated;cxx_binary_literals;cxx_contextual_conversions;cxx_decltype_auto;cxx_digit_separators;cxx_generic_lambdas;cxx_lambda_init_captures;cxx_relaxed_constexpr;cxx_return_type_deduction;cxx_variable_templates
-- Configuring done
-- Generating done
-- Build files have been written to: /data/code/01-basic/L-cpp-standard/iii-compile-features/build


$ make VERBOSE=1
/usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/iii-compile-features -B/data/code/01-basic/L-cpp-standard/iii-compile-features/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
cd /data/code/01-basic/L-cpp-standard/iii-compile-features/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/iii-compile-features /data/code/01-basic/L-cpp-standard/iii-compile-features /data/code/01-basic/L-cpp-standard/iii-compile-features/build /data/code/01-basic/L-cpp-standard/iii-compile-features/build /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
Dependee "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Dependee "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles/hello_cpp11.dir/depend.internal".
Scanning dependencies of target hello_cpp11
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
make[2]: Entering directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
[ 50%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
/usr/bin/c++     -std=gnu++11 -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/iii-compile-features/main.cpp
[100%] Linking CXX executable hello_cpp11
/usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
/usr/bin/c++      CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
make[2]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
[100%] Built target hello_cpp11
make[1]: Leaving directory '/data/code/01-basic/L-cpp-standard/iii-compile-features/build'
/usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/iii-compile-features/build/CMakeFiles 0
```

