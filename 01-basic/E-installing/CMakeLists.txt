# 设置安装的目录为二进制文件目录
set( CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "Path used to install" FORCE)


# 设置最低支持的CMake版本
cmake_minimum_required(VERSION 3.5)

# 设置工程名
project(hello_install)

################################################################
# 创建动态库
################################################################

# 从源码中创建动态库
add_library(hello_inst_library
    SHARED
        src/Hello.cpp
)

# 将头文件目录添加到动态库中
target_include_directories(hello_inst_library
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)

################################################################
# 创建可执行文件
################################################################
# 从源码中创建可执行文件
add_executable(hello_inst
    src/main.cpp
)

# 将动态库链接到可执行文件
target_link_libraries(hello_inst
    PRIVATE
        hello_inst_library
)

################################################################
# 设置install
################################################################
install(TARGETS hello_inst
    DESTINATION
        bin
)

install (TARGETS hello_inst_library
    LIBRARY DESTINATION lib)

install(DIRECTORY ${CMAKE_SOURCE_DIR}/include
    DESTINATION
        include
)