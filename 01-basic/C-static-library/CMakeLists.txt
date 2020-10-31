# 设置最低支持的CMake版本号
cmake_minimum_required(VERSION 3.5)

# 创建项目名称
project(hello_library)


################################################################
# 创建静态库
################################################################
add_library(hello_library
    STATIC
        src/Hello.cpp
)

# 静态库头文件目录
target_include_directories(hello_library
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)

################################################################
# 创建可执行文件
################################################################
add_executable(hello_binary
    src/main.cpp
)

# 将静态库链接到可执行文件上
target_link_libraries(hello_binary
    PRIVATE
        hello_library
)