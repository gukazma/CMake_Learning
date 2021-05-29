cmake_minimum_required(VERSION 3.1)

project(hello_cpp11)

add_executable(hello_cpp11 main.cpp)

target_compile_features(hello_cpp11 PUBLIC cxx_auto_type)

message("List of compile features : ${CMAKE_CXX_COMPILE_FEATURES}")