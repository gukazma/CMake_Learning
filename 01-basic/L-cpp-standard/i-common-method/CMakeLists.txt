cmake_minimum_required(VERSION 2.8)

project(hello_cpp11)

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++1" COMPILE_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILE_SUPPORTS_CXX0X)

if(COMPILE_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILE_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler")
endif()

add_executable(hello_cpp11 main.cpp)