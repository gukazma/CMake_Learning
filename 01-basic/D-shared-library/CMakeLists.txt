# 设定最低的CMake版本
cmake_minimum_required(VERSION 3.5)

# 设定项目名
project(hello_library)

################################################################
# 创建库
################################################################
# 从源码中生成动态库
add_library(hello_library SHARED 
    src/Hello.cpp
)

# 动态库别名
add_library(hello::library ALIAS hello_library)

# 为动态库添加头文件目录
target_include_directories(hello_library
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/include
)
################################################################
# 创建可执行文件
################################################################
# 从源码中生成可执行文件
add_executable(hello_binary
    src/main.cpp
)

# 将动态库链接到可执行文件
target_link_libraries(hello_binary
    PRIVATE
        hello::library
)