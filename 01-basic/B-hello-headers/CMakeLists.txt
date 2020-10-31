# 设置支持的最低cmake版本
cmake_minimum_required(VERSION 3.5)

# 设置项目名称
project(hello_headers)

# 将所有需要编译的cpp文件保持到一个变量中
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

# 将源码添加到可执行文件中
add_executable(${PROJECT_NAME} ${SOURCES})

# 为目标工程添加头文件目录
target_include_directories(hello_headers
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)