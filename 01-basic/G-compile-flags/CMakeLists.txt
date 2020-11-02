# 设置CMake支持的最低版本
cmake_minimum_required(VERSION 3.5)

# 设置CMAKE_CXX_FLAGS的宏定义参数
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DEX2" CACHE STRING "Set C++ compiler Flags" FORCE)

# 设置项目名称
project(compile_flag)

# 添加可执行文件
add_executable(cmake_examples_compile_flags main.cpp)

# 对目标编译设置宏定义
# PUBLIC或者INTERFACE是所有链接到这个可执行文件的宏定义
# PRIVATE 只有当前可执行文件的宏定义
target_compile_definitions(cmake_examples_compile_flags
    PRIVATE EX3
)