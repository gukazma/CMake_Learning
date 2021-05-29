cmake_minimum_required(VERSION 3.5)

project (imported_targets)

find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)

if(Boost_FOUND)
    message ("boost found")
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()

add_executable(imported_targets main.cpp)

target_link_libraries( imported_targets
    PRIVATE
        Boost::filesystem
)